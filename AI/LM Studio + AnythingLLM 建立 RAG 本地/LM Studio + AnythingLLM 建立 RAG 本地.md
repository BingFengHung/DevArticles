# LM Studio + AnythingLLM 建立 RAG 本地應用
LM Studio 是一款能夠在本地端使用 LLM 的應用，

AnythingLLM 是一款讓你不用寫程式，就能夠馬上把 LLM 拿來應用的工具，由於他是透過 API Key 的方式來呼叫 LLM 做事情，因此，你的電腦就算 GPU 不強，也是能夠拿來使用的。

LM Studio 裡面可以讓你直接選取模型，然後就可以開始進行對話

另外 LM Studio 也可以讓你直接在裡面開啟 Local Server，並且提供 API KEY 讓你串接你想要的服務

很多 AI 功能都是透過 API KEY 來提供指定模型的服務的，像是 Visual Code 裡面的 AI 程式碼提示功能，就需要 API KEY，此時就用本地端的 Server 來操作了。

- LM Studio 官網： https://lmstudio.ai/
- AnythingLLM 官網： https://useanything.com/


## LM Studio 操作
首先要先去下載模型，

到左邊的菜單選擇放大鏡的 icon，就可以開始搜尋需要的模型了

在畫面的上方，可以知道你的 RAM 與 GPU (VRAM) 有多大 

使用 LM Studio 的好處是，他會告訴你哪個模型在你這台電腦上面是可以本地運行的

下載完成之後

他有一個對話的 icon，可以馬上進行對話

旁邊有一些設定可以進行調整

## Anything LLM
要使用 Anthing LLM 的話我們需要 API KEY

這個 LM Studio 有提供，只需要開啟 Server 即可使用

上傳資料也很簡單

上傳完成之後一個簡單的 RAG 檔案問答機器人就完成了。

