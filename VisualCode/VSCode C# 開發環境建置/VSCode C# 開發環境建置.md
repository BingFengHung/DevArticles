# VSCode C# 開發環境建置
## 前言
VSCode 是一款好用的編輯器，裡面的開發套件也是相當的豐富，由於 Visual Studio 相比較算是更加功能豐富，但是有時候我只是需要開發一個簡單的功能，並不需要開啟向 Visual Studio 那麼龐大的程式，此時 VSCode 就算是比較合適的開發工具了。

## 必備工具
- dotnet cli
- vscode 延伸套件 C# Dev Kit

## 環境配置
我們需要讓程式能夠順利地在 VSCode 中順利的編譯與執行，會需要一個 .vscode 的資料夾在專案的根目錄，裡面會放置一些 lanuch.json 與 tasks.json 的檔案。

由於我已經測試好我所需要的專案配置了，所以我就直接放上來

.vsocde/lanuch.json 檔案內容如下：
```json
{
  // Use IntelliSense to learn about possible attributes.
  // Hover to view descriptions of existing attributes.
  // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
  "version": "0.2.0",
  "configurations": [
    {
      "name": "C#: JoeCLI Debug",
      "type": "coreclr",
      "request": "launch",
      "preLaunchTask": "build",
      "program": "${workspaceFolder}/src/joe/bin/Debug/net8.0/joe.dll",
      "args": [],
      "console": "integratedTerminal", 
      "cwd": "${workspaceFolder}",
      "stopAtEntry": false
    }
  ]
}
```

.vscode/tasks.json 檔案內容如下：

```json
{ 
    "version": "2.0.0", 
    "tasks": [ 
        { 
            "label": "build",
            "command": "dotnet",
            "type": "process",
            "args": [
                "build",
                "${workspaceFolder}/src/joe/joe.csproj"
            ],
            "problemMatcher": "$msCompile" ,
            "group": {
                "kind": "build",
                "isDefault": true
            }
        } 
    ]
}
```

配置好了之後，如果我們需要讓程式跑起來，可以直接按下 F5，那麼此專案就會先進行編譯的動作，然後執行程式。

如果我們只是單純的要編譯而已，我們也可以直接用 Ctrl + Shift + b 的組合鍵，他就會自動對專案進行編譯的動作。

## 結論
本篇簡單說明一些基本的配置，如果有其他需求再到微軟的官網上面查看要怎麼做額外的設定即可。