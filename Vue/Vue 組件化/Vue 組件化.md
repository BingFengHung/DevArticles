# Vue 組件化
## 什麼是組件化
Vue 組件化 (Component) 就是它可以讓使用者，用小塊小塊組件開發的方式，一開始只需要專注於小組件的 UI 與功能設計，在到最後將這些組件組合起來變成一個大型的應用程式。

## 組件化的好處
這麼做的好處有以下幾點：
1. 可重用性：組件可以在好幾個地方重複使用，減少重複的程式碼，提高開發效率，並且只需要針對一個組件做修改即可。
2. 獨立性：每個組件有自己獨立的範圍 (scope)，這讓組件之間的耦合度可以降低。
3. 模組化：每個組件放在單獨的檔案中，讓程式碼結構清晰。 
4. 易測試性：由於是獨立的單元，要針對某個組件進行測試是很方便的。

## Vue 定義組件的方式
如果有使用 Vue 的開發工具開發的話，可以使用 vue 的模板，一個 .vue 的副檔名

.vue 檔案裡面的結構包含以下三個：
- template：當作 html 即可
- script：寫 JavaScript 的地方
- style：定義 css 樣式的地方

檔案結構如下
```vue
<template>
</template>

<script>
</script>

<style>
</style>
```

### 實際程式碼

```vue
<template>
<!-- 在這邊寫 html -->
<div class='greet'>
  <h1>{{ hello }}</h1>
</div>
</template>

<script setup>
import { ref } from 'vue';
const hello = ref('')
hello.value = 'Hello, world!'
</script>

<style scoped>
.greet {
  font-size: 2rem;
  color: 'green';
}
</style>
```

這樣就是一個組件宣告完成了