1.取得HTML element

     const slider = document.querySelector('.items');

2.建立所需變數

     let isDown = false; // 記錄滑鼠是否按下
     let startX; // 按下滑鼠的x位置
     let scrollLeft;  // 目前滑動的距離

3.加入eventListener觀察滑鼠按下的狀態

     slider.addEventListener('mousedown', () => {
          isDown = true; // 按下滑鼠時紀錄為true
     });

     slider.addEventListener('mouseleave', () => {
          isDown = false; // 滑鼠離開物件時紀錄為false
     });

     slider.addEventListener('mouseup', () => {
          isDown = false; // 放開滑鼠時紀錄為false
     });

     slider.addEventListener('mousemove', (e) => {
          if (!isDown)    // 只在有按下滑鼠的時候滑動才有動作
               return;
     });

4.當滑鼠按下時改變CSS code

     CSS code:
          .items {
               height:800px;
               padding: 100px;
               width:100%;
               border:1px solid white;
               overflow-x: scroll;
               overflow-y: hidden;
               white-space: nowrap;
               user-select: none;
               cursor: pointer;
               transition: all 0.2s;
               transform: scale(0.98);
               will-change: transform;
               position: relative;
               background: rgba(255,255,255,0.1);
               font-size: 0;
               perspective: 500px;
          }

          .items.active {
               background: rgba(255,255,255,0.3); // 背景改半透明
               cursor: grabbing; // 游標改成手抓緊的圖案
               cursor: -webkit-grabbing;
               transform: scale(1); // 物件放大0.98->1
          }

     JavaScript code:

          slider.addEventListener('mousedown', (e) => {
               isDown = true;
               slider.classList.add('active');
          });

          slider.addEventListener('mouseleave', () => {
               isDown = false;
               slider.classList.remove('active');
          });

          slider.addEventListener('mouseup', () => {
               isDown = false;
               slider.classList.remove('active');
          });

5.記錄按下的x位置

     slider.addEventListener('mousedown', (e) => {
          isDown = true;
          slider.classList.add('active');
          startX = e.pageX - slider.offsetLeft;
          // e.pageX為滑鼠點擊頁面的x值, slider.offsetLeft為物件的左端x值
          // 為了避免因為其他物件或是margin讓物件的左端非而造成誤差即將他扣除
     });

6.當滑鼠按下時滑動滾輪，物件跟著滑動

     slider.addEventListener('mousemove', (e) => {
          if (!isDown)
               return;
          e.preventDefault(); // 取消預設動作(反白選取)
          const x = e.pageX - slider.offsetLeft; // 與startX取法一樣，取得目前位置
          const walk = x - startX; // 取得位移的距離
          slider.scrollLeft = -walk; // 將物件左端設為反向位移的距離
          // 問題點：一但滑鼠放開再點一次就又回到原點
     });

7.修正回到原點的問題，需記錄目前滑動的距離

     slider.addEventListener('mousedown', (e) => {
          isDown = true;
          slider.classList.add('active');
          startX = e.pageX - slider.offsetLeft;
          scrollLeft = slider.scrollLeft; // 記錄目前滑動的距離

     });

     slider.addEventListener('mousemove', (e) => {
          if (!isDown)
               return;
          e.preventDefault();
          const x = e.pageX - slider.offsetLeft;
          const walk = x - startX;
          slider.scrollLeft = scrollLeft - walk; // 由上次滑動的位置繼續滑動
     });