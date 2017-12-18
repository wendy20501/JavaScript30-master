1.將bands排序

    const bands = ['The Plot in You', 'The Devil Wears Prada', 'Pierce the Veil', 'Norma Jean', 'The Bled', 'Say Anything', 'The Midway State', 'We Came as Romans', 'Counterparts', 'Oh, Sleeper', 'A Skylit Drive', 'Anywhere But Here', 'An Old Dog'];

    const sortedBands = bands.sort(function(a, b) {
         if (a > b) {
              return 1;
         } else {
              return -1;
         }
    });

2.將a, the, an 過濾掉

     function strip(bandName) {
          return bandName.replace(/^(a |the |an )/i, '').trim(); // 以xxx開頭
     }

3.依照過濾後的band排序

    // 寫法1:
    const sortedBands = bands.sort(function(a, b) {
         if (strip(a) > strip(b)) {
              return 1;
         } else {
              return -1;
         }
    });

    // 寫法2:
    const sortedBands = bands.sort((a, b) => {
         return strip(a) > strip(b) ? 1 : -1;
    });

    // 寫法3:
    const sortedBands = bands.sort((a, b) => strip(a) > strip(b));

4.將結果寫回HTML

    document.querySelector('#bands').innerHTML =
              sortedBands
                   .map(band => `<li>${band}</li>`)
                   .join('');
    // innerHTML必須是string, 若為array則會直接呼叫toString(), 但是element間會存在逗號','
    // 用join就不會有此問題