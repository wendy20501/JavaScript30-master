1.取得所有anchor element

     const treggers = document.querySelectorAll('a');

2.產生一個反白的element(CSS已寫好)

     const highlight = document.createElement('span');
     highlight.classList.add('highlight');
     document.body.append(highlight); // 將該element加進html中

3.偵測滑鼠是否移動到anchor物件

     treggers.forEach(a => a.addEventListener('mouseenter', highlightLink));

     function highlightLink() {
          console.log('Highlight!');
     }

4.取得anchor的位置



     function highlightLink() {
          const linkCoords = this.getBoundingClientRect(); // 取得該anchor的位置值
          console.log(linkCoords);
     }

5.將反白element設定為anchor的位置

     function highlightLink() {
          const linkCoords = this.getBoundingClientRect();
          highlight.style.width = `${linkCoords.width}px`;
          highlight.style.height = `${linkCoords.height}px`;
          highlight.style.transform = `translate(${linkCoords.left}px, ${linkCoords.top}px)`;
     }

6.修正上下跟左右滑動視窗的距離

     function highlightLink() {
          const linkCoords = this.getBoundingClientRect();
          const coords = {
               width: linkCoords.width,
               height: linkCoords.height,
               top: linkCoords.top + window.scrollY,
               left: linkCoords.left + window.scrollX
          }
          highlight.style.width = `${coords.width}px`;
          highlight.style.height = `${coords.height}px`;
          highlight.style.transform = `translate(${coords.left}px, ${coords.top}px)`;
     }