1.取得HTML elements

     const triggers = document.querySelectorAll('.cool > li'); // nav bar 裡面的選項
     const background = document.querySelector('.dropdownBackground'); // 下拉對話框的白色背景
     const nav = document.querySelector('.top'); // nav bar

2.追蹤滑鼠是否進入nav bar選項範圍

     function handleEnter() {
          console.log('ENTER!');
     }

     function handleLeave() {
          console.log('LEAVE!');
     }

     triggers.forEach(trigger => trigger.addEventListener('mouseenter', handleEnter));
     triggers.forEach(trigger => trigger.addEventListener('mouseleave', handleLeave));

3.當滑鼠進入nav bar選項範圍，則顯示相對應內容

     CSS code:
          // 原始狀態
          .dropdown {
               opacity: 0; // 完全透明
               position: absolute;
               overflow: hidden;
               padding:20px;
               top:-20px;
               border-radius:2px;
               transition: all 0.5s; // 第一層變成第二層時的轉換時間
               transform: translateY(100px);
               will-change: opacity;
               display: none; // 隱藏不顯示
          }

          // 第一層由隱藏改為顯示
          .trigger-enter .dropdown {
               display: block;
          }

          // 第二層由透明改為不透明
          .trigger-enter-active .dropdown {
               opacity: 1;
          }

     JavaScript code:
          function handleEnter() {
               this.classList.add('trigger-enter'); // 第一層由隱藏改為顯示

               // 第二層由透明改為不透明，設定延遲150ms後執行，才有轉換動畫效果
               // 寫法一 (錯誤)
               setTimeout(function() { // 使用一般function的話，this會變成Window
                    console.log(this); //Window
                    this.classList.add('trigger-enter-active');
               }, 150);

               // 寫法二 (正確)
               setTimeout(() => {
                    console.log(this); //trigger element
                    this.classList.add('trigger-enter-active');
               }, 150);
          }

4.當滑鼠離開nav bar選項範圍，則隱藏相對應內容

     function handleLeave() {
          this.classList.remove('trigger-enter', 'trigger-enter-active');
     }

5.加入內容的白色背景

     CSS code:
          .dropdownBackground {
               width:100px;
               height:100px;
               position: absolute;
               background: #fff;
               border-radius: 4px;
               box-shadow: 0 50px 100px rgba(50,50,93,.1), 0 15px 35px rgba(50,50,93,.15), 0 5px 15px rgba(0,0,0,.1);
               transition:all 0.3s, opacity 0.1s, transform 0.2s;
               transform-origin: 50% 0;
               display: flex;
               justify-content: center;
               opacity:0; // 完全透明

          }

          .dropdownBackground.open {
               opacity: 1;
          }

     JavaScript code:
          function handleEnter() {
               this.classList.add('trigger-enter');
               setTimeout(() => {
                    this.classList.add('trigger-enter-active');
               }, 150);

               background.classList.add('open');
          }

6.動態調整背景的大小及位置

     function handleEnter() {
          this.classList.add('trigger-enter');
          setTimeout(() => {
               this.classList.add('trigger-enter-active');
          }, 150);
          background.classList.add('open');

          const dropdown = this.querySelector('.dropdown'); // 取得相對應li下方的dropdown

          const dropdownCoords = dropdown.getBoundingClientRect(); // 取得dropdown的大小及位置

          const navCoords = nav.getBoundingClientRect(); // 取得nav bar的大小及位置

          // 計算background的大小及位置

          const coords = {
               height: dropdownCoords.height, // height與dropdown相同

               width: dropdownCoords.width, // width與dropdown相同
               // top需抵銷掉nav的top(目前為0), 否則如果nav上面有其他element(假設為20px)就會亂掉(往下移了20px, 需調整回去)

               top: dropdownCoords.top - navCoords.top,
               // left同理

               left: dropdownCoords.left - navCoords.left
          };

          // 設定background的大小及位置CSS code

          background.style.setProperty('height', `${coords.height}px`);
          background.style.setProperty('width', `${coords.width}px`);
          background.style.setProperty('transform', `translate(${coords.left}px, ${coords.top}px)`);
     }

7.當滑鼠離開nav bar選項範圍，則隱藏背景

     function handleLeave() {
          this.classList.remove('trigger-enter', 'trigger-enter-active');
          background.classList.remove('open');
     }

8.解決延遲顯示內容所造成的問題

     當滑鼠移動得很快的時候，進入nav bar還沒到150ms就離開，則會在離開後才加上 trigger-enter-active class，造成錯誤
     所以必須在加上trigger-enter-active class前先判斷是否需要加

     setTimeout(() => {
          if (this.classList.contains('trigger-enter'))
               this.classList.add('trigger-enter-active');
     }, 150);

     // 寫法二：更簡潔
     setTimeout(() => this.classList.contains('trigger-enter') && this.classList.add('trigger-enter-active'), 150);