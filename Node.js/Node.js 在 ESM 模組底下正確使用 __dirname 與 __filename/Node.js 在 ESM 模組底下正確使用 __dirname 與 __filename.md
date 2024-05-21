# Node.js 在 ESM 模組底下正確使用 __dirname 與 __filename

在 CommonJS 的模組風格底下，能使用 __dirname 取得當前的資料夾路徑以及 __filename  取得當前的檔案路徑，程式碼如下：

```js
console.log(__dirname)
console.log(__filename)

// output:
// dirname: C:\temp\index
// filename: C:\temp\index.js
```

如果換作是在 ESM 模組風格底下，會直接報錯

```js
console.log(__filename);

// ReferenceError: __filename is not defined in ES module scope
// This file is being treated as an ES module because it has a '.js' 
// file extension and 'C:\temp\package.json' contains "type": "module". 
// To treat it as a CommonJS script, rename it to use the '.cjs' file extension.
```

在 ESM 模組風格下需要另外處理，使用 import.meta.url 這個東西，能夠取得該檔案的 URL，然後再使用 url 模組底下的 fileURLToPath 轉成將 file url 格式轉成一般的路徑格式，就能順利取得該檔案的完整路徑，程式碼如下：

```js
import { fileURLToPath } from 'url'

const __filename = fileURLToPath(import.meta.URL);

console.log(__filename);  // C:\temp\index.js
```

當我們拿到完整的 __filename 的路徑之後，要拿到 __dirname 就簡單多了，引入 path 模組，然後使用 path.dirname() 方法，取得指定檔案路徑下的完整資料夾路徑，程式碼如下所示：

```js
import { fileURLToPath } from 'url'
import path from 'path'

const __filename = fileURLToPath(import.meta.URL);
const __dirname = path.dirname(__filename)

console.log(__dirname);  // C:\temp
```