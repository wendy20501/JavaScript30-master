Using Javascript to update CSS variables: When you update a variable in CSS, everywhere on the page that variable is referenced  will update itself.
With Sass, you define them at compile time and then it gets compiled you cannot change it.

1.定義可動態調整的CSS variables (CSS code)

     :root { // the highest level in CSS
       --base: #ffc600;
       --spacing: 10px;
       --blur: 10px;
     }
     // custom properties name，搭配var使用，var(--base) 即表示它的值。reference: https://drafts.csswg.org/css-variables/


     img {
       padding: var(--spacing);
       background: var(--base);
       filter: blur(var(--blur)); // The filter property defines visual effects (like blur and saturation) to an element (often <img>).
     }

     .hl {
       color: var(--base);
     }

2.用Javascript 動態調整CSS root裡面的variables (Javascript code)

     1) 抓出用來調整的input object
          const inputs = document.querySelectorAll('.controls input');
          // 回傳的是NodeList而非Array, 所以他不能用array的其他methods (map, reduce, ...), NodeList可以用的method比較少

     2) 用來 update CSS 的function
          function handleUpdate() {
               const suffix = this.dataset.sizing || '';
               // 因為我們從input只能取得sizing的數值, 假設是20, 但是要改 CSS code我們還需要加一個單位後綴(px), 組合起來才是20px
               // 參考以下HTML code, 在input的element裡已寫入data-sizing這個HTML data-* Attributes
               // ELEMENT.dataset 這個OBJECT包含了此ELEMENT的所有 data attributes

               // 因此HTML data-* Attributes的值可以用 OBJECT.dataset.*來取得

               //後面 || '' 是為了以防萬一沒有這個後綴詞的需求(例如顏色就沒有)，也就沒有該HTML data-* Attributes，怕會出錯於是就加上“或是空的”


               document.documentElement.style.setProperty(`--${this.name}`, this.value + suffix);
               // 又用到了Template literals, 把--base, --spacing, 和 --blur轉換成變數
               // 在以下HTML code可以發現我們把input的name都取為和要調整的變數一致, 所以可以直接用--${this.name}取得相對應的CSS變數名
          }
     -----------------------------------------以下為HTML code 片段-----------------------------------------
     <div class="controls">
          <label for="spacing">Spacing:</label>
          <input id="spacing" type="range" name="spacing" min="10" max="200" value="10" data-sizing="px">

          <label for="blur">Blur:</label>
          <input id="blur" type="range" name="blur" min="0" max="25" value="10" data-sizing="px">

          <label for="base">Base Color</label>
          <input id="base" type="color" name="base" value="#ffc600">
     </div>
     -----------------------------------------------------------------------------------------------------

     3) 用EventListener來trigger handleUpdate function
          inputs.forEach(input => input.addEventListener('change', handleUpdate)); // 只在值停止變動的時候trigger
          inputs.forEach(input => input.addEventListener('mousemove', handleUpdate)); // 在滑鼠滑來滑去的時候就跟跟著trigger
