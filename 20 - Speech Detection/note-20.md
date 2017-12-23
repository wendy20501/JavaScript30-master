1.前置作業：

     開啟一個localhost server
     npm install
     npm start

     允許browser使用麥克風

2.取得語音辨識

     window.SpeechRecognition = window.SpeechRecognition || window.webkitSpeechRecognition;

     const recognition = new SpeechRecognition(); // the controller interface for the recognition service
     recognition.interimResults = true; // Controls whether interim results should be returned (true) or not (false.) Interim results are results that are not yet final

3.監聽語音辨識

     recognition.addEventListener('result', e => { // 有result則觸發
          console.log(e.results);
     });

     recognition.start(); // 開始監聽

4.擷取需要的資料

     // 可以發現我們要的是nested list 中 result[0]的transcript的資料

     recognition.addEventListener('result', e => {
          const transcript = Array.from(e.results) // 將results存成Array的型態
               .map(result => result[0])
               .map(result => result.transcript)
               .join('');

          console.log(transcript);
     });

5.循環開啟語音辨識(一次語音辨識完再繼續開啟)

     recognition.addEventListener('end', recognition.start);

6.將取得的資料顯示在網頁中

     // 先生成一個paragraph的element為初始值
     let p = document.createElement('p');
     const words = document.querySelector('.words');
     words.appendChild(p);

     recognition.addEventListener('result', e => {
          const transcript = Array.from(e.results)
               .map(result => result[0])
               .map(result => result.transcript)
               .join('');

          p.textContent = transcript; // 將取得的資料寫入paragraph

          if (e.results[0].isFinal) { // 判斷是否結束(暫時沒有input)
               p = document.createElement('p'); // 產生新的paragraph
               words.appendChild(p);
          }
     });

7.添加判斷, 可以做成Siri

     recognition.addEventListener('result', e => {
          const transcript = Array.from(e.results)
               .map(result => result[0])
               .map(result => result.transcript)
               .join('');

          p.textContent = transcript;

          if (e.results[0].isFinal) {
               p = document.createElement('p');
               words.appendChild(p);
          }

          if (transcript.includes('poop')) { // 當transcript包含什麼字詞的時候，可做相對應反應
               console.log('💩💩💩💩💩💩💩💩');
          }
     });