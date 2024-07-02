# JavaScript ESM 模組

ES Module 是 JavaScript 中實現模組功能的標準方法，讓使用者能夠將程式碼拆分成可重複使用的模組，讓這些模組能夠輕易的在不同的 JavaScript 檔案中引入使用。

## 基本概念
模組通常指的是一個單獨的 JS 檔案，這個檔案包含了特定的功能，可以被其他模組或是檔案引入使用。

每個 JS 檔案，再被當作模組使用的時候，都有自己的作用域，也就是說在模組內定義的變數、函式、類別等，不會成為全域作用域的一部分。

1. 導出 (Export): 將任何 JavaScript 項目 (如變數、函數、類別等) 導出。
2. 導入 (Import): 從其他模組導入

## export 語法

### 預設導出

每個 js 模組只能有一個預設導出，再導入時，可以給他任意的名稱

```js
export default function() {
   console.log("Hello world");
}
```

### 具名導出: 可導出多個值，每個都有自己的名稱

```js
export const number = 42;
export function printNumber() {
   console.log(number);
}
```

## import 語法

### 導入具名的 export： 需要明確指明要導入的名稱

```js
import { number, printNumber } from './file1.js';

printNumber();  // 42
```

### 導入預設 export: 可以給定任意名稱

應該是說一定要給一個名稱

```js
import myFunction from './file2.js';

myFunction();
```

## 其他導入方式
導入整個模組，可以用 * 將一個模組內的所有導出導入

```js
import * as File1 from './file1.js';

File1.printNumber();  // 42
```

如果說是使用 export default 輸出的化，要使用上面的方法引入，就會變成這樣呼叫

```js
// file-default.js
export default function() {
    console.log('Hello');
}
```

```js
import * as my from './file-default.js';

my.default();  // Hello
```

## 動態導入

ESMAScript 允許你非同步導入模組

```js
async function loadModule() {
  const module = await import('./file1.js');
  module.printNumber();  // 42
}

loadModule
```

**優點:**
1. 模組內的變數不會汙染全域環境
2. 延遲執行，導入的模組在需要時才執行。

## 注意
```js
export const number = 42;
export function printNumber() { 
    console.log(number);
}
```

這樣寫與

```js
const number = 42;
function printNumber() { 
    console.log(number);
}

export { number, printNumber }
```

**差別在哪邊**

這兩種導出方式在功能上是相同的，都可以讓其他模塊導入 number 和 printNumber，但它們在語法結構和靈活性上有所不同：

直接導出 (Inline Export):

```js
export const number = 42;
export function printNumber() {
    console.log(number);
}
```

**在宣告變數或函式時立即導出。**
這種方式對於立即明確哪些項目被導出是很有幫助的，因為每個導出的元素都明確地標記了 export。
較為直觀，可以一目了然地看到哪些功能是對外可用的。

**先宣告後導出 (Declaration First, Export Later):**

```js
const number = 42;

function printNumber() {
    console.log(number);
}

export { number, printNumber };
```

先宣告變數和函式，然後在檔案的某個位置集中導出。

這種方式可更加靈活地控制導出的元素，特別是當模組中有多個函式和變數，而只希望導出其中幾個時。

可以在模塊的底部一次性看到所有導出的項目，這對於模塊的管理和維護來說，可能更清晰易懂。

### 選擇哪種方式？
如果你想在需告的時候就表明這個變數或函式是用於導出的，直接導出是一個不錯的選擇。
如果你需要更好的導出控制或想在模組中保持一定的封裝性，則可以選擇先宣告後導出。

## 小結
導出 (Export): 分為「直接導出」和「集中導出」。
- 直接導出：在聲明變數、函數或類的同時加上 export。
- 集中導出：在檔案的底部或適當位置使用 export { item1, item2 }。

導入 (Import): 分為「具名導入」、「預設導入」和「整體導入」。
- 具名導入：import { item1, item2 } from './module'。
- 預設導入：import defaultItem from './module'。
- 整體導入：import * as Module from './module'。
