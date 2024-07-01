# TypeScript 專案建立步驟

1. 建立專案資料夾
```bash
mkdir ts-proj
cd ts-proj
```

2. 專案初始化
```bash
npm init -y
```

3. 安裝 TypeScript 套件
```bash
npm install --save-dev typescript ts-node
```

4. 修改 package.json 檔案
```json
script: {
  "build": "npx tsc",
  "start": "ts-node index.ts"
}
```

4. 建立 TypeScript 配置檔案
建立 tsconfig.json 的 TypeScript 專案配置檔案

```bash
npx tsc --init
```

修改成想要的專案配置

```json
{
  "compilerOptions: {
    "outDir": "./dist",
    // 其他編譯配置...
  },
  "include": [
    "./src/**/*"
  ],
  "exclude": [
    "node_modules",
    "**/*.spec.ts"
  ]
}
```

5. 編譯 TypeScript

```bash
npx tsc
```