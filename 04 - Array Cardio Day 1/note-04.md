Practice Array methods!
    Array.prototype.filter()
    1. Filter the list of inventors for those who were born in the 1500's
          三種不同的寫法：
  1)
  const fifteen = inventors.filter(function(inventor) {
            if (inventor.year >= 1500 && inventor.year < 1600)
                  return true; // keep it!
  }); // filter: inventors Array中每個element都經過function過濾(inventor), 只有return true的會留下來

  2)
  const fifteen = inventors.filter(inventor => {
             if (inventor.year >= 1500 && inventor.year < 1600)
                 return true; // keep it!
  });

  3)
           const fifteen = inventors.filter(inventor => (inventor.year >= 1500 && inventor.year < 1600));

  console.table(fifteen); // 在console中印出table格式

  Array.prototype.map()
  2. Give us an array of the inventors' first and last names
  兩種不同的寫法：
  1)
           const fullnames = inventors.map(inventor => inventor.first + ' ' + inventor.last);
           // map:  inventors Array中每個element都經過function去處理(inventor), 回傳處理後的結果
           // 將first name跟last name中間加空格, 可以直接加一個' '

  2)
           const fullnames = inventors.map(inventor => `${inventor.first} ${inventor.last}`);
          // 將first name跟last name中間加空格, 又用到Template literals

  console.log(fullnames);

  Array.prototype.sort()
  3. Sort the inventors by birthdate, oldest to youngest
兩種不同的寫法：
  1)
          const ordered = inventors.sort(function(a, b) {
  if (a.year > b.year)
       return 1;
  else
       return -1;
  });
// sort: inventors Array中每個element都經過function去比較大小來排序
          2)
                   const ordered = inventors.sort((a,b) => a.year > b.year ? 1: -1);

           console.table(ordered);

  Array.prototype.reduce()
  4. How many years did all the inventors live?
  const totalYears = inventors.reduce((total, inventor) => {
               return total + (inventor.passed - inventor.year);
  }, 0);
  // reduce: inventors Array中每個element都經過function去執行(inventor), 得出一個總結果(total), 第二個參數是total的初始值
  console.log(totalYears);

  5. Sort the inventors by years lived
  const oldest = inventors.sort(function(a, b) {
  const lastGuy = a.passed - a.year;
  const nextGuy = b.passed - b.year;
  return lastGuy > nextGuy ? -1 : 1;
  });
  console.table(oldest);

  6. create a list of Boulevards in Paris that contain 'de' anywhere in the name
    https://en.wikipedia.org/wiki/Category:Boulevards_in_Paris         // 在該url的console實作
  const category = document.querySelector('.mw-category');
  const links = Array.from(category.querySelectorAll('a')); // 因為後面要用到Array method, 所以先把NodeList轉成Array
  const de = links
                         .map(link => link.textContent)
                         .filter(streetName => streetName.includes('de'));

  7. sort Exercise
    Sort the people alphabetically by last name
兩種不同的寫法：
  1)
  const alpha = people.sort(function(lastOne, nextOne) {
  const [aLast, aFirst] = lastOne.split(', '); // 用中括號Array指定回傳值的名稱
  const [bLast, bFirst] = nextOne.split(', ');
  return aLast > bLast ? 1 : -1;
  });

          2)
  const alpha = people.sort((lastOne, nextOne) => {
  const [aLast, aFirst] = lastOne.split(', ');
  const [bLast, bFirst] = nextOne.split(', ');
  return aLast > bLast ? 1 : -1;
  });

         console.log(alpha);

  8. Reduce Exercise
  Sum up the instances of each of these
  const data = ['car', 'car', 'truck', 'truck', 'bike', 'walk', 'car', 'van', 'bike', 'walk', 'car', 'van', 'car', 'truck' ];
  const transportation = data.reduce(function(obj, item) {
  if (!obj[item]) {
       obj[item] = 0;
  }
  obj[item]++;
  return obj;
  }, {});
  console.log(transportation);