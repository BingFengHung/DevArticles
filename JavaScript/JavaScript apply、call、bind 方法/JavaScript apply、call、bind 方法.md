# JavaScript apply、call、bind 方法
## 前言
在 JavaScript 中，apply、call 與 bind 都是用來指明呼叫的物件到底是誰，避免被 this 搞混，以避免再呼叫的時候，使用錯誤的物件方法。

這些方法都很像，但還是可以做個區分:
- apply、call 是回傳 function 的執行結果
- bind 方法回傳的是綁定 this 後的函式

另外 apply 與 call 給定參數的方式不一樣
- apply 傳遞一個陣列
- call 參數各別傳遞

## apply
給定 this 參數以及一個**陣列形式的值組**
> fn.apply(thisArg, [arg1, arg2, ..., argn])

```js
const car = {
  Car(brand, model) {
    return `${this.owner} owned ${brand} ${model}`
  }
}

const tesla = {
  owner: "Joe"
}

car.Car.apply(tesla, ["Tesla", "Model Y"])  // Joe owned Tesla Model Y
```

## call
給定 this 參數以及**分別給定**的參數來呼叫某個函數

> fn.call(thisArg, arg1, arg2, ..., argn)

```js
const car = {
  Car(brand, model) {
    return `${this.owner} owned ${brand} ${model}`
  }
}

const tesla = {
  owner: "Joe"
}

car.Car.apply(tesla, "Tesla", "Model Y")  // Joe owned Tesla Model Y
```

## bind
bind() 方法，會回傳一個新的函式

> fn.bind(thisArg, arg1, arg2, ..., argn)

```js
const model = {
  x: 10,
  getX() {
    return this.x
  }
}

model.getX()  // 10

const myX = model.getX
myX()  // undefined

const bindGetX = model.getX.bind(model)
bindGetX();  // 10
```