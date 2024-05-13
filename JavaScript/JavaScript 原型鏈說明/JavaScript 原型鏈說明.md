# JavaScript 原型鏈說明

## 原型鏈說明
當試圖存取一個物件上面的方法或是屬性時，JavaScript 首先會在該物件上面尋找，若沒有找到，就會沿著原型鏈往上面去找，直到找到或是抵達原型鏈的終點 (`Object.prototype.__proto__`)

## 原型鏈式如何運作的
在 JavaScript 中，每個物件都有一個名為 `__proto__` 的內部屬性，此屬性是指向物件原型的一個連結(也就是從哪一個物件繼承屬性與方法)。

這邊的物件還有一點要注意的是，函式也屬於一個物件，讓函式除了可以被呼叫之外，還可以像其他物件一樣擁有屬性與方法，所以函式也有 `__proto__` 的內部屬性。

前面提到函式也是一個物件，是 Function 的實例，因此繼承自 Function.prototype，這也是為什麼函式有 apply、call 這些方法，因為這些方法都定義在 Function.prototype 上面。

這邊的原型鏈要分為函式與物件實例分開說明會比較好。

## 函式原型鏈解析
建立一個 myFunction 的函式，程式碼如下：

```js
function myFunction() { }
```

> 原型鏈解析:
> - `myFunction.__proto__` 指向 `Function.prototype` (因為所有函式都是繼承自 Function.prototype)
> - `Function.prototype.__proto__` 會在指向 `Object.prototype`
> - `Object.prototype.__proto__` 最終為 null，是原型鏈的終點

## 物件原型鏈解析
建立一個 Car 的建構函式，程式碼如下：

```js
function Car(brand, model) {
  this.brand = brand;
  this.model = model;
}

let car = new Car("Tesla", "Model X");
```

> 原型鏈解析：
> - `car.__proto__` 指向 `Car.prototype`
> - `Car.prototype.__proto__` 指向 `Object.prototype`
> - `Object.prototype.__proto__` 最終為 null，是原型鏈的終點

### 為什麼 `Car.prototype.__proto__ != Function.prototype ` 呢?
先說結論，由於 `Car.prototype` 是一個普通的 JavaScript 物件，他需要從 `Object` 繼承基本的物件行為，而非函式行為。

`Function.prototype` 是所有函式的原型。如果 `Car.prototype.__proto__` 指向了 `Function.prototype`，那麼透過 Car 建構函式建立的每個實例都將能訪問函式特有的方法，例如： call()、apply() 和 bind()，這就不是我們通常所期望的。

```js
function Car() { }

let car = new Car();
car.bind()  // undefined
```

`Car.prototype` 是當您用 new Car() 建立物件時，這些新物件的原型。這個 prototype 物件本身是一個普通的 JavaScript 物件。因此，它的原型 (`__proto__`) 不是指向 `Function.prototype`，而是指向 `Object.prototype`，這是所有普通 JavaScript 物件的基本原型。這種設定允許從 Object 繼承基本的物件功能，如 toString() 或 hasOwnProperty()。

## constructor 屬性
每個函式的 prototype 物件，會有一個 constructor 屬性，這個屬性會指向原本的函式
```js
function Car() {}
console.log(Car.prototype.constructor === Car) // true
```