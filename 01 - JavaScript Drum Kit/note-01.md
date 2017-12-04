1. 用css改變物件外觀：
    .playing {
      transform: scale(1.1);     //放大1.1倍
      border-color: #ffc600;
      box-shadow: 0 0 1rem #ffc600;
    }

    .key {
      border: .4rem solid black;
      border-radius: .5rem;
      margin: 1rem;
      font-size: 1.5rem;
      padding: 1rem .5rem;
      transition: all .07s ease; //變換特性的時候的變身時間 (all: 特性名稱, .07s: 秒數, ease:  specifies a transition effect with a slow start, then fast, then end slowly (this is default)
      width: 10rem;
      text-align: center;
      color: white;
      background: rgba(0,0,0,0.4);
      text-shadow: 0 0 .5rem black;
    }

    CSS3 transitions allows you to change property values smoothly (from one value to another), over a given duration.

2. 取得KeyCode的網站：http://keycode.info/

3.  HTML data-* Attributes
    The data-* attributes gives us the ability to embed custom data attributes on all HTML elements.

4. Listening up for key event
    window.addEventListener('keydown', function(e){
    ...
    });
    // e為event

5. Is there an audio element has data-key=65?
    const audio = document.querySelector(`audio[data-key="${e.keyCode}"]`)
    // document.querySelector: Returns the first Element within the document that matches the specified selector, or group of selectors, or null if no matches are found.

    `audio[data-key="${e.keyCode}"]`
    audio: tag name
    'audio[data-key="65"]' => 把"65"變成一個variable "${e.keyCode}", 最外層要從 ' 改為 `
    Template literals: Template literals are enclosed by the back-tick (` `)  (grave accent) character instead of double or single quotes. Template literals can contain placeholders. These are indicated by the dollar sign and curly braces (${expression}). The expressions in the placeholders and the text between them get passed to a function. The default function just concatenates the parts into a single string. If there is an expression preceding the template literal (tag here),  the template string is called "tagged template literal". In that case, the tag expression (usually a function) gets called with the processed template literal, which you can then manipulate before outputting. To escape a back-tick in a template literal, put a backslash \ before the back-tick.

6.Play audio
    如果只用audio.play();會發生一個問題：在audio還沒播完前再次執行play的話會沒有反應（不會從頭播放）
    => 加 audio.currentTime = 0; 在前面, 手動從頭播

7.Is there any key element has data-key=65?
    const key = document.querySelector(`.key[data-key="${e.keyCode}"]`);

8.Add class to the element
    key.classList.add('playing'); 相當於jQuery的語法 key.addClass('playing');

9.Remove class to the element
    兩種做法：

    - setTimeout(function() {
    -       // remove class here
    }, 0.07);
    - 可以做, 但是當你的css transition想從0.07s改為0.09s時還需要跑回來改js
    - Transition end event (以目前的例子為.key => .playing)
    const keys = document.querySelectorAll('.key'); // 找出所有的key, 這裡用querySelectorAll會返回一個都符合的NodeList

    - keys.forEach(key => key.addEventListener('transitionend', removeTransition))
    - // 不能直接用keys.addEventListener('transitionend', removeTransition) 因為keys為一個NodeList而不是一個element

    - function removeTransition(e) { // 實作removeTransition function
    -   if (e.propertyName !== 'transform') return; //會有很多個transitionend event, 因為我們同時改了大小, 邊筐, 顏色, 陰影
    -   this.classList.remove('playing'); // this 指的是call這個event的element, 也就是key
    - }