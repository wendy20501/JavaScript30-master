1.取得HTML elements

     const msg = new SpeechSynthesisUtterance(); // 發聲元件
     let voices = [];
     const voicesDropdown = document.querySelector('[name="voice"]');
     const options = document.querySelectorAll('[type="range"], [name="text"]');
     const speakButton = document.querySelector('#speak');
     const stopButton = document.querySelector('#stop');

2.將輸入的文字設定為發聲的內容

     msg.text = document.querySelector('[name="text"]').value;

3.取得聲音種類清單

     .addEventListener('voiceschanged', populateVoices); // 一開始就會trigger

     function populateVoices() {
          voices = this.getVoices(); // 取得所有聲音種類(mac 較多, windows 較少)
          console.log(voices);
          voicesDropdown.innerHTML = voices
               .filter(voice => voice.lang.includes('en'))  // 取得英文發音的聲音
               .map(voice => `<option value="${voice.name}">${voice.name} (${voice.lang})</option>`)
               .join('');
     }

4.將發聲元件設定為選定的聲音

     voicesDropdown.addEventListener('change', setVoice); // 當dropdown list選到新的選項就trigger

     function setVoice() {
          msg.voice = voices.find(voice => voice.name === this.value);
     }

5.讓發聲元件發聲

     function toggle(startOver = true) {
          speechSynthesis.cancel(); // 取消先前的發聲
          if (startOver) {
               speechSynthesis.speak(msg); // 開始發聲
          }
     }

     // 選完聲音就發聲

     function setVoice() {
          msg.voice = voices.find(voice => voice.name === this.value);
          toggle();
     }

     // 按下"speak"按鈕就發聲
     speakButton.addEventListener('click', toggle);

     // 按下"stop"按鈕就停止發聲(三種寫法)
     // 1)
          stopButton.addEventListener('click', toggle.bind(null, false));

     // 2)
          stopButton.addEventListener('click', function() {
               toggle.(false)
          });

     // 3)
          stopButton.addEventListener('click', () => toggle(false));

6.調整音速與音調

     function setOption() {
          msg[this.name] = this.value;
          toggle();
     }

     options.forEach(option => option.addEventListener('change', setOption));