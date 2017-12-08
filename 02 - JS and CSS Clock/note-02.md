1.旋轉物件(CSS code):

     transform: rotate(20deg); // 轉20度, 預設是沿著物件的中心
     transform-origin: 100%; // 把旋轉中心定為最右側(從左0%~右100%, default為50%也就是正中間)

2.動畫時間(CSS code):

     transition: all 0.5s; // 每個移動都是0.5秒做完
     transition-timing-function: ease-in-out; //ease-in-out：Specifies a transition effect with a slow start and end
     此時你會發現有個紫色小圖案可以編輯動畫的快慢(cubic bezier editor)
     點進去可以自己調整它的曲線這樣
     選好以後你會得到他的曲線值：
     transition-timing-function: cubic-bezier(0.1, 2.7, 0.58, 1);

3.讓指針在固定時間轉動(JS code):

     function setDate() {
          ...
          console.log('Hi');
     }
     setInterval(setDate, 1000); // 每一秒執行一次setDate function
     // The setInterval() method calls a function or evaluates an expression at specified intervals (in milliseconds).

4.取得當前時間(JS code):

     const secondHand = document.querySelector('.second-hand');
     function setDate() {
       const now = new Date(); // 使用Date 物件
       const seconds = now.getSeconds(); // 可以擷取需要的部份(秒)
       const secondsDegrees = ((seconds / 60) * 360) + 90; // 算出每秒要轉的角度(指針初始值為水平, 所以加90來調整到時鐘整點的位置)
       secondHand.style.transform = `rotate(${secondsDegrees}deg)`; // 調整物件的CSS code, 又用到｀與${變數}來動態調值
     }

5. 把minutes跟hours補齊

6.出現問題: 回到整點(60)時, 它會轉一整圈...

     => 解法：
     1) 時間一直加上去
     2) 在60的時候把transition停掉, 之後再打開
