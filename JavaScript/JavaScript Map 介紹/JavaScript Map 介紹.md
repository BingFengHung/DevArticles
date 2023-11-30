# JavaScript Map 介紹
Map 是一種 key-value 的集合，與物件 (object) 類似，但有一些差別。
1. 與物件相比，Map 能夠使用任意類型的值作為 key，例如：object、number、string 等
2. Map 的 key 會根據加入的時間排序，不像 object 沒有順序性。
3. Map 的 key 是唯一的，如果設定到重覆的 key，那 Map 裡面舊的 value 會被覆蓋。
4. 透過 Map 的 size 屬性，可以得知 key-value 的數量。

Map 基本操作
```js
let map = new Map();

// 設定 key-value
map.set('key1', 1);
map.set('key2', 2);
```

// 取得 key 對應的 value
console.log(map.get('key1'));  // 1

// 檢查 key 是否存在
console.log(map.has('key1'));  // true

// 取得 Map 大小
console.log(map.size);  // 2

// 刪除 key-value
map.delete('key1');

// Map 迴圈
for(let [key, vlaue] of map) {
     console.log(`${key} = ${value}`)
}

// 清空 Map
map.clear();
```