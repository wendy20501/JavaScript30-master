1.用fetch API取得資料並存成array
    
    fetch API 介紹：https://eyesofkids.gitbooks.io/javascript-start-from-es6/content/part4/ajax_fetch.html

    const cities = []; // 這是我們要存成的結果array
    //let cities = [];

    const prom = fetch(endpoint) // 從endpoint中提取資料(回傳promise型態物件)
                                    .then(blob => blob.json())
                                       // 用then去讀取promise物件, blob是raw data
                                       // 可以把blob印在console.log上, 會發現有json function可以用 (回傳promise型態物件)



                                       .then(data => cities.push(...data));
                                           // 再用then去讀取promise物件, 把data印在console.log上, 會發現它是一個Array
                                           // 要把data array存進cities不能單純用 cities = data
                                           // => 因為cities 是const, 把const改成let就可以, 但就有機會會被修改到
                                           // 另外一個方法：cities.push(data)
                                           // => 但是 data會成為cities的第一個item(cities[0] = data)...不是我們要的
                                           // => 將data spread into : 也就是前方加上...
                                           // spread into:  https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_operator

2.在cities中找到與輸入的值匹配的項目, 回傳新的array

    function findMatches(wordToMatch, cities) {
         return cities.filter(place => {
              // here we need to figure out if the city or state matches what was searched
              // return place.city.match(/Bos/i);
              //使用match去匹配一個string, 但是我們需要該string為一個變數 => regular expression
              const regex = new RegExp(wordToMatch, 'gi'); // g: global  i: insensitive 大小寫皆可
              return place.city.match(regex) || place.state.match(regex);
         });
    }

3.加入eventListener

    const searchInput = document.querySelector('.search');
    const suggestions = document.querySelector('.suggestions');

    searchInput.addEventListener('change', displayMatches); // 當點擊input框框以外的地方則觸發
    searchInput.addEventListener('keyup', displayMatches); // 每打一個字就觸發

    function displayMatches() {
        console.log(this.value);  // searchInput的值
    }

4.把匹配值顯示在HTML中

    function displayMatches() {
         const matchArray = findMatches(this.value, cities);
         const html = matchArray.map( place => {
              return ` // Template literals
                   <li>
                        <span class="name">${place.city}, ${place.state}</span>
                        <span class="population">${place.population}</span>
                  </li>
              `;
         }).join(''); // map return的是一個Array, 我們要的是一個string, 所以直接用join把所有element接起來
         suggestions.innerHTML = html;
    }

5.把匹配值反白，把人口數加上格式

    function displayMatches() {
         const matchArray = findMatches(this.value, cities);
         const html = matchArray.map( place => {
              const regex = new RegExp(this.value, 'gi'); // regular expression
              const cityName = place.city.replace(regex, `<span class="hl">${this.value}</span>`); // 將匹配值套上CSS
              const stateName = place.state.replace(regex, `<span class="hl">${this.value}</span>`);
              return `
                   <li>
                        <span class="name">${cityName}, ${stateName}</span>
                        <span class="population">${numberWithCommas(place.population)}</span>
                  </li>
              `;
         }).join('');
         suggestions.innerHTML = html;
    }

    function numberWithCommas(x) {  // 數字的格式

         return x.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ',');
    }