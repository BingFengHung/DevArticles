# JavaScript 傳值與傳址
JavaScript 資料型態分為兩大類：原始型別 (primitive type) 與參考型別 (object、array、Map)

原始型別如下：
- Boolean
- Null
- undefined
- Number
- BigInt
- String
- Symbol

> 一般來說原始型別是 call by value，參考型別是 call by reference

## call by value
當變數是原始型別時，JavaScript 會用 call by value 的方式對變數內的值進行更新。

```js
let a = 10;
let b = a;
console.log(`a=${a}, b=${b}`);  // a=10, b=10

b = 20

console.log(`a=${a}, b=${b}`);  // a=10, b=20
```

當 b 被初始化的時候，JavaScript 會分配一塊新的記憶體位置，並複製變數 a 的數值到新的記憶體中。

## call by reference
傳址主要用於物件與陣列等複合類型的變數。將物件或陣列作為參數傳給函數的時候，傳遞的是記憶體位置。因此，在函數內對此物件或陣列進行修改的時候，會影響到原先的物件或是陣列。

```js
function changeObj(obj) {
   obj.name = "Jack"
}

const person = { name: "Joe" } 
changeObj(person)

console.log(person.name)  // Jack
```

## call by sharing
當參考型別作為參數傳遞給函式時，如果該參考型別在函式中有被重新賦值，外部的物件內容是不會被修改的。

```js
let obj1 = { name: 'Joe' }

function changeName(obj) {
   obj = { name: 'Jack' }
}

changeName(obj1);

console.log(obj1.name);  // Joe
```

如果是 array 的話情況也是一樣的
```js
let arr = [1, 2, 3, 4]

function changeArray(ary) {
   ary = [6, 7]
}

changeArray(arr);

console.log(arr);  // [1, 2, 3, 4]
```

## 總結
- call by value: 適用於原始型別，傳遞的是變數的副本
- call by reference：適用於複合型別，傳遞的是變數的引用
- call by sharing：函數內只能改變引用所指向的物件的屬性或元素，不能改變引用本身。