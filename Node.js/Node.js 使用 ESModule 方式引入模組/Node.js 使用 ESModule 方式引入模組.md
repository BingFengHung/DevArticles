# Node.js 使用 ESModule 方式引入模組

## 前言

在 Node.js 中縣再支援兩種模組引入方式，一種是 CommonJS 風格，另一種是 ESM 的方式。

CommonJS 是使用 require、module.exports、exports 的方式；ESM 的話是使用 import、export、export default 的方式，來將模組導入與導出。

本篇將針對 ESM 導入與導出進行介紹

## ESM

ES6 可以使用 ESM 的方式導入與導出模組。

在這邊有兩種做法

1. 將 js 檔案副檔名改為 **mjs**
2. 修改 package.json 檔案，將 type 改為 module

首先說明第一種，將 js 副檔名改為 mjs 的方式，當 Node.js 碰到 .mjs 副檔名的檔案的時候，他就會認為他是 ESM 模組，然後我們就能夠直接 node test.mjs 檔案去執行檔案。

```js
// hello.mjs
export function Hello() {
   console.log("Hello World")
}

// test.mjs
import { Hello } from './hello.mjs'
Hello();   // Hello World
```

第二種方式，修改 package.json 檔案，將 type 改為 module

如果說不想要將 js 副檔名改為 mjs，可以在 package.json 檔案中設定 "type": "module"，設定完成之後，就能夠像是一般執行那樣呼叫即可 node test.js。

package.json

```json
{
   "type": "module"
}
```

程式碼

```js
// hello.js
export function Hello() {
   console.log("Hello World")
}

// test.mjs
import { Hello } from './hello.js'
Hello();   // Hello World
```


### 參考連結

[Node.js 如何处理 ES6 模块 - 阮一峰的网络日志 (ruanyifeng.com)](https://www.ruanyifeng.com/blog/2020/08/how-nodejs-use-es6-module.html)
