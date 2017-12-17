1.追蹤mousemove event的滑鼠位置

     const hero = document.querySelector('.hero');
     const text = hero.querySelector('h1');

     function shadow(e) {
          // 寫法1:
          const width = hero.offsetWidth;
          const height = hero.offsetHeight;

          // 寫法2:
          const { offsetWidth: width, offsetHeight: height } = hero;

          let {offsetX: x, offsetY: y} = e;
          console.log(x, y);
          // 可以發現x跟y都是mouse在“當下”的element的位置
          // text為hero的child element, 但mouse hoover text時x, y是mouse在text的位置(左上0,0開始)
     }

     hero.addEventListener('mousemove', shadow);

2.修正取得的位置(必須取得mouse在hero的位置)

     function shadow(e) {
          const { offsetWidth: width, offsetHeight: height } = hero;
          let {offsetX: x, offsetY: y} = e;

          if (this !== e.target) { // 當不是取得hero時需要修正
               x = x + e.target.offsetLeft; // 加上當前element在hero中的位置
               y = y + e.target.offsetTop;
          }
          console.log(x, y);
     }

3.加上跟隨滑鼠的陰影

     const walk = 100; //100px 陰影範圍

     function shadow(e) {
          const { offsetWidth: width, offsetHeight: height } = hero;
          let {offsetX: x, offsetY: y} = e;

          if (this !== e.target) {
               x = x + e.target.offsetLeft;
               y = y + e.target.offsetTop;
          }

          const xWalk = (x / width * walk) - (walk / 2); // 0 ~ width 等比例對應為 -50px ~ +50px
          const yWalk = (y / height * walk) - (walk / 2); // 0 ~ height 等比例對應為 -50px ~ +50px

          text.style.textShadow = `${xWalk}px ${yWalk}px 0 rgba(255, 0, 255, 0.7),
                                                   ${xWalk * -1}px ${yWalk}px 0 rgba(0, 255, 255, 0.7),
                                                   ${yWalk}px ${xWalk * -1}px 0 rgba(0, 255, 0, 0.7),
                                                   ${yWalk * -1}px ${xWalk}px 0 rgba(0, 0, 255, 0.7)`;
          // 添加陰影
     }