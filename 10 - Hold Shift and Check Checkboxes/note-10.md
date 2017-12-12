1.選取所有checkbox物件

     const checkboxes = document.querySelectorAll('.inbox input[type="checkbox"]'); // 選取特定type

2.添加event listener

     checkboxes.forEach(checkbox => checkbox.addEventListener('click', handleCheck));

3.完成handleCheck function

    let lastChecked; // 紀錄上一個打勾的checkbox

    function handleCheck(e) {
        let inBetween = false;  // 設一旗標，判定是否在兩個勾選的checkbox中

        // Check if they have the shift key down
        // AND check that they are checking it
        if(e.shiftKey && this.checked) {
    
            // go ahead and do what we please
            // loop over every single checkbox
            checkboxes.forEach(checkbox => {
                if (checkbox === this || checkbox === lastChecked) {
                     // 如果遇到當前的checkbox或lastChecked, 則更新旗標(反向)
                     inBetween = !inBetween;
                }
                if (inBetween) {
                     checkbox.checked = true;
                }
            });
        }
        lastChecked = this; // 更新lastChecked
    }