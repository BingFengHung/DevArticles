# JavaScript Array.from 介紹

**Array.from()** 可將類陣列物件 (array-like) 或是可迭代的物件 (iterable) 轉換為 Array 實體。

## 什麼是類陣列物件 (array-like)?
類陣列物件 (物件具有 length 屬性以及索引化 (indexed) 的元素)

ex. arguments 物件、字串、或是 DOM 操作返回的 NodeList 物件。

檢查一個物件是否為類陣列物件

```js
function isArrayLike(obj) {
  return obj !== null && typeof obj === 'object' && 
         isFinite(obj.length) && obj.length >= 0 && 
         obj.length === Math.floor(obj.length) && obj.length < 4294967296;
}
```

上面程式碼中的檢查條件
- 物件不是 null 並且是物件型別
- length 屬性是有限 (不是無限大、NaN 或負無限大)
- length 屬性為非負整數
- length 屬性值小於 2^32，這是 js 中陣列最大長度的限制

## 什麼是可迭代的物件 (iterable)?
可迭代物件 (物件具有可讓你利用迭代的方式取得他自己本身的元素)

ex. Map 與 Set

用一個函數檢查物件是否為可迭代的物件

```js
function isIterable(obj) {
  return obj !== null  && typeof obj[Symbol.iterator] === 'function';
}
```

上面函數檢查了
- 確保物件不是 null
- 檢查物件的 Symbol.iterator 屬性是否是一個函數