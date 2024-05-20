# JavaScript 嚴格模式下的限制
## 前言
嚴格模式是 ES5 新增的功能，只要在程式碼中加入 "use strict" 的字樣，加入嚴格模式字樣的位置也是需要考量的，如果是加在檔案的開頭，那麼整個 js 檔案都會以嚴格模式進行；如果是在函式內的第一行加上嚴格模式，那就是只有該函式需要遵守嚴格模式。

## 為什麼需要嚴格模式
JavaScript 撰寫相當的自由，但也由於這份自由，會讓程式碼有點不好控制，為此為了讓開發人員避免ㄧ些不必要的錯誤，甚至是提早避免錯誤的發生，為此，嚴格模式應運而生。

## 加入 "use strict" 的效果位置
## 檔案開頭加上 "use strict"
在檔案開頭加上 "use strict" 代表該整個檔案使用嚴格模式
```js
// main.js
"use strict"

var name = "Joe";
```

## 函式第一行加上 "use strict"
在函式第一行加上 "use strict"，只有在該函式有嚴格模式，其他地方則沒有
```js
function foo() {
  "use strict"
  var name = "Joe"
}
```

## 在嚴格模式下的撰寫規則
在嚴格模式下，有一些撰寫限制，如果不按照規則來，會跳出 error 訊息
1. 沒有宣告就直接使用變數進行給值的動作
2. 使用 delete 刪除變數或是函式
3. 重複的函式名稱宣告
4. 物件內有重複的屬性
5. 將 eval 與 argument 當作變數名稱
6. 使用 with 語法
7. 嚴格模式下的 this 會是 undefined
8. 不可使用 8 進位的值
9. 不可對只可讀取的屬性進行寫入

## 如何在瀏覽器的開發者工具測試嚴格模式
找了很久怎麼快速進行嚴格模式的測試，發現瀏覽器上面好像都沒有相關的按鈕可以勾選，最後看到是說直接使用 IIFE 的方式進行測試就可以了

```js
(function() {
  "use strict"
  test = 1;  // Uncaught Reference Error: test is not defined
})()
```

## 如何在 Node.js 的 REPL 開啟嚴格模式
開啟終端機然後輸入

> node  --use_strict

就能開啟嚴格模式了