# Python 快速查詢內建模組與關鍵字

## 列出內建的模組
要查詢 Python 本身內建的模組可以透過以下方式

開啟終端機輸入 Python 進入 REPL 模式

然後再裡面輸入 

```py
dir(__builtins__)
```

就能夠列出所有的內建類別與方法

如果僅需要內建的方法呈現可以用以下程式碼

```py
# 列出 __builtins__ 中所有的內建函數
builtin_functions = [f for f in dir(__builtins__)
                     if type(getattr(__builtins__, f)) == type(len)]

print(builtin_functions)
```

## 列出關鍵字
一樣是在 REPL 模式下面輸入以下

```py
import keyword
keyword.kwlist
```

## help
help 函式可用來查詢指定的方法與模組說明