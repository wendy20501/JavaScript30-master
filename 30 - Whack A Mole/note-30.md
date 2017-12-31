1.選取HTML elements

     const holes = document.querySelectorAll('.hole'); // 每個洞
     const scoreBoard = document.querySelector('.score'); // 計分板
     const moles = document.querySelectorAll('.mole'); // 每隻地鼠

2.亂數取出時間

     function randomTime(min, max) { // 可藉由不同數值調整難度
          return Math.round(Math.random() * (max - min) + min);
     }

3.亂數選一個地鼠洞

     function randomHole(holes) {
          const idx = Math.floor(Math.random() * holes.length);
          return holes[idx]; // 測試後發現容易重複選取
     }

4.修正重複選取的問題

     let lastHole;

     function randomHole(holes) {
          const idx = Math.floor(Math.random() * holes.length);
          const hole = holes[idx];
          if (hole === lastHole) { // 判斷如果重複選取了
               console.log('Ah nah thats the same one bud');
               return randomHole(holes); // 就重新再選一個
          }
          lastHole = hole; // 紀錄上一個選取的洞

          return hole;
     }

5.讓選到的地鼠按照時間探出頭來

     function peep() {
          const time = randomTime(200, 1000);
          const hole = randomHole(holes);
          hole.classList.add('up'); // 地鼠探出頭
          setTimeout(() => { // 倒數計時到特定秒數
               hole.classList.remove('up');  // 地鼠躲起來
          }, time);
     }

6.讓peep function接續進行，直到時間到為止

     let timeup = false;  // 設定遊戲限定時間

     function peep() {
          const time = randomTime(200, 1000);
          const hole = randomHole(holes);
          hole.classList.add('up');
          setTimeout(() => {
               hole.classList.remove('up');
               if (!timeup)  // 當時間還沒到就繼續peep
                    peep();
          }, time);
     }

     function startGame() {
          timeup = false; // 設定timeup初始值
          peep();
          setTimeout(() => timeup = true, 10000); // 10秒後時間到，peep會停止
     }

7.打到地鼠要計分

     let score = 0; // 記錄分數

     function startGame() {
          timeup = false;
          scoreBoard.textContent = 0;  // 計分板初始值
          score = 0; // 分數初始值
          peep();
          setTimeout(() => timeup = true, 10000);
     }

     function bonk(e) {
          if (!e.isTrusted) // cheater! 用JavaScript模擬滑鼠點擊的動作
               return;
          score++;
          this.classList.remove('up');  // 被打到的地鼠就躲起來啦
          scoreBoard.textContent = score; // 顯示分數
     }

     moles.forEach(mole => mole.addEventListener('click', bonk));  // 每當地鼠被打到就加分