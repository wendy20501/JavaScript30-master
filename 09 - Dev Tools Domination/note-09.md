1.在網頁執行時debug

     在要追蹤的element按右鍵 > Break on... > attribute modifications
     之後再重新測試，會直接跳到改變element的行數

2.Regular

     console.log('hi!');

3.Interpolated

     console.log('hi %s!', 'Jason'); // 用%s插入string

4.Styled

     console.log('%c today is my day', 'color:red; background: blue; font-size:5px'); // 用%c差入CSS

5.warning

     console.warn('ooh no');

6.Error

     console.error('wrong answer');

7.Info

     console.info('I am hungry');

8.Testing

     console.assert(1===31, 'nothing'); // 參數1: 判斷式，參數2: 為False則顯示在console.error的訊息

9.clearing

     console.clear();

10.Viewing DOM Elements

     var i = document.querySelector('p');
     console.log(i);
     console.dir(i); // 印出element的詳細內容項目

11.Grouping together // 將訊息用群組方式顯示在console.log

    dogs.forEach(dog => {
    console.group(`${dog.name}`); // 開始群組(群組名)，預設展開
    // console.groupCollapsed(`${dog.name}`); // 開始群組(群組名)，預設收合

    console.log(`This is ${dog.name}`);
    console.log(`${dog.name} is ${dog.age} years old`);
    console.groupEnd(`${dog.name}`); // 結束群組(群組名)

    });

12.counting

    console.count('dd'); // 計算紀錄了幾次
    console.count('dde');
    console.count('dd');
    console.count('dd');
    console.count('dd');

13.timing // 計算執行時間

    console.time('fetching data'); // 開始計算(名稱)
    fetch('https://api.github.com/users/wesbos')
    .then(data => data.json())
    .then(data => {
        console.timeEnd('fetching data'); // 結束計算(名稱), 印出計算結果於console.log
        console.log(data);
    });