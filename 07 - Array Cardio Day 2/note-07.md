1.Array.prototype.some() // is at least one person 19 or older?

    const isAdult = people.some(person => { // 只要有一個是true就回傳true
    const currentYear = (new Date()).getFullYear(); // date.getFullYear() == date.getYear() + 1900
    return currentYear - person.year >= 19;
    });
    console.log({isAdult});

2.Array.prototype.every() // is everyone 19 or older?

      const allAdult = people.every(person => {// 全部都是true才回傳true
      const currentYear = (new Date()).getFullYear();
      return currentYear - person.year >= 19;
      });
      console.log({allAdult});

3.Array.prototype.find()
// Find is like filter, but instead returns just the one you are looking for

    // find the comment with the ID of 823423
    // 兩種寫法：
    1) 寫完整function
        const comment = comments.find(function(comment) { // 回傳是true的subset
            if (comment.id === 823423)
                return true;
        });

    2) 簡短版
          const comment = comments.find(comment => comment.id === 823423);
    console.log({comment});

 4.Array.prototype.findIndex()
// Find the comment with this ID

     // delete the comment with the ID of 823423

     const index = comments.findIndex(comment => comment.id === 823423); // 回傳是true的index
     console.log(index);

     // 刪除element有兩種寫法：
     1)
               comments.splice(index, 1); // 從index開始往後刪除一個element

     2)
     const newComments = [ // 宣告一個新的變數
     ...comments.slice(0, index), // 把comments Array做切割 // 另外要做spread才不會多存一層array

     ...comments.slice(index + 1)
     ];