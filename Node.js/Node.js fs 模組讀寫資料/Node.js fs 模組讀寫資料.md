# Node.js fs 模組讀寫資料

fs 模組在讀寫資料的時候，從 fs 的方法列表中就能看出他有分為同步的版本與非同步的版本，同步的版本會阻塞程式碼，只有在他順利完成裡面的程式碼才會繼續地往下執行；而非同步的版本會讓程式碼往下執行，當 操作檔案的 IO 完成的時候，會再透過 Callback 方法接續後面處裡的邏輯。

## 檔案讀取
以下程式碼將介紹同步讀取與非同步讀取的方式，在非同步讀取又可分為 Callback 的處理方式以及 Promise 的處理方式。
### 同步的方式讀取檔案

```js
import fs from 'fs';

const data = fs.readFileSync('./test.txt', 'utf8')
console.log(data);
```

###  非同步讀取檔案 (以 Callback 方式)

```js
import fs from 'fs';

fs.readFile('./test.txt', 'utf8', (err, data) => {
   console.log(data)
})
```

### 非同步讀取檔案 (promise 的寫法)

```js
import fs from 'fs/promises'

fs.readFile('./test.txt', 'utf8')
.catch((data) => console.log(data))
.catch((err) => console.log(err));
```

### 非同步讀取檔案 (async/await)

```js
import fs from 'fs/promises'

const readFile = async () => {
   try {
      const data = await fs.readFile('./test.txt', 'utf8')
      console.log(data)
   } catch (err) {
      console.log(err)
   }
}
```

## 檔案寫入
以下程式碼將介紹同步讀取與非同步寫入的方式，在非同步寫入又可分為 Callback 的處理方式以及 Promise 的處理方式。

### 同步方式寫入檔案
```js
import fs from 'fs';

// 假如檔案不存在它會自動創建一個檔案
fs.writeFileSync('./test.txt', 'This is write test'); 
```

### 非同步方式寫入檔案 (Callback)
```js
import fs from 'fs';

fs.writeFile('./test.txt', 'This is write test', (err) => {
   console.log(err)
}); 
```

### 非同步方式寫入檔案 (Promise)
```js
import fs from 'fs/promises';

fs.writeFile('./test.txt', "That's good!")
.then(() => console.log('success'))
.catch(err => console.log(err))
```

### 非同步方式寫入檔案 (async/await)
```js
import fs from 'fs/promises';

const write = async () => { 
  await fs.writeFile('./test.txt', "That's good too!")
};

write()
```

## 小結
上面分別說明了寫入與讀取檔案的操作方式，並且完整列出同步與非同步的操作方式。

在同步的情況下，在讀寫檔案的時候，會阻塞當前的程式碼，等到完成之後再繼續執行

在非同步的情況下，上面說明了三種撰寫方式
- Callback 
- Promise 
- async/await

以閱讀便利性來說，async/await 的方式會比較容易閱讀。