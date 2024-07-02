# JavaScript Object Descriptor 說明

在 JavaScript 中，Object 描述符 (descriptor) 用來定義或是修改物件的屬性。這些描述符可用 Object.defineProperty 與 Object defineProperties 方法中。屬性描述符有兩種類型：資料描述符 (data descriptor) 與存取描述符 (accessor descriptor)。`這兩者不能同時使用`。

### 資料描述符 (Data Descriptor)

用來定義物件屬性的值以及一些屬性的行為，包含以下幾個屬性：

- value：該屬性的值，預設為 undefined
- writable：該屬性的值是否可被修改，預設為 false
- enumerable：該屬性是否會在迭代物件的屬性時出現，預設為 false
- configurable：該屬性是否可以被刪除或修改屬性描述符，預設為 false

```js
let obj = {} 

// 定義資料描述符
Object.defineProperty(obj, 'test', {
  value: 42,
  writable: true,
  enumerable: true,
  configurable: true
})

console.log(obj.test)  // 42
```

### 存取描述符 (Accessor Descriptor)

用來定義物件屬性的 getter 與 setter 函式，包含以下幾個屬性：

- get：該屬性的 getter 函式，當屬性被訪問時呼叫，預設為 undefined
- set：該屬性的 setter 函式，當屬性被設定時呼叫，預設為 undefined
- enumerable：與資料描述符中的 enumerable 屬性相同
- configurable：與資料描述符中的 configurable 屬性相同

```js
let obj = {}

Object.defineProperty(obj, 'test', {
  get: function() {
    return this.test * 2;
  },
  set: function(value) {
    this.test = value / 2
  },
  enumerable: true,
  configurable: true
})

obj.test = 50
console.log(obj.test); // 50
obj.test = 100
console.log(obj.test);  // 100
```

## 針對 enumerable 與 configurable 進行深度說明

### enumerable

enumerable 屬性描述符是說針對該屬性是否會在列舉物件的屬性出現，例如 for...in 迴圈或是 Object.keys() 方法中

```js
let obj = {}

// 可列舉
Object.defineProperty(obj, 'canEnum' , {
  value: 10,
  enumerable: true
})

// 不可列舉
Object.defineProperty(obj, 'noEnum', {
  value: 20,
  enumerable: false
})

// 列舉物件
for (let key in obj) {
  console.log(key);  // 僅輸出 canEnum
}

console.log(Object.keys(obj))  // ["canEnum"]
console.log(obj.propertyIsEnumerable("canEnum"))  // true
console.log(obj.propertyIsEnumerable("noEnum"))  // false
```

### configurable

configurable 屬性描述符是說該屬性可否被刪除或是修改屬性描述符。若 configurable 為 false，則不能刪除該屬性或修改其屬性描述符 (除了 writable 可以從 true 改為 false)

```js
let obj = {}

// configurable 為 true
Object.defineProperty(obj, 'canConfig', {
  value: 30,
  configurable: true
});

// configurable 為 false
Object.defineProperty(obj, 'noConfig', {
  value: 40,
  configurable: false
})

// 嘗試刪除屬性
delete obj.canConfig
console.log(obj.canConfig)  // undefined (屬性被刪除了)

delete obj.noConfig
console.log(obj.noConfig)  // 40 (屬性沒有被刪除)

// 嘗試修改屬性描述符
Object.defineProperty(obj, 'noConfig', {
  value: 50,
  configurable: true  // 會拋出異常，因為此屬性不可被配置
})
```

## 問題

### 1. enumerable 如果是 true，迭代時是怎麼用的

> 針對有設定 enumerable 屬性為 true 的物件，再用 for...in 迴圈，或是 Object.keys 的時候，就會被列出來，反之，則不會出現

### 2. enumerable 如果是 true，那其他屬性也是可 enumerable 那會怎麼呈現

> 在使用 for...in 迴圈或是 Object.keys() 方法的時候，會被同時列出來。

### 3. 資料描述符就有 enumerable 與 configurable 屬性了，為什麼存取修飾符還要再有這兩個屬性

> 因為這兩個屬性描述符控制的是屬性本身的行為，並非屬性的具體數值或是存取方法
>
> 1. enumerable：無論該屬性適用於存取資料還是透過 getter/setter 方法進行操作，這對於屬性的可見性和迭代操作中的表現是一致的要求
> 2. configrable：決定屬性是否可以被刪除或其屬性描述符是否可以被重新定義。這對於保護屬性不被意外刪除或修改來說是必要的，無論該屬性是存取資料還是通過 getter/setter 方法操作。

enumerable 和 configurable 屬性在資料描述符和存取描述符中具有相同的意圖：控制屬性的可見性和可配置性。這些屬性對於保持屬性行為的一致性和可預測性至關重要，無論屬性是如何定義的。因此，它們在兩種類型的屬性描述符中都是必須的。

### 4. 使用 Object.defineProperty 與物件字面值差別在哪邊

> 使用 **物件字面值** 定義物件的屬性會自動將屬性設定為可列舉、可寫、可配置
> 使用 **Object.defineProeprty** 定義的屬性則可精確控制屬性行為。它讓你自行設定屬性的可配置性、可列舉性與可寫性，並且可用來定義 getter 與 setter 方法。
>
> **兩者的差異：**
> 屬性行為控制：
>
> - **物件字面值**：屬性是可枚舉、可寫、可配置的。
> - **Object.defineProperty**：可以精確控制屬性的 writable、enumerable 和 configurable 屬性。
>
> 靈活性：
>
> - **物件字面值**：適用於大多數簡單場景，語法簡潔。
> - **Object.defineProperty**：適用於需要更細微控制的情況，例如，定義只讀屬性、隱藏屬性或使用 getter/setter。
>
> 預設值：
>
> - **物件字面值**：所有屬性預設為 true。
> - **Object.defineProperty**：所有屬性預設為 false（除非明確的指定）。

### 5. 如果同時使用資料修飾符與存取修飾符會怎麼樣

> 在 JavaScript 中，同一個屬性不能同時使用資料描述符與存取描述符。如果嘗試同時設定這兩種類型的描述符，JavaScript 會拋出一個 TypeError 錯誤。
> 這兩種類型的描述符屬性是互斥的：如果一個描述符中同時存在 value 或 writable 和 get 或 set，這將被認為是無效的，因為這違反了描述符的定義。

### 6. Object.defineProperty 用在什麼場合

> 適合需要特別設定屬性行為的場景，如定義只讀屬性、隱藏屬性或使用 getter/setter。
