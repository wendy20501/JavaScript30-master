1.點擊enter或是submit button後，添加input的值到itemsList

     const addItems = document.querySelector('.add-items'); // form
     addItems.addEventListener('submit', addItem);

     const itemsList = document.querySelector('.plates');
     const items = [];

     function addItem(e) {
          e.preventDefault();
          // default是會reload頁面(網址加上?item=fish)，不是我們要的效果，所以將其取消

          const text = (this.querySelector('[name=item]')).value;
          const item = {
               text, // 也可以寫成 text: text,
               done: false
          }
          items.push(item);
          populateList(items, itemsList); // 將item添加到itemsList(修改html code)
          this.reset();
     }

     function populateList(plates = [], platesList) { // 設定default是空的array, 如果沒有輸入值也不會出錯
          platesList.innerHTML = plates.map((plate, i) => { // i: index
               return `
                    <li>
                         <input type="checkbox" data-index=${i} id="item${i}" ${plate.done ? 'checked' : ''} /> // ${} 內可以寫判斷式
                         <label for="item${i}">${plate.text}</label> // label for: Specifies which form element a label is bound to
                    </li>
               `;
          }).join(''); // map回傳的是一個string array, 將他轉換為一個string
     }

2.將itemsList內容存進localStorage, 頁面reload後還可以保存

     /* 查看localStorage
          Application -> Storage -> Local Storage */



     const items = JSON.parse(localStorage.getItem('items')) || [];
     // localStorage有值則用JSON parse將其轉換回array, 沒值的話就是空array

     function addItem(e) {
          e.preventDefault();
          const text = (this.querySelector('[name=item]')).value;
          const item = {
               text,
               done: false
          }
          items.push(item);
          populateList(items, itemsList);
          localStorage.setItem('items', JSON.stringify(items)); // localStorage setItem只能存string, 所以先把array轉成string
          this.reset();
     }

     populateList(items, itemsList); // 在頁面第一次load時就先把localStorage的值顯示在itemsList中

3.點擊item時將checkBox打勾或取消打勾

    populateList(items, itemsList);

    /* -------------------------------------------------------------------------------------------*/
    /* 錯誤示範：當頁面第一次load時直接將input都加上eventListener的話,
         之後動態添加的input就沒有eventListener了 */
    const checkBoxes = document.querySelectorAll('input');
    checkBoxes.forEach(input => input.addEventListener('click', () => alert('hi')));
    /* -------------------------------------------------------------------------------------------*/

    itemsList.addEventListener('click', toggleDone); // 正確做法：直接對parent element添加eventListener

    function toggleDone(e) {
         console.log(e.target); // e.target為被按到的element, 會發現每點一次都會出現不只一個element(checkbox + label)

         if (!e.target.matches('input')) // 過濾不必要的element
              return; //skip this unless it's an input

         const el = e.target;
         const index = el.dataset.index; // 取出index
         items[index].done = !items[index].done; // 打勾或取消打勾
         localStorage.setItem('items', JSON.stringify(items)); // 存回localStorage
         populateList(items, itemsList); // update HTML
    }