# Node.js path 模組基本概念

## path 模組

| 函數與屬性      | 說明                                           |
| --------------- | ---------------------------------------------- |
| path.resolve()  | 處裡路徑片段並結合當前工作目錄返回一個絕對路徑 |
| path.join()     | 拼接路徑片段                                   |
| path.parse()    | 解析檔案路徑並返回一個 path 的物件             |
| path.basename() | 取得路徑內的檔案名稱                           |
| path.dirname()  | 取得路徑內的資料夾路徑                         |
| path.extname()  | 取得路徑內的檔案副檔名                         |
| path.sep        | 取得作業系統的路徑分割符號                     |


## path.join 與 pathw.resolve 差別

pathj.join 只會串接前一個參數，並不會將路徑轉為絕對路徑。
path.resolve 會根據當前的工作目錄與給定的路徑，最後產生一個絕對路徑。

1. path.join：

- path.join 會將所有提供的路徑片段串接起來，不僅僅是前一個參數。它會按照順序處理每個參數，並用系統特定的路徑分隔符來連接它們。
- 它不會將路徑轉換為絕對路徑。而是根據提供的片段返回一個正規化的路徑，這個路徑可能是相對的或絕對的，取決於給定的片段。

2. path.resolve：

- path.resolve 會從右到左處理給定的路徑片段，結合當前工作目錄來產生一個絕對路徑。
- 如果處理過程中遇到絕對路徑，則會立即使用該絕對路徑並停止進一步解析。

因此，path.join 更像是一個簡單的路徑串接工具，而 path.resolve 則提供更複雜的功能，它能夠根據當前工作目錄和給定路徑的相對性質來解析出一個絕對路徑。這是兩者之間最主要的差異。

## 絕對路徑與相對路徑
Node.js 的 path 模組會根據作業系統來處理路徑。

絕對路徑: 當路徑中有 / 開頭就代表的是絕對路徑
相對路徑: 當路徑中有 ./ 開頭代表會是相對路徑的方式

在 Windows 中，根目錄的概念與 Unix 或 Linux 系統不同。

Unix/Linux 系統有一個單一的根目錄 /，而 Windows 則為每個磁碟機（如 C:, D:, 等）設定了獨立的根目錄（如 C:, D:\）。

因此，當在 Windows 環境中使用 Node.js 的 path.resolve('/') 時，它會根據當前的磁碟機（通常是 C:）解析為該磁碟機的根目錄。

這種行為體現了 Node.js 的跨平台設計，它會根據不同的作業系統行為進行相應的路徑解析。這就是為什麼在 Windows 上 path.resolve('/') 會解析為 C:\，而在 Unix/Linux 系統上會解析為 /。

## 程式碼

### path.resolve()
使用 path.resolve 的時候參數內沒有對應的絕對路徑，會以當前的工作目錄進行串接，產生絕對路徑。

```js
const path = require('path')

path.resolve('test');  // C:\User\test
```

當 path.resolve 參數中有絕對路徑的話，要特別注意，resolve 在解析路徑的時候，會從函式帶入的參數，由`右到左`去搜尋是否有以 '/' 開頭的絕對路徑，如果有的話會直接把它當作是絕對路徑，然後才繼續往右找是否有沒有以 '/' 開頭的路徑，如下面的範例:

```js
const path = require('path')

path.resolve('test', '/temp', '/dummy') // C:\dummy
path.resolve('test', '/temp', 'dummy') // C:\test\dummy
```

### path.join()
path.join 會由左到右串接給定的參數

```js
const path = require('path')

path.join('test', 'abc')  // test\\abc
```

### path.parse()
解析給定的路徑，並返回以下參數
- dir
- root
- base
- name
- ext

```js
const path = require('path')

path.parse('C:\\test\\dir\\temp.txt')

// {
//    root: 'C:\\',
//    dir: 'C:\\test\\dir',
//    base: 'temp.txt',
//    ext: '.txt',
//    name: 'temp',
// }
```

### path.basename()
basename 回傳路徑的最後一個部分

```js
const path = require('path')

path.basename('C:\\test\\dir\\temp.txt') // temp.txt
path.basename('C:\\test\\dir\\temp.txt', '.txt') // temp  (移除副檔名)
```

### path.dirname()
取得路徑內的資料夾路徑

```js
const path = require('path')

path.dirname('C:\\test\\dir\\temp.txt') // C:\\test\\dir
```

### path.extname()
取得指定路徑檔案的副檔名

```js
const path = require('path')

path.extname('C:\\test\\dir\\temp.txt') // .txt
```
### path.sep
```js
const path = require('path')

console.log(path.sep)  // Windows: \ Linux: /
```