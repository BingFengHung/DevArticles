# Node.js 連接到 MongoDB 伺服器

本篇將建立 Node.js 專案來對 MongoDB 伺服器進行操作

## 專案建置

這邊我使用的是 pnpm 套件管理工具，要安裝的套件有

- mongodb
- typescript
- ts-node
- nodemon

首先建立一個資料夾 mongo_db，然後執行專案初始化指令

```sh
pnpm init
```

安裝套件

```sh
pnpm add typescript mongodb
pnpm add -D nodemon ts-node
```

安裝完成之後，先在專案根目錄底下建立一個 src 的資料夾，我們所寫的原始碼會放在裡面

然後再 src 資料夾裡面再建立一個 index.ts 的檔案

好了之後使用指令建立 tsconfig.ts 的檔案

```sh
npx tsc --init
```

產生 tsconfig.json 檔案之後，只需在裡面修改一個屬性值 outDir，改為 "outDir": "./dist"

主要要讓 typescript 編譯出來的檔案放到 dist 資料夾裡面

在來打開 package.json，將裡面的內容如下設定

```json
{
  "name": "mongo_db",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "build": "tsc",
    "dev": "nodemon"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "dependencies": {
    "mongodb": "^6.3.0",
    "typescript": "^5.3.3"
  },
  "devDependencies": {
    "nodemon": "^3.0.2",
    "ts-node": "^10.9.2"
  },
  "nodemonConfig": {
    "watch": ["src"],
    "ext": "ts",
    "exec": "ts-node src/index.ts"
  }
}
```

裡面主要是修改 scripts 以及 nodemonConfig 的設定

以上為開發專案的基本設定

## 撰寫 mongodb 操作

上面的步驟都設定完成後，就可以開始撰寫 mongodb 的相關操作了

打開 index.ts 檔案

```ts
import { MongoClient } from "mongodb";

// mongodb 連線字串
const uri = "mongodb://root:1234@localhost:27017"

const client = new MongoClient(uri);

async function run() {
  try {
    await client.connect();
    console.log("connect successfully")
    const db = client.db("test");
    const collection = db.collection("todo");

    const results = await collection.find().toArray();
    console.log(results)
  } finally {
    await client.close();
  }
}

run().catch(console.dir)
```

然後執行 pnpm run dev 指令

以上即為 Node.js 操作 MongoDB 伺服器的專案設定與操作
