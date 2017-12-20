1.前置作業:

    // 需要用到Webcam所以必須在一個secured的server上跑, 使用nodeJS開設localhost server
    // 在該資料夾執行
    // npm install
    // npm start
    // 會自動在localhost開啟檔案
    package.json
    {
      "name": "gum",
      "version": "1.0.0",
      "description": "",
      "main": "scripts.js",
      "scripts": {
      "start": "browser-sync start --server --files \"*.css, *.html, *.js\""
      },
      "author": "",
      "license": "ISC",
      "devDependencies": {
      "browser-sync": "^2.12.5"
      }
    }

2.使用到的element

    const video = document.querySelector('.player');  // video element
    const canvas = document.querySelector('.photo');  // canvas element
    const ctx = canvas.getContext('2d'); // Context
    const strip = document.querySelector('.strip');  // captured photo location
    const snap = document.querySelector('.snap');  //  audio element (camera click sound)

3.取得video

     function getVideo() {
          navigator.mediaDevices.getUserMedia({video: true, audio: false}) // 取得video, 回傳值是一個promise
                  .then(localMediaStream => {
                            // console.log(localMediaStream); // 此為media stream object
                            video.src = window.URL.createObjectURL(localMediaStream); // 把object轉成URL(live video)
                            video.play();
                  })
                  .catch(err => {
                       console.error(`Oh No!!!`, err);
                  });
     }

4.取出一個frame, 將其畫在canvas上面

     function paintToCanvas() {
          const width = video.videoWidth; // 實際video的寬度
          const height = video.videoHeight; // 實際video的高度
          canvas.width = width;
          canvas.height = height;

          return setInterval(() => { // 定時執行
               ctx.drawImage(video, 0, 0, width, height); // 將video畫在畫布上
          }, 16); // 每16ms執行一次
     }

5.截圖的音效

    function takePhoto() {
        // played the sound
        snap.currentTime = 0;
        snap.play();
    }

6.當browser準備好播放影片時，執行paintToCanvas

     video.addEventListener('canplay', paintToCanvas);

7.截圖

     function takePhoto() {
          snap.currentTime = 0;
          snap.play();

          // take the data out of the canvas
          const data = canvas.toDataURL('image/jpeg');
          // 取得一個data URL, 把圖片的資料不是存在本機而是存在URL中, 開啟就是該圖片

          const link = document.createElement('a'); // 產生一個新的anchor element
          link.href = data;
          link.setAttribute('download', 'beauty'); // 可以下載，下載的話他的預設名稱會是beauty
          //link.textContent = 'Download Image';
          link.innerHTML = `<img src="${data}" alt="Beauty girl">`;
          strip.insertBefore(link, strip.firstChild); // 插入該element到strip的第一個child之前
     }

8.加入各種濾鏡

     function paintToCanvas() {
          const width = video.videoWidth;
          const height = video.videoHeight;
          canvas.width = width;
          canvas.height = height;

          return setInterval(() => {
               ctx.drawImage(video, 0, 0, width, height);

               // take the pixels out
               let pixels = ctx.getImageData(0, 0, width, height);

               // mess with them
               pixels = redEffect(pixels);  // 濾鏡 1

               pixels = rgbSplit(pixels);  // 濾鏡 2

               ctx.globalAlpha = 0.1;  // 調整透明度

               pixels = greenScreen(pixels); // 濾鏡 3


               // put them back
               ctx.putImageData(pixels, 0, 0);
          }, 16);
     }

     // 濾鏡 1 加強紅色
     function redEffect(pixels) {
          for (let i = 0; i < pixels.data.length; i += 4) { // 遍歷所有的pixel data (rgba共四個值)
               pixels.data[i + 0] = pixels.data[i + 0] + 100; // r
               pixels.data[i + 1] = pixels.data[i + 1] - 50; // g
               pixels.data[i + 2] = pixels.data[i + 2] * 0.5; // b
          }
          return pixels;
     }

     // 濾鏡 2 把r, g, b各自位移到不同的位置
     function rgbSplit(pixels) {
          for (let i = 0; i < pixels.data.length; i += 4) {
               pixels.data[i - 150] = pixels.data[i + 0]; // r
               pixels.data[i + 100] = pixels.data[i + 1]; // g
               pixels.data[i - 150] = pixels.data[i + 2]; // b
          }
          return pixels;
     }

     // 濾鏡 3 把某一範圍的顏色濾掉
     function greenScreen(pixels) {
          const levels = {}; // input的值

          document.querySelectorAll('.rgb input').forEach((input) => {
               levels[input.name] = input.value;
          });

          for (i = 0; i < pixels.data.length; i = i + 4) {
               red = pixels.data[i + 0];
               green = pixels.data[i + 1];
               blue = pixels.data[i + 2];
               alpha = pixels.data[i + 3];

               if (red >= levels.rmin
                    && green >= levels.gmin
                    && blue >= levels.bmin
                    && red <= levels.rmax
                    && green <= levels.gmax
                    && blue <= levels.bmax) {
                    // take it out!
                    pixels.data[i + 3] = 0; // 把alpha設為0, 就是把它變成透明的
               }
          }
          return pixels;
     }