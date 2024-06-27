# JavaScript this 關鍵字
## 前言
之前對於 JavaScript 的 this 關鍵字，指向的到底是誰搞不清楚，最近好好的研讀了一下，終於將 this 關鍵字搞明白；為此，趕緊筆記下來以免過一段時間又忘記。

## What is this?
通常 **this** 的值是由呼叫的物件所決定的。不過由於箭頭函式作法有點不太一樣，所以在另外拉出來做特例討論。

一開始提到 this 的值，是由呼叫的物件所決定的

### 物件方法呼叫方法
當函式做為某個物件的方法被呼叫的時候，this 所指向的會是該物件本身。

```js
const person = { 
   name: "Joe",
   greet() {
      console.log(`Hello, ${this.name}`)
   }
}

person.greet()  // Hello, Joe
```

### 在全域環境下呼叫
這邊有一點要特別注意，如果是在非嚴格模式下，全域環境下的 this 會指向全域物件 (瀏覽器的話是 window 物件；Node.js 環境下是 global 物件)；如果是在嚴格模式下，this 會是 undefined。

**非嚴格模式**
```js
var name = "Joe"

function greet() {
   console.log(`Hello, ${this.name}`)
}

greet()  // Hello, Joe
```

由於直接呼叫 greet() 方法，但是實際上它算是 window 物件的其中一個方法，所以本質上來說他其實還是由 window 所呼叫，因此 this 對應到的會是 window 物件。

**嚴格模式**
```js
"use strict"
var name = "Joe"

function greet() {
   console.log(`Hello, ${this.name}`)
}

greet()
/*
Uncaught TypeError: Cannot read properties of undefined (reading 'name')
    at greet (<anonymous>:5:31)
    at <anonymous>:8:1
*/
```

由於在嚴格模式下的 this 是 undefined，所以根本無法讀到 name 屬性。

### 建構函式中的 this
當使用 new 關鍵字建立物件的時候，建構函式中的 this，所指向的會是新建立的物件實體。

```js
function Car(brand, model) {
   this.brand = brand;
   this.model = model
}

const car = new Car("Tesla", "Model Y");
console.log(car);
// Car {brand: 'Tesla', model: 'Model Y'}
```

### 箭頭函式中的 this
箭頭函式的 this 會從外部函式取得，如果他的外部函式也式箭頭函式，那他就會繼續往外找，最終會找到 window 物件。

**技巧：他的外層沒有函式，this 會是 window；外層有函式，看外層函式的 this 是誰**

**情況 1:**

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

**情況 2:**

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

### setTimeout 的 callback 方法裡的 this
setTimeout 函式接收的方法，如果是 function 形式或是箭頭函式的形式，兩種情況下的 this 會是不同的東西

**function 形式**
```js
function Car(brand, model) {
   this.brand = brand 
   this.model = model 
   
   setTimeout(function() {
      console.log(this)  // 會印出 window
   }, 1000);
} 

const car = new Car("Tesla", "Model Y");
```

**箭頭函式形式**
```js
function Car(brand, model) {
   this.brand = brand 
   this.model = model 
   
   setTimeout(() => {
      console.log(this)  // 會 car 這個物件
   }, 1000);
} 

const car = new Car("Tesla", "Model Y");
```

## 作為 DOM 事件處理器中的 this
如果一個函式做為事件處裡器的話，this 的值會設定為觸發事件的元素
```js
function onClick(e) {
   console.log(this)
   e.stopPropagation()
}

var elements = document.getElementsByTagName("*")

for (let i = 0; i < elements.length; i++) {
   elements[i].addEventListener("click", onClick)
}
```

如果使用箭頭函式他會去指到 window 物件
```js
var elements = document.getElementsByTagName("*")

for (let i = 0; i < elements.length; i++) {
   elements[i].addEventListener("click", (e) => { 
      console.log(this) 
      e.stopPropagation()
   })
}
```

如果是直接在 DOM 元素上面定義的話，this 就會指向該 DOM 元素

```html
<button onclick="alert(this);">Click</button>
```

## 結論
以下分為幾個 this 的幾種判斷

1. 全域環境：在非嚴格模式下，全域上下文中的 this 指向全域物件 (在瀏覽器中是 window 物件、在 Node.js 環境下是 global 物件)；在嚴格模式下，this 的值會是 undefined。
2. 物件呼叫方法：當函式做為某個物件的方法被呼叫時，this 指向的是那一個物件。
3. 建構函式：在使用 new 關鍵字呼叫建構函式時，this 所指向的是新創建的物件實例。
4. 箭頭函式：箭頭函式中的 this 會從他所在的外部非箭頭函式中繼承，如果找不到外部函式那會一直往上找。