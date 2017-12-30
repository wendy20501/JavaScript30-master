1.倒數計時的timer

     // 直覺寫法：用setInterval
     // 瀏覽器為了執行順暢，切換分頁後常會把interval停掉，就會出錯
     function timer(seconds) {
          setInterval(function() {
               seconds--;
         }, 1000);
     }

     // 修改後寫法：
     let countdown; // 宣告一變數來儲存Interval, 當倒數為0後可直接將其清除
     function timer(seconds) {
          const now = Date.now(); // 取得現在的時間(ms)
          const then = now + seconds * 1000; // 將seconds也換為ms

          setInterval(() => {
               const secondsLeft = Math.round((then - Date.now()) / 1000); // 剩餘秒數(整數)

               // check if we should stop it
               if (secondsLeft < 0) {
                    clearInterval(countdown); // 將interval清除
                    // 若沒有清除直接return的話，此interval雖然不會繼續印出時間，但還是會持續在背景執行
                    return;
               }

               // display it
               console.log(secondsLeft);
          }, 1000);
     }

2.修正沒有一開始就印出seconds的問題

     // 因為執行setInterval需要時間，雖不到一秒但是印出的第一個秒數就不是seconds

     let countdown;
     function timer(seconds) {
          const now = Date.now();
          const then = now + seconds * 1000;
          displayTimeLeft(seconds); // 初始就先印出seconds

          setInterval(() => {
               const secondsLeft = Math.round((then - Date.now()) / 1000);

               if (secondsLeft < 0) {
                    clearInterval(countdown);
                    return;
               }
               displayTimeLeft(secondsLeft); // Interval中每秒印一次
          }, 1000);
     }

     function displayTimeLeft(seconds) {
          console.log(display);
     }

3.將倒數秒數顯示於網頁中，非console.log

     const timerDisplay = document.querySelector('.display__time-left');

     function displayTimeLeft(seconds) {
          const minutes = Math.floor(seconds / 60);
          const remainderSeconds = seconds % 60;
          const display = `${minutes}:${remainderSeconds < 10? '0' : ''}${remainderSeconds}`; // 秒數小於10時在前面補0
          timerDisplay.textContent = display;
          document.title = display; // 在tab上面也顯示時間
     }

4.顯示預計結束的時間

     const endTime = document.querySelector('.display__end-time');

     function timer(seconds) {
          const now = Date.now();
          const then = now + seconds * 1000;
          displayTimeLeft(seconds);
          displayEndTime(then);

          setInterval(() => {
               const secondsLeft = Math.round((then - Date.now()) / 1000);

               if (secondsLeft < 0) {
                    clearInterval(countdown);
                    return;
               }
               displayTimeLeft(secondsLeft);
          }, 1000);
     }

     function displayEndTime(timestamp) {
          const end = new Date(timestamp);
          const hour = end.getHours();
          const minutes = end.getMinutes();
          endTime.textContent = `Be Back At ${hour > 12 ? hour - 12 : hour}:${minutes < 10 ? '0' : ''}${minutes}`;
          // 超過12點就扣掉12, 由1點開始顯示(美國顯示小時的方式)
     }

5.點擊相對應的按鈕要顯示相對應的倒數秒數

     HTML code:
          <div class="timer">
            <div class="timer__controls">
               <button data-time="20" class="timer__button">20 Secs</button>
               <button data-time="300" class="timer__button">Work 5</button>
               <button data-time="900" class="timer__button">Quick 15</button>
               <button data-time="1200" class="timer__button">Snack 20</button>
               <button data-time="3600" class="timer__button">Lunch Break</button>
               <form name="customForm" id="custom">
                    <input type="text" name="minutes" placeholder="Enter Minutes">
               </form>
          </div>

     JavaScript code:
          const buttons = document.querySelectorAll('[data-time]');

          function startTimer() {
               const seconds = parseInt(this.dataset.time); // 取得data-time的值
               timer(seconds);
          }

          buttons.forEach(button => button.addEventListener('click', startTimer));

6.輸入秒數後要啟動倒數 (見上方HTML code)

     document.customForm.addEventListener('submit', function(e) {
          // 小技巧：當可以直接HTML element有設定name property就可以直接用document選取，不用querySelector
          e.preventDefault(); // 不reload page
          const mins = this.minutes.value;  // 同上，直接取得minutes element
          timer(mins * 60);
          this.reset(); // 將輸入框內的值清掉
     })

7.修正點擊不同的button時會同時開啟多個timer造成混亂的問題

     function timer(seconds) {
          clearInterval(countdown); // clear any existing timers

          const now = Date.now();
          const then = now + seconds * 1000;
          displayTimeLeft(seconds);
          displayEndTime(then);

          setInterval(() => {
               const secondsLeft = Math.round((then - Date.now()) / 1000);

               if (secondsLeft < 0) {
                    clearInterval(countdown);
                    return;
               }
               displayTimeLeft(secondsLeft);
          }, 1000);
     }