# Python 同步 vs 非同步程式設計
## 前言
同步程式設計也就是程式碼一步一步按順序的執行下去，當中間有比較耗時的工作時 (I/O 密集型任務)，該程式會卡住；非同步程式設計的話，它裡面有一個 Event Loop 的機制，主要用來等待與處裡非同步任務，在 Event Loop 中，當有非同步任務完成時，會觸發一個事件。Event Loop 會從 Event Queue 中得到這個事件，並呼叫相應的 Callback 函式來處理。


## 非同步程式設計說明
先來看一個 Python 官方的程式碼

```py
import asyncio

async def main():
  print('hello')
  await asyncio.sleep(1) # 等待 1 秒鐘
  print('world')
  
asyncio.run(main())
```

上面的程式碼，會先印出 hello，然後等待一秒鐘之後，印出 world

> 此時，我就有一個疑問了，如果使用 await asyncio.sleep(1) 他會暫停一秒，那我為什麼還需要使用 async/await 的方式，我直接讓 main 執行緒暫停一秒就好了啊!

主要的差異在於，傳統的同步程式中，當使用 time.sleep(1) 等待一秒，整個執行緒會阻塞 (Block)，表示在這暫停的期間，此程式並不能做其他的工作，對於單一執行緒程式來說，整個程式就暫停了。

非同步程式設計的話，會允許執行緒在等待的同時，繼續執行其他的任務 (Task)。

也就是說上面的 `await asyncio.sleep(1)`，是告訴 Event Loop，我要暫停這個非同步函式 1 秒，但在這 1 秒內，你可以去執行其他的非同步任務。

下面的範例就是示範在非同步任務的等待期間，同時執行其他的非同步任務。

```py
import asyncio

async def do_task_1():
  print('Task 1 started')
  await asyncio.sleep(3)
  print('Task 1 finshed')

async def do_task_2():
  print('Task 2 started')
  awati asyncio.sleep(1)
  print('Task 2 finshed')

async def main():
  task1 = asyncio.create_task(do_task_1)
  task2 = asyncio.create_task(do_task_2)
  
  await task1
  await task2

asyncio.run(main())
```

在上面的程式碼中，
1. 在 main 函式開始執行時，使用 asyncio.create_task 的方法建立了兩個 task
2. 然後就是 await task1，當執行到 task1 也就是 do_task_1 中的 await asyncio.sleep(3)，任務會進入等待狀態
3. 在等待的期間，Event Loop 會轉而開始執行 task2
4. task2 會執行，然後他也會等待 1 秒鐘 (`await asyncio.sleep(1)`)，他的等待時間比較短，會比 task1 先執行完畢。
5. 兩個 task 完成後 main 函式就完成了。

## 結論:
非同步的 Event Loop 機制，會在非同步任務等待的期間，去檢查是否有其他的非同步任務執行，如果有他就會去執行該任務，當之前的任務完成時，他會從該任務 await 的地方繼續往下執行。

**同步程式設計**
1. 阻塞操作：任務按順序執行，當一個任務執行時，程式會在該任務完成前阻塞，不執行其他任務。
2. 簡單性：同步程式設計通常更簡單、更直觀，因為程式的執行順序與流程容易跟踪和理解。
3. 資源利用：在等待I/O操作（如讀取檔案、網路請求）時，同步程式可能無法有效利用系統資源，因為執行緒在等待期間是閒置的。

> 適用場景：適合快速的、不涉及大量等待或 I/O 操作的任務。

**非同步程式設計**
1. 非阻塞操作：允許程式在等待一個任務完成的同時，繼續執行其他任務。它不會阻塞程式的執行流程。
2. 複雜性：實現非同步程式設計通常比同步程式設計更複雜，因為需要管理非同步任務和事件循環，並且可能會遇到Race condition 和同步問題。
3. 效率和性能：更有效地利用資源，特別是在 I/O 操作時。這樣可以提高程式的整體效率和回應速度。

> 適用場景：適合 I/O 密集型應用，如網路服務、GUI、即時資料處理等。