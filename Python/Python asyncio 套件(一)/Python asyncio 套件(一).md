# Python asyncio 套件 (一)
要使用非同步程式設計，需要先引入 asyncio 的套件，並且搭配 async/await 關鍵字進行非同步程式設計。

async/await 主要用於處裡需要等待的任務，ex: 網路通訊。asyncio 運算核心是一個 Event Loop，Even Loop 就像是一個大腦，他會面對很多可以執行的任務，並決定要執行哪一個任務。

在 Python 裡面，同時執行的任務只能有一個，他不存在系統級別的上下文切換，他與執行緒不一樣，他需要主動告訴 Event Loop，我這邊結束了，可以讓別的任務開始執行。

## 使用 asyncio 之前
先理解以下三個名詞：  
- coroutine object
- coroutine function
- Task

首先介紹 coroutine function，在下面的程式碼中，async def main 就是一個 coroutine function，在 Python 中所有 async def 開頭的東西都叫做 corountine function，他定義了一個 coroutine 的過程

```py
import asyncio

async def main(): 
  print('hello')
  await asyncio.sleep(1) # 等 1 秒
  print('world')

asyncio.run(main())
```

所有用 async def 定義的函式，在呼叫後，返回的是一個 coroutine object。

以上面的例子來說，當我們直接呼叫 main() 函式的時候，他並不會執行 main 裡面的邏輯，他只會返回一個 coroutine object，並不會執行裡面的程式碼。

那要怎麼執行 coroutine 裡面的程式碼呢?

需要兩件事情，第一進入 async 模式，也就是進入 Event Loop 開始控制整個程式的狀態，第二就是將 coroutine 變成 task

首先要如何進入 async 模式

在一般的 Python 執行下，屬於同步模式，要切換到讓 Event Loop 控制，我們要使用 asyncio.run 方法，此方法接受一個 coroutine object，這個 asyncio.run 他會做兩件事情，第一，建立 Event Loop，第二，將這個 coroutine 變成 Event Loop 裡面的第一個 Task。

Event Loop 建立之後，他會去尋找哪一個 Task 可以執行，此時只會有一個 Task，所以他就會開始執行這個 coroutine object。

## 增加多個 Task
Event Loop 核心是他可以有很多個 Task，然後他來決定要執行哪一個 Task，因此，當我們在 async 模式下的時候，我們要如何增加 Task

```py
import asyncio
import time

async def say_afeter(delay, what):
  await asyncio.sleep(delay)
  print(what)

async def main():
  print(f'started at {time.strftime('%X')}')
  
  await say_afeter(1, 'hello')
  await say_afeter(2, 'world')
  
  print(f'fniished at {time.strftime('%X')}')

asyncio.run(main())
```

上面的程式碼可以看到 await 關鍵字，這個關鍵字也就是將 coroutine 轉成 Task 的方法

使用 await 關鍵字的時，會發生以下幾件事情，coroutine 被包裝成一個 Task，並且告訴 Event Loop 多了一個 Task; 第二他會告訴 Event Loop 現在這個 Task (async def main) 需要等到 say_after Task 完成之後才能繼續往後執行。第三他會 yield 出去

上面程式碼整個執行過程

在 asyncio.run 裡面有 main 這個 coroutine object，他會把這個 main 作為一個 Task 放到 Event Loop，之後 Event Loop 會尋找會尋找 Task，會發現只有一個 main 的 Task，然後他就開始執行這個 Task ，

按照 main 函式裡面的執行順序執行，碰到 say_after 這個 coroutine function 時，此方法會回傳 coroutine object，await 關鍵字會將這個 coroutine object 變成一個 Task，放到 Event Loop 裡面，同時告訴 Event Loop 我需要等待他，然後將控制權交給 Event Loop。

現在 Event Loop 裡面有兩個 Task，一個是 main 一個是 say_after，但是 main 現在執行不了，因為他要等待這個 say_after，所以 Event Loop 就會先讓 say_after 先執行，say_after 裡面有一個 await asyncio.sleep(delay) 的非同步方法，同樣也是告訴 Event Loop 再多這個 Task，並且必須等到這個 Task 完成才行繼續往後執行，所以又將控制權給了 Event Loop。

現在 Event Loop 有三個 Task，main、say_after 與 sleep，現在 sleep 會告訴 Event Loop 等待一秒，一秒後此 Task 完成了，現在 Event Loop 一看剩一個 main 一個 say_after，因此繼續執行 say_after 後面的程式碼，之後 say_after 也完成了，又把控制權還給了 Event Loop，現在只剩下一個 Task 了，因此又把控制權交給 main，後續的程式碼也是依此規則完成。

這個過程要注意，所有控制權的返回都是明確的，也就是 Event Loop 並沒有辦法強行從 Task 中拿回控制權，必須要等 Task 主動將控制權交回去；交回去的方式有兩種，第一個是 await 會交回，第二個當函式執行完畢之後會交回，因此要注意當 Task 裡面有無窮迴圈，那這個 Event Loop 就卡死了。

## coroutine
coroutine 是一種在單執行緒中管理非同步操作的方法。與傳統函式不同，coroutine 可以在執行過程中暫停和恢復他們的執行。

coroutine 有一些特點:
1. 多重進入點與退出點：與 subroutine 相比，coroutine 一大特色就是擁有多重進入點和退出點。代表在執行過程中，可以在多個點暫停或恢復。
2. 狀態保留：當 coroutine 暫停時，他會保留與其當前的執行狀態，包含局部變數和執行堆疊。使得在恢復時，能夠繼續從上次暫停的地方繼續執行。
3. async def 語法：這句語法表明一個函數是 coroutine，且可以使用 await 關鍵字來暫停與恢復它的執行。
4. 非同步操作簡化：由於 coroutine 的特性，在處理 I/O 密集型任務和需要高校併發處裡的場景中很有幫助。


## subroutine
Routine 中文意思為例行公事，在程式中例行公事指的就是主程式，在例行公事之下的也就是副程式 subroutine，副程式不會自動執行，他需要被呼叫才會執行。

以 Python 來說每一個函式就是一個 subroutine

以下是一些 Subroutine 的重要特性
1. 單一進入點: 當 subroutine 被呼叫時，他從函式的開始處執行。
2. 參數傳遞：subroutine 可以接收參數，這些參數用來傳遞執行 subroutine 所需的資料。
3. 返回值：subroutine 可以返回一個值，當執行完畢後，控制權和任何返回值都會被傳回呼叫端。
4. 獨立的執行堆疊：每個 subroutine 被呼叫時，系統會在執行堆疊中為其建立一個新的框架，用於儲存局部變數和其他執行訊息。