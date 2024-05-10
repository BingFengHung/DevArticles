# JavaScript this 關鍵字

之前對於 JavaScript 的 this 關鍵字常常搞不清楚，最近靜下心來好好的研讀了一下，終於將 this 關鍵字搞明白，為此趕緊筆記下來以免過一段時間又忘記。

this 關鍵字指向的是當前的一個執行環境，可以對當前**執行環境**進行一些操作。

由於他指向的是執行環境，所以在撰寫程式碼的階段其實是不知道 this 指向的是誰，需要等到執行的時候才能夠確定。

也就是說，this 這個值是在函式呼叫時才確定的，而不是在函式定義的時候確定的。

以下分為幾個 this 的幾種判斷

1. 全域上下文：在非嚴格模式下，全域上下文中的 this 指向全域物件 (在瀏覽器中是 window 物件、在 Node.js 環境下是 global 物件)；在嚴格模式下，this 的值會是 undefined。
2. 物件方法：當函式做為某個物件的方法被呼叫時，this 指向的是那一個物件。
3. 建構函式：在使用 new 關鍵字呼叫建構函式時，this 所指向的是新創建的實例。
4. 箭頭函式：箭頭函式中的 this 會從他所在的外圍非箭頭函式中繼承。

## 1. 全域上下文

在非嚴格模式下，this 指向的是 window

```js
function foo() {
   return this;
}

console.log(foo());  // window
```


在嚴格模式下，this 為 undefined

```js
function foo() {
    return this;
}

console.log(foo());   // undefined
```

## 2. 物件方法

```js
let obj = {
   x: "bar",
   foo: function() {
      console.log(this.x)
   }
}

obj.foo();  // "bar"
```


## 3. 建構函式

```js
var x = "Outer"

function Temp() {
   this.x = "bar"
}

var obj = new Temp()
console.log(obj.x);   // "bar"
```

## 4. 箭頭函式

技巧：他的外層沒有函式，this 會是 window；外層有函式，看外層函式的 this 是誰

情況 1:

```js
var name = 'window'

var A = {
   name: "A",
   sayHi: () => {
      console.log(this.name)
   }
}

A.sayHi();  // window
```

情況 2:

```js
var name = "window"

var A = {
   name: 'A',
   sayHi: function() {
      var s = () => console.log(this.name)
      s()
   }
}


A.sayHi();  // A
```
