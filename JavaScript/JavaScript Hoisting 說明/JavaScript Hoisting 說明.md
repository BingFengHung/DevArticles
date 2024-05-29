# JavaScript Hoisting 說明
## Hoisting 介紹
提升 (Hoisting)，根據 MDN 所說，提升看起來是單純的將變數和函式宣告，移動到程式的區塊頂端，然而並非如此。**變數和函數的宣告會在編譯階段就被放入記憶體，但實際位置和程式碼中完全一樣**。

這樣做的優點是，因為有預先載入到記憶體中，所以是函式定義之前就對函式進行呼叫的話，他還是能夠順利找到該函式並且執行，範例程式碼如下：

```js
call()  // hello

function call() {
  console.log('hello')
}
```

如果是變數的部分，可以看以下程式碼，在還沒有到達宣告變數的那一行，還是能夠順利對變數進行存取，只是拿到的會是 undefined。

```js
console.log(myVar)  // undefined
var myVar = 10;
console.log(myVar)  // 10
```

## Hoisting 到底提升到哪一個範圍
這邊就要提到 var、let 與 const 了，let 與 const 是 ES6 才推出的關鍵字，這兩類的提升範圍也就是 Scope 不一樣。

> var 是 Function Scope
> let、const 是 Block Scope

簡單的程式碼比對如下

### var (Function Scope)

```js
function call() {
  console.log(x)  // undefined
  if (true) {
    var x = 'hello'
    console.log(x)  // hello
  }
}

call()
```

### let、const (Block Scope)

```js
function call() {
  var x = 'hi'
  console.log(x)  // 'hi'
  
  if (true) {
    let x = 'hello'
    console.log(x)  // 'hello'
  }
  
  console.log(x)  // 'hi'
}

call()
```

可以看到在 if 裡面有宣告 let x，但由於它的作用範圍在大括號之間 **{}**，所以最後面的 x 在輸出的時候還是 var x 的值 hi。

**這邊有兩個重點要記得：**
- let 與 const 也是會 hoisting 的只是在還沒有到宣告變數那一行之前，前面都算是暫時死區 (Temporal dead zone)
```js
x = 10;  // Cannot access 'x' before initialization
let x;
```
- 在嚴格模式底下，如果沒有宣告就直接對變數做賦值的動作會跳錯誤
```js
"use strict"
x = 10
// ReferenceError: x is not defined
```

## 個人疑問? 
var 是 funtion level scope，但是上面的程式碼並沒有放在 function 中，為甚麼該變數也會被 hosting?

如果該變數宣告是在全域範圍內宣告，那麼該變數的作用域會是全域範圍。也由於 hosting 在全域範圍內也是有效的，代表該變數宣告會被提升到全域範圍的最頂層。

## 參考資料
[MDN - Hoisting](https://developer.mozilla.org/zh-TW/docs/Glossary/Hoisting)