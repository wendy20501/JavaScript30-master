1.取得所有div element

     const divs = document.querySelectorAll('div');

2.default onclick event

     點了div three後，瀏覽器的動作：
     1) capture from top down (one -> two -> three)
     2) bubbling: trigger events bubble up (three -> two -> one)
     3) 所以你在console得到(three, two, one)

     function logText(e) {
          console.log(this.classList.value);
     }

     divs.forEach(div => div.addEventListener('click', logText);

3.讓瀏覽器在capture時就執行event

     divs.forEach(div => div.addEventListener('click', logText, {
          capture: true // trigger logText while capture down (default: false)
     })); // third parameter: option object

4.停止Propagation

     function logText(e) {
          console.log(this.classList.value);
          e.stopPropagation();
          // click 'three' then just show 'three', 因為Propagation被停掉了不會跑到two跟one
     }

     divs.forEach(div => div.addEventListener('click', logText);

5.once : 只執行一次

     divs.forEach(div => div.addEventListener('click', logText, {
          capture: false, // default
          once: true // listen for a click and unbind itself => no future click events
          // 只會trigger一次logText function, 之後就removeEventListener了

     }));

