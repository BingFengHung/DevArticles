# 將 Vite/React 部屬到 GitHub Pages 上

如果是用 **npm create vite@latest** 的方式，想要將專案部屬到 GitHub Pages 上面僅需要幾個簡單的步驟即可完成

1. 安裝 gh-pages 套件
```bash
npm install gh-pages --save-dev
```

2. 修改 package.json 的 scripts
```json
"scripts" : {
  ...
  "generate": "npm run build",  
  "deploy": "gh-pages -d dist", 
}
```

3. 修改 vite.config.js 的檔案
```js
export default defineConfig({
  plugins: [react()],
  base: "HPOptions"
})
```

上面的設定之後，就可以開啟終端機先執行 npm run generate 指令，然後再執行 npm run deploy 指令就可以順利部屬到 GitHub Pages 上面了。