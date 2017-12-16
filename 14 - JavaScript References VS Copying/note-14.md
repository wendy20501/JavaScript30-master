1.start with strings, numbers and booleans

    let age = 100;
    let age2 = age; // 用'='直接是Copy
    console.log(age, age2);
    age = 200;
    console.log(age, age2); // 不會改到age2的值

    // string也是一樣
    let name = 'Wes';
    let name2 = name;
    console.log(name, name2);
    name = 'weslay';
    console.log(name, name2);

2.Let's say we have an array

    const players = ['Wes', 'Sarah', 'Ryan', 'Poppy'];

    // and we want to make a copy of it.
    const team = players;

    console.log(players, team);

    // You might think we can just do something like this:
    team[3] = 'Lux';

    // however what happens when we update that array?
    // now here is the problem!
    // oh no - we have edited the original array too!
    // Why? It's because that is an array reference, not an array copy. They both point to the same array!
    // So, how do we fix this? We take a copy instead!

    // one way
    const team2 = players.slice(); // 不填參數就是直接copy

    // or create a new array and concat the old one in
    const team3 = [].concat(players);

    // or use the new ES6 Spread
    const team4 = [...players]; // 將array拆解後又丟到array中

    // or create a new array from the old one
    const team5 = Array.from(players);

    // now when we update it, the original one isn't changed

3.The same thing goes for objects, let's say we have a person object

    const person = {
        name: 'Wes Bos',
        age: 80
    };

    // and think we make a copy:
    const captain = person;
    captain.number = 99;

    // how do we take a copy instead?
    const cap2 = Object.assign({}, person, {number: 99, age: 12});
    // 參數1: 空的object, 參數2: 原始object, 參數3: 要添加/修改的element

    // We will hopefully soon see the object ...spread
    const cap3 = {...person};

    // Things to note - this is only 1 level deep - both for Arrays and Objects. lodash has a cloneDeep method, but you should think twice before using it.
    // 第二層以上要改的話還是會改到原始的object

    const wes = {
        name: 'Wes',
        age: 100,
        social: {
            twitter: '@wesbos',
            facebook: 'wesbos.developer'
        }
    };
    console.clear();
    console.log(wes);

    const dev = Object.assign({}, wes);

    // 偷吃步, 先轉成JSON再轉回來即為不一樣的obiect, 但效率可能差
    const dev2 = JSON.parse(JSON.stringify(wes));