# JavaScript iterable 與 enumerable 如何區分
## 前言
可迭代 iterable: 適用於值
可列舉 enumerable：適用於物件屬性

## 可迭代 iterable
當一個物件是 iterable 的話，代表他有一個 iterator。

如果是 iterable 物件的話，可以使用 for...of 的迴圈來訪問 iterable 物件。

```js
const animals = ["cat", "dog", "bird"]

for (const animal of animals) {
  console.log(animal);
}

// cat
// dog
// bird
```
有哪一些可以進行迭代呢? 
> Array、Set、Map、String

## 可列舉 enumerable

如果是 enumerable 物件的話，代表的是走訪物件的所有屬性
```js
cosnt person = {
  age: 10,
  name: "Joe",
  gender: "male"
}

for (const key in person) {
   console.log(key)
}

// age
// name
// gender
```

## 可迭代 iterable
可迭代代表一個物件有實作 Symobl.iterator 方法，這個方法會返回一個迭代器 (iterator) 物件。

這個物件裡面有一個 next 方法，用來返回當前位置的值的物件，這個物件有兩個屬性：value 與 done。
當 done 為 true 的時候，就代表到達最後了，value 則是當前位置的值。

## 可列舉 enumerable
除了 null、undefined 以及此物件的屬性有明確指定不可列舉，否則任何物件的屬性都是可以被列舉的。

## 讓物件也能夠 iterable
只要物件上面具有可迭代器的話，那麼這個物件也就是 iterable 的，就能使用 for...of 進行迭代。

```js
let iterObj = {
   name: "Joe",
   age: 10, 
   hobby: ["music", "baseball", "basketball"],
   [Symbol.iterator]: function() {
      let index = 0
      let values = this.hobby;
      return {
         next: () => 
          ({ value: values[index++],
            done: index > values.length 
          })
      }
   }
}

for (let v of iterObj) {
   console.log(v)
}
// music
// baseball
// basketball
```

## 參考資料
https://openhome.cc/zh-tw/javascript/es-basics/iterator/