# Vue 聲明式與命令式的差別
## 聲明式 vs 命令式
先說說命令式，在以前如果我們要操作 html 元素，或是需要創建元素，我們需要在 JavaScript 腳本中，使用 **document.createElement** 等方法來建立元素，建立完元素之後，可能要根據判斷決定它裡面是不是要塞字串或者其他的 html 元素，或是後續需要做事件的監聽，都需要一步一步手動的建立出來。

相比於命令式，Vue 的聲明式是讓我們只需要使用 Vue 所提供的方法，它就會幫助我們自動的產生對應的 DOM 元件，以及事件的綁定，這麼做的好處是，我們只需要專注於描述應用的狀態與 UI 介面，因為 Vue 會自動幫我們處理 DOM 的更新與操作。

## 程式碼比較
下面是聲明式與命令式的程式碼比較：

### 命令式
```html
<div id="app"></div>

<script>
   const app = document.getElementById('app');
   const message = document.createElement('div');
   message.textContent = 'Hello, world!';
   app.appendChild(message);
</script>
```

### 聲明式 (Vue)
```html
<div id="app">
  {{ message }}
</div>
<script setup>
  const message = ref('')
  message.value = 'Hello, world!';
</script>
```

從上面的例子就能看出，自行使用 document 物件來建立元素操作有多麻煩，現在 Vue 幫你處理後面這些事情，讓我們只需要專注在我們想要呈現甚麼樣的畫面，以及確保程式業務邏輯符合我們需求即可。