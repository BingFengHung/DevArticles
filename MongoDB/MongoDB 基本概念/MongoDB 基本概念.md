# MongoDB 基本概念

## 前言

本篇主要針對 MongoDB 的操作做一個簡單的介紹，並且會使用 mongosh 這個工具對 mongodb 進行操作；這邊將g使用 docker 來建立 MongoDB 的測試伺服器。

## MongoDB 是什麼?

MongoDB 是一種以文件為導向來儲存資料的資料庫，是一種非關聯資料庫 (NoSQL)。

MongoDB 的優點是**易開發、擴展**

## MongoDB 核心概念

Database -> Collection -> Document

- Database: 在 MongoDB 伺服器中，會有一至多個 Database。
- Collectino: 可以當作是資料表
- Document: 資料表裡面的一筆資料。

**Document 類似 js 的物件**

Document 是由多組 key-value 組成

## 建立 MongoDB Server

1. 拉取 mongo images

```sh
docker pull mongo
```

2. 啟動 mongo 容器

```sh
docker run --name MongoDB -p 27017:27017 --rm -e MONGO_INITDB_ROOT_USERNAME=root -e MONGO_INITDB_ROOT_PASSWORD=1234 -itd mongo
```

## 使用 docker exec 進入 MongoDB 的容器

進入 MongoDB 容器，並使用 mongosh 工具

```sh
docker exec -it MongoDB bash

mongosh -u root -p
```

## MonogoDB 語法操作

### 顯示所有的 database

使用 `show dbs` 指令的時候可以顯示所有的 database，不過如果某個 database 裡面是空的話，他就不會列出來。

### 使用或是創建 database

`use test;`

當指定的 database 不存在時就會自動創建

### 建立 collection

- 方法 1
  db.createCollection("todo");

- 方法 2
  db.posts.insertOne({"Title": "Fix bug"})

### 插入 document

- insertOne(document, options)
  如果要插入單一筆 document 使用 insertOne 方法
  ```sh
  db.todo.insertOne({"Title": "Bug Fixes", "Desc": "Found a big bug in code", "Time": new Date("2099-12-31"), "Urgent": 1});
  ```

  若新增成功會回傳

  ```js
  {
    acknowledged: true,  // true 表示成功 
    insertedId: ObjectId('6578ffee97823f675df321de')  // document 的 id
  }
  ```

- insertMany(documents, options)
  插入多筆 document

  ```sh
  db.todo.insertMany([
     {"Title": "Bug Fixes", "Desc": "Found a big bug in code", "Time": new Date("2099-12-31"), "Urgent": 1}, 
     {"Title": "Buy video game", "Desc": "New game release", "Time": new Date("2090-12-31"), "Urgent": 5}, 
     ]);
  ```

  回傳值

  ```js
  {
    acknowledged: true,
    insertedIds: {
      '0': ObjectId('657900ca97823f675df321df'),
      '1': ObjectId('657900ca97823f675df321e0')
    }
  }
  ```

### 找尋 document
- 顯示所有的 documents
  ```sh
  db.todo.find()
  ```
  回傳
  ```js
  [
    {
      _id: ObjectId('657900ca97823f675df321df'),
      Title: 'Bug Fixes',
      Desc: 'Found a big bug in code',
      Time: ISODate('2099-12-31T00:00:00.000Z'),
      Urgent: 1
    },
    {
      _id: ObjectId('657900ca97823f675df321e0'),
      Title: 'Buy video game',
      Desc: 'New game release',
      Time: ISODate('2090-12-31T00:00:00.000Z'),
      Urgent: 5
    }
  ]
  ```

- 顯示找到的第一筆資料 
  ```sh
  db.todo.findOne()
  ```

  回傳值
  ```js
  {
    _id: ObjectId('6578ffee97823f675df321de'),
    Title: 'Fix buges',
    Desc: 'Found a big bug in code',
    Time: ISODate('2099-12-31T00:00:00.000Z'),
    Urgent: 1
  }
  ```

- 如果想要找尋指定的資料，後面直接帶參數條件
  ```sh
  db.todo.find({Title: "Buy video game"})
  ```
  回傳值
  ```js
  [
    {
      _id: ObjectId('657900ca97823f675df321e0'),
      Title: 'Buy video game',
      Desc: 'New game release',
      Time: ISODate('2090-12-31T00:00:00.000Z'),
      Urgent: 5
    }
  ]
  ```

- 如果說搜尋結果只想要顯示指定的 fiedls，後面可以帶第二個參數，此參數叫做 projection
  ```sh
  db.todo.find({Title: "Buy video game"}, {Title: 1, Urgent: 1})
  ```
  
  回傳值
  ```js 
  [ 
    {
      _id: ObjectId('657900ca97823f675df321e0'),
      Title: 'Buy video game',
      Urgent: 5 
    } 
  ]
  ``` 
  
  第二個參數中的 1 代表是要引入這個 field 如果是 0 的話代表要排除這個 filed

  上面的範例中的第一個參數帶有指定搜尋條件的，如果想要全部尋找的話，給一個 {} 即可

### 更新 document
- updateOne() 更新第一筆 document 匹配的資料
  ```sh
  db.todo.updateOne({Title: "Bug Fixes"}, {$set: {Urgent: 2}})
  ```

  回傳值
  ```js
  {
    acknowledged: true,
    insertedId: null,
    matchedCount: 1,
    modifiedCount: 1,
    upsertedCount: 0
  }
  ```

  注意需要使用 $set 運算子

  如果說原本要更新的資料沒有找到，可以直接插入資料
  
  ```sh
  db.todo.updateOne(
    { Title: "Wash Dishes" },
    {
      $set:
        {
          Title: "Wash Dishes",
          Desc: "Oh, no!",
          Time: Date(),
          Urgent: 5
        }
    },
    { upsert: true }
  )
  ``` 
  需要使用 upsert 選項
  
  回傳值
  ```js
  {
    acknowledged: true,
    insertedId: ObjectId('6579047385d061e07fe7c531'),
    matchedCount: 0,
    modifiedCount: 0,
    upsertedCount: 1
  }
  ```

- updateMany 更新多筆資料
  ```sh
  db.todo.updateMany({}, { $inc: { Urgent: 1 } })
  ```
  
  回傳值
  ```js
  {
    acknowledged: true,
    insertedId: null,
    matchedCount: 4,
    modifiedCount: 4,
    upsertedCount: 0
  }
  ```

  使用 $inc 運算子對所有 document 的 Urgent 屬性值 + 1

### 刪除 document
- deleteOne() 刪除一筆資料
  ```sh
  db.todo.deleteOne({ Title: "Wash Dishes" })
  ```
  回傳值
  ```js
  { acknowledged: true, deletedCount: 1 }
  ```

- deleteMany() 刪除多筆資料
  ```sh
  db.todo.deleteMany({ Time: new Date("2099-12-31") })
  ```
  回傳值
  ```js
  { acknowledged: true, deletedCount: 2 }
  ```