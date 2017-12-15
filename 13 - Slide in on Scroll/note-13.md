1.選取要插入的圖片

     const sliderImages = document.querySelectorAll('.slide-in');

2.監聽網頁滑動時觸發的事件

     function checkSlide(e) {
          console.count(e); // 記錄觸發的次數
     }

     window.addEventListener('scroll', checkSlide);

3.不希望在滑動期間觸發的次數過多導致效能差，降低觸發頻率

    function debounce(func, wait = 20, immediate = true) {
        var timeout;
        return function() {
            var context = this, args = arguments;
            var later = function() {
                timeout = null;
                if (!immediate)
                     func.apply(context, args);
            };
            var callNow = immediate && !timeout;
            clearTimeout(timeout);
            timeout = setTimeout(later, wait);
            if (callNow) func.apply(context, args);
        };
    }

    window.addEventListener('scroll', debounce(checkSlide));

4.完成圖片滑動

    function checkSlide(e) {
        sliderImages.forEach(sliderImage => {
            // half way through the image
            const slideInAt = (window.scrollY + window.innerHeight) - sliderImage.height / 2;
            // window.scrollY: Y軸滑動的距離(畫面最上方)
            // window.innerHeight: browser的viewpoint height
            // 相加就是畫面的最下方

            // bottom of the image
            const imageBottom = sliderImage.offsetTop + sliderImage.height;
            // sliderImage.offsetTop: 從網頁最頂端到圖片的上緣

            const isHalfShown = slideInAt > sliderImage.offsetTop ; // 判斷畫面最下方是否滑超過圖片的一半
            const isNotScrolledPast = window.scrollY < imageBottom; // 判斷畫面最上方是否還沒超過圖片
            if (isHalfShown && isNotScrolledPast) {
                 sliderImage.classList.add('active');
            } else {
                 sliderImage.classList.remove('active');
            }
        });
    }