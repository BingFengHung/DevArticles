# ESLint 與 Prettier 專案設定
## 安裝套件說明
- Prettier (一個按鍵將雜亂的程式碼排列整齊)
  -  prettier: Prettier 核心套件
- ESLint (開發風格統一，當寫法與設定不相符時，會跳出錯誤警告)
  - eslint： ESLint 核心套件
  - @typescript-eslint/parser: 讓 ESLint 能夠解析 TypeScript 程式碼
  - @typescript-eslint/eslint-plugin: 包含一系列針對 TypeScript 的 ESLint 規則
- 整合 Prettier 與 ESLint
  - eslint-config-prettier: 關閉所有不需要或是與 Prettier 衝突的 ESLint 規則
  - eslint-plugin-prettier: 將 Prettier 規則作為 ESLint 規則來運行

## 套件安裝指令
> pnpm add -D eslint prettier eslint-config-prettier eslint-plugin-prettier @typescript-eslint/parser @typescript-eslint/eslint-plugin

## 專案配置
> npx eslint --init

在生成的 .eslintrc 檔案中（可能是 .eslintrc.js、.eslintrc.json 等格式），確保包含了 Prettier 的配置。例如：

```js
{
    "extends": [
        "plugin:@typescript-eslint/recommended",
        "prettier",
        "plugin:prettier/recommended"
    ],
    "parser": "@typescript-eslint/parser",
    "parserOptions": {
        "ecmaVersion": 2020,
        "sourceType": "module"
    },
    "plugins": [
        "@typescript-eslint",
        "prettier"
    ],
    "rules": {
        "prettier/prettier": "error"
        // ... 其他自定義規則
    }
}
```

創建一個 Prettier 的配置檔案 .prettierrc。這個檔案可以是 JSON 或 YAML 格式，也可以是一個可導出配置的 JavaScript 檔案。一個基本的 .prettierrc 示例：
```json
{
    "semi": true,
    "singleQuote": true,
    "tabWidth": 2,
    "useTabs": false,
    "trailingComma": "es5"
}
```

配置 package.json
```json
"scripts": {
    "lint": "eslint --ext .ts .",
    "lint:fix": "eslint --ext .ts . --fix",
    "format": "prettier --write \"**/*.{js,ts,json}\""
}
```
