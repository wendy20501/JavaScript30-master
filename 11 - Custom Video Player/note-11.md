1.Get our elements

    const player = document.querySelector('.player');
    const video = player.querySelector('.viewer');
    const progress = player.querySelector('.progress');
    const progressBar = player.querySelector('.progress__filled');
    const toggle = player.querySelector('.toggle');
    const skipButtons = player.querySelectorAll('[data-skip]'); // 取得有這個property的element
    const ranges = player.querySelectorAll('.player__slider');

2.Play and pause video

    function togglePlay() {
        // 有兩種寫法
        // 1)
        const method = video.paused ? 'play' : 'pause'; // 判斷是否為pause
        video[method]();

        // 2)
        if(video.paused) {
             video.play();
        } else {
             video.pause();
        }
    }

    video.addEventListener('click', togglePlay);
    toggle.addEventListener('click', togglePlay);

3.更新播放按鈕圖案(不綁定togglePlay, 因有可能用其他plugin去播放, 因此綁定video的狀態)

    function updateButton() {
    const icon = this.paused ? '►' : '❚ ❚';
    toggle.textContent = icon; // 更改text內容
    }

    video.addEventListener('play', updateButton);
    video.addEventListener('pause', updateButton);

4.往前或往後快進特定時間

    function skip() {
         video.currentTime += parseFloat(this.dataset.skip); // String to float
    }

    skipButtons.forEach(button => button.addEventListener('click', skip));

5.調整音量跟速度

    function handleRangeUpdate() {
         video[this.name] = this.value;
    }

    ranges.forEach(range => range.addEventListener('change', handleRangeUpdate));
    ranges.forEach(range => range.addEventListener('mousemove', handleRangeUpdate));

6.更新播放進度

    function handleProgress() {
        const percent = (video.currentTime / video.duration) * 100;
        progressBar.style.flexBasis = `${percent}%`; // CSS中調整進度的值
    }

    video.addEventListener('timeupdate', handleProgress); // 或用progress也行

7.動態調整播放進度

    function scrub(e) {
        const scrubTime = ((e.offsetX / progress.offsetWidth)) * video.duration;
        video.currentTime = scrubTime;
    }

    let mousedown = false;
    progress.addEventListener('click', scrub);
    progress.addEventListener('mousemove', (e) => mousedown && scrub(e)); //同時按下滑鼠及拖移的時候執行
    progress.addEventListener('mouseDown', () => {mousedown = true});
    progress.addEventListener('mouseUp', () => {mousedown = false});