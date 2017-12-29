1.取得HTML elements

     const speed = document.querySelector('.speed'); // 速度條整體
     const bar = document.querySelector('.speed-bar'); // 調整速度的拉霸
     const video = document.querySelector('.flex');

2.加入eventListener監聽在速度條上的滑鼠移動

     function handleMove(e) {
          console.log(e);
     }

     speed.addEventListener('mousemove', handleMove);

3.計算目前滑鼠滑到的高度，設定拉霸及影片速度

     function handleMove(e) {
          const y = e.pageY - this.offsetTop; // 減去速度條的最高點高度，若有其他element才不會出錯
          const percent = y / this.offsetHeight;  // 計算相對應的百分比
          const min = 0.4;
          const max = 4;
          const height = Math.round(precent * 100) + '%'; // 取得拉霸應有的高度
          const playbackRate = percent * (max - min) + min; // 取得影片的速度(在min及max之間相對應比例的數值)
          bar.style.height = height; // 設定拉霸高度
          bar.textContent = playbackRate.toFixed(2) + 'x'; // 設定拉霸顯示數字(只取到小數點後兩位)
          video.playbackRate = playbackRate; // 設定影片速度(不用取到特定位數)
     }