1.取得所有element

     const timeNodes = Array.from(document.querySelectorAll('[data-time]')); // 將NodeList轉成Array

2.取得所有秒數

     const seconds = timeNodes.map(node => node.dataset.time) // 取得element的時間
                              .map(timeCode => {
                                 const [mins, secs] = timeCode.split(':').map(parseFloat); // 將時間分成mins, secs且轉成浮點數
                                 return (mins * 60) + secs; // 取得總秒數
                              })
                              .reduce((total, vidSeconds) => total + vidSeconds); // 全部加總

3.計算出h:m:s

     let secondLeft = seconds;
     const hours = Math.floor(secondLeft / 3600);
     secondLeft = secondLeft % 3600;
     const minutes = Math.floor(secondLeft / 60);
     secondLeft = secondLeft % 60;
     console.log("%s:%s:%s", hours, minutes, secondLeft);