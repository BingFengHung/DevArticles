# Python Pandas 套件介紹與安裝
Pandas 是一個開源的 Python 套件，主要用來操作與分析資料。由於 Pandas 在底層使用了 Cython 與 C 語言的優化，因此在處裡大型資料時，仍能夠保持良好的性能。

Pandas 可以處理大型數據，並轉為結構性資料，也可快速產生視覺化圖表，幫助使用者理解資料內容。

Pandas 用來做簡單的資料清理，包含資料查詢、缺失值檢查等。

Pandas 資料來源可以是串列、元組、字典、Numpy，並且能夠讀取來自 csv、excel、sql、json、html 等的資料。

Pandas 簡單來說就是把 Excel 的表格觀念丟到 Python 裡面，在 Excel 上面的所有操作都可以透過 Pandas 的函式做簡單的處裡，像是欄位的加總、分群、樞紐分析表、畫圓餅圖等，再學會 Pandas 之後，就能用 Python 取代 Excel 做到許多自動化腳本的處理。

Pandas 是基於 Numpy，Numpy 是支援陣列的，陣列與 list 相比，當資料量很大時，使用陣列的效率會比較高。其原因為，陣列裡面的資料型別必須相同，list 的話能夠放置不同的型別資料，這導致效能會比同一型別的陣列還要差。


## 環境建立
通常習慣會建立一個虛擬環境來開發 Python 應用程式

```sh
python -m venv <name>
cd <name>

" 啟動環境
<name>\Script\Activate.ps1
```

安裝 Pandas 套件

```sh
pip install pandas
```

確認 pandas 是否有安裝成功

```py
import pandas as pd
pd.__version__
```
