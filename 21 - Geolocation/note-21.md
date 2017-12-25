1.事前準備

     a.開啟server:
          npm install
          npm start

     b.啟用xcode simulator:

          使用safari開啟External IP address

          允許使用current location

     c.開啟電腦端的safiri
          如果選單裡沒有“開發”或"Develop"選項：
               把 Safari > 偏好設定 > 進階 > 在選單中顯示『開發』選單 打勾

          打開Simulator的console:
               開發 > Simulator > 你要開的頁面


2.取得HTML物件

     const arrow = document.querySelector('.arrow'); // 羅盤的箭頭
     const speed = document.querySelector('.speed-value');  // 當前速度的值

3.取得當前位置

     navigator.geolocation.watchPosition((data) => { // 使用watchPosition會不斷回傳數值，getCurrentPosition則只回傳一次
          console.log(data);
     });

4.顯示當前方位及速度

     navigator.geolocation.watchPosition((data) => {
          speed.textContent = data.coords.speed; // 取得移動速度值
          arrow.style.transform = `rotate(${data.coords.heading}deg)`; // 取得方位
     }, (err) => {
          console.log(err);
          alert('HEY! YOU GOTTA ALLOW THAT TO HAPPEN!!!');
     });