# Node.js 簡易 middleware 機制實現
通常使用像是 Express.js 框架，它裡面有內建的 Middleware 註冊方式，讓使用者能夠很輕易的在請求與回應的流程中間加上一些返回給客戶端前執行一些邏輯，像是身分驗證、額外輸出訊息等功能。

## Middleware 基本性質
1. 執行順序：Middleware 是按照順序執行，讓你能在應用層級、路由層級或是錯誤處理層級加上 Middleware。
2. 請求處理：Middleware 函式參數有三個 (req, res, next)
3. 回應終止：Middleware 可以隨時終止請求，或是呼叫 next() 函式將控制權交給下一個 Middleware

## Middleware 類型
1. 應用層級 Middleware: 使用 app.use 進行註冊，作用於整個應用程式。ex: 紀錄請求的時間點
2. 路由層級 Middleware: 使用 router.use() 註冊，作用於特定的路由。 ex: 驗證某些陸游的請求
3. 錯誤處裡 Middleware: 用來捕捉何處理應用中的錯誤。有四個參數 (err、req、res、next)
4. 內建 Middleware: Express 提供一些內建的 Middleware 像是 express.static() 用來處理靜態檔案

以上是使用 express 框架的 Middleware，他幫你做很好了很多功能，讓你無須再自行手動建立，萬一想要使用 http 模組，硬去自行建立 Middleware 機制，應該要如何實作呢?

以下是實作 Middleware 的簡易程式碼

```js
import http from 'node:http'

const PORT = 8080

// Middleware 管理
const middleware = []

// 用於註冊 Middleware 的函式
function use(fn) {
   middleware.push(fn)
}

// Middleware 處裡函式
function requestHandler(req, res) {
   let index = 0;

   function next() {
      const fn = middleware[index++];
      if (fn) {
         fn(req, res, next);
      } else {
         res.end('Request processed')
      }
   }
 
   next()
}

// 註冊一些 Middleware
use((req, res, next) => {
   console.log("Middleware 1: Request recevied");
   next();
});

use((req, res, next) => {
   console.log("Middleware 2: processing recevied");
   next();
});


use((req, res, next) => {
   console.log("Middleware 3: Final");
   res.end('Hello world')
});


const server = http.createServer(requestHandler)

server.listen(PORT, () => {
   console.log(`Server listen on port: ${PORT}`);
})
```

上面只是簡單實現了 Middleware 的程式碼，並不是最好的做法，可參閱 express 的原始碼，了解一下專案的框架都是如何製作的。