1.使用flexbox調整panel位置(CSS code):
flexbox 教學：http://sweeteason.pixnet.net/blog/post/42781628-css-flexbox-layout-%E5%AD%B8%E7%BF%92%E5%BF%83%E5%BE%97

    .panels {
       min-height:100vh;
       overflow: hidden;
       display: flex;
     }

    .panel {
        background:#6B0F9C;
        box-shadow:inset 0 0 0 5px rgba(255,255,255,0.1);
        color:white;
        text-align: center;
        align-items:center;
        /* Safari transitionend event.propertyName === flex */
        /* Chrome + FF transitionend event.propertyName === flex-grow */
        transition:
            font-size 0.7s cubic-bezier(0.61,-0.19, 0.7,-0.11),
            flex 0.7s cubic-bezier(0.61,-0.19, 0.7,-0.11),
            background 0.2s;
        font-size: 20px;
        background-size:cover;
        background-position:center;
        flex: 1; // 將panels區塊平分 (1 : 1 : 1 : 1 : 1)
    }

2.將文字置中(CSS code)

    .panel {
        background:#6B0F9C;
        box-shadow:inset 0 0 0 5px rgba(255,255,255,0.1);
        color:white;
        text-align: center;
        align-items:center;
        /* Safari transitionend event.propertyName === flex */
        /* Chrome + FF transitionend event.propertyName === flex-grow */
        transition:
            font-size 0.7s cubic-bezier(0.61,-0.19, 0.7,-0.11),
            flex 0.7s cubic-bezier(0.61,-0.19, 0.7,-0.11),
            background 0.2s;
        font-size: 20px;
        background-size:cover;
        background-position:center;
        flex: 1;
        justify-content: center; // <p>文字上下置中
        align-items: center; // <p>文字水平置中
        display: flex; // 巢狀flex, 把panel也設為flex
        flex-direction: column; // <p>文字item方向由上而下，由左而右
    }

    /* flex items */
        .panel > * { // Selects all elements where the parent is a panel class element
        margin:0;
        width: 100%;
        transition:transform 0.5s;
        flex: 1 0 auto;  //把panel中的<p>文字也平均分配在panel中(每欄上中下三個)
        display: flex; // 巢狀flex, 把panel中的<p>文字也設為flex
        justify-content: center; // 文字上下置中
        align-items: center; // 文字水平置中
    }

3.將最上面那格和最下面那格隱藏起來(CSS code)

    .panel > *:first-child { // Selects every element that is the first child of its parent
        transform: translateY(-100%); // 將element往Y軸-100%方向移動
    }
    .panel.open-active > *:first-child {  //加上open-active class的話
        transform: translateY(0); // 移回來
    }
    .panel > *:last-child {
        transform: translateY(100%);
    }
    
    .panel.open-active > *:last-child {
        transform: translateY(0);
    }

4.把panel的寬度變比其他的寬(CSS code)

    .panel.open { //加上open class的話
        font-size:40px;// 該element的字變大
        flex: 5; // 該element的size為其他的五倍大
    }

5.加入click event(Javascript code)

    const panels = document.querySelectorAll('.panel');

    function toggleOpen() {
        this.classList.toggle('open'); // add/remove循環
    }

    function toggleActive(e) {
        //console.log(e.propertyName); 發現每click一次會偵測到兩個transition(font-size變大和寬度變五倍大)
        if (e.propertyName === 'flex-grow') {  //我們只需要在panel變寬後執行
             this.classList.toggle('open-active');
        }
    }

    panels.forEach(panel => panel.addEventListener('click', toggleOpen)); // 點擊panel時把它的寬度變比其他的寬
    panels.forEach(panel => panel.addEventListener('transitionend', toggleActive)); // 變寬以後才執行把隱藏文字重新顯現