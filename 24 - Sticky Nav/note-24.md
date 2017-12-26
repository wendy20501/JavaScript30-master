1.取得nav bar

     const nav = document.querySelector('#main');

2.當捲動螢幕超過nav時固定nav

     const topOfNav = nav.offsetTop; // nav的頂端

     function fixNav() {
          //console.log(topOfNav, window.scrollY);
          if (window.scrollY >= topOfNav) {
               document.body.style.paddingTop = nav.offsetHeight + 'px';
               // (參考以下CSS code) 將nav 的 position改成fixed
               // 原先relative時nav bar所佔的體積消失了，nav bar以下的內容會突然往上跳
               // 在body加入nav bar的高度作為padding-top來修正消失的體積
               document.body.classList.add('fixed-nav'); // 將body加上fixed-nav class
          } else {
               document.body.style.paddingTop = 0 + 'px'; // 把修正用的padding-top拿掉

               document.body.classList.remove('fixed-nav'); // 將body移除fixed-nav class

          }
     }

     window.addEventListener('scroll', fixNav);

3.修改CSS code

     nav {
          background:black;
          top:0;
          width: 100%;
          transition:all 0.5s;
          position: relative;
          z-index: 1;
     }

     .fixed-nav nav{
          position: fixed; // 將position改成fixed => 原先relative時nav bar所佔的體積消失了
          box-shadow: 0 5px rgba(0,0,0,0.1);
     }

4.當捲動螢幕超過nav時讓logo從nav bar左邊移出來 (CSS code)

     li.logo {
          max-width:0; // 初始值寬度0，所以不會顯示

          overflow: hidden;
          background: white;
          transition: all .5s;
          font-weight: 600;
          font-size: 30px;
     }

     .fixed-nav li.logo {
          max-width: 500px; // 給它一個寬度值，就會顯示，不能為auto, 因為寬度無法從0變成auto

     }

5.當捲動螢幕超過nav時nav bar以下的內容放大

     .site-wrap {
          max-width: 700px;
          margin: 70px auto;
          background:white;
          padding:40px;
          text-align: justify;
          box-shadow: 0 0 10px 5px rgba(0, 0, 0, 0.05);
          transform: scale(0.98); // 初始值縮小了0.98倍
          transition: transform 0.5s;
     }

     .fixed-nav .site-wrap {
          transform: scale(1); // 還原大小
     }