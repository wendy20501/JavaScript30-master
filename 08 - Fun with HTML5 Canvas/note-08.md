1.取得canvas

    const canvas = document.querySelector('#draw');
    const ctx = canvas.getContext('2d'); // 之後是畫在context上面, 可以取2d或3d
    canvas.width = window.innerWidth;  // 設定canvas寬度跟視窗一樣大
    canvas.height = window.innerHeight; // 設定canvas長度跟視窗一樣大
    ctx.strokeStyle = '#BADA55'; // color
    ctx.lineJoin = 'round'; // join between two lines
    ctx.lineCap = 'round'; // the end of the line

2.追蹤滑鼠移動的痕跡

    let isDarwing = false;
    let lastX = 0;  // 劃線的開始值
    let lastY = 0;  // 劃線的開始值

    function draw(e) {
         console.log(e);
    }
    canvas.addEventListener('mousemove', draw);

3.判斷是否畫(滑鼠按下才畫)

    function draw(e) {
         if (!isDarwing) return; // stop the function from running when they are not moused down.
         console.log(e);
    }

    canvas.addEventListener('mousedown', () => isDarwing = true); // 按下才開始畫
    canvas.addEventListener('mouseup', () => isDarwing = false); // 放開就停止畫
    canvas.addEventListener('mouseout', () => isDarwing = false); // 超出視窗也停止畫

4.實際畫線

    function draw(e) {
        if (!isDarwing) return;
        ctx.beginPath(); // 劃線開始
        ctx.moveTo(lastX, lastY); // 從起始點
        ctx.lineTo(e.offsetX, e.offsetY); // 到終點 (目前滑鼠位置)
        ctx.stroke(); // 劃線結束
        [lastX, lastY] = [e.offsetX, e.offsetY]; // 更新起始點位置, 但每次都會接續上次的結束位置...
    }

5.滑鼠按下時更新起始點為目前滑鼠位置

    canvas.addEventListener('mousedown', (e) => {
        isDarwing = true;
        [lastX, lastY] = [e.offsetX, e.offsetY];
    });

6.調整顏色=>彩色

    Mother-effing hsl: 用程式選顏色！網址：http://mothereffinghsl.com/
    hue: 彩虹顏色從紅色到紅色
    saturation: 飽和度
    light: 亮度

    let hue = 0;
    ctx.strokeStyle = `hsl(${hue}, 100%, 50%)`; //每次畫線都改變顏色
    hue++;
    if (hue >= 360)
        hue = 0;

7.調整寬度=>從大到小到大到小...

    let direction = true;

    if (ctx.lineWidth >= 100 || ctx.lineWidth <= 1) { // 超過最大或最小=>換方向

         direction =! direction;
    }
    if (direction)
         ctx.lineWidth++;
    else
         ctx.lineWidth--;

8.globalCompositeOperation

    ctx.globalCompositeOperation = 'multiply'; // 當顏色重疊時會變累加變成黑色
    不同的效果：https://developer.mozilla.org/en-US/docs/Web/API/CanvasRenderingContext2D/globalCompositeOperation
