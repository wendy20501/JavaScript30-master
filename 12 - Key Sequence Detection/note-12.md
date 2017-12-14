如果在網頁上key-in特定的密碼，會產生小驚喜

1.偵測key-in事件

     window.addEventListener('keyup', (e) => {
          console.log(e.key);
     });

2.設定密碼

     const secretCode = 'wesbos';

3.將key-in記錄下來

     const pressed = [];

     window.addEventListener('keyup', (e) => {
          pressed.push(e.key);
     });

4.從key-in Array最後擷取密碼的長度做判定

     window.addEventListener('keyup', (e) => {
          pressed.push(e.key);
          pressed.splice(-secretCode.length - 1, pressed.length - secretCode.length);
          // 負號為反向刪除 // 參數1: 開始的index // 參數2: 要刪除的長度
          if(pressed.join('').includes(secretCode)) {
               cornify_add(); // 隨機添加獨角獸與彩虹在頁面上
          }
     }) ;