# RxJS 基本概念 (6) - Scheduler

## 前言
要精確的控制 Observable 的操作，可以透過排程器 (Scheduler) 進行處理。

## Scheduler
Scheduler 決定了某個 Observable 何時執行訂閱函數、何時發送通知，以及如何安排這些執行過程。

### Scheduler 的主要特性
1. 調度執行：控制何時執行 Observable 的訂閱邏輯
2. 調度通知：控制何時發送 next、error 與 complete 通知給 Observer
3. 管理並行：可以管理並控制非同步操作的併行性。

### 常見的 Scheduler 類型
以下是一些常見的 Scheduler：
1. `asapScheduler`：盡快執行的 Scheduler。在微任務佇列上排程，這與用於 Promise 的佇列相同。
2. `asyncScheduler`： 用 setTimeout 的機制，非同步的執行任務。
3. `queueScheduler`： 同步執行任務，任務會被排入一個先進先出的佇列中，按順序執行。
4. `animationFrameScheduler`：使用 requestAnimationFrame 在下一個瀏覽器重繪之前執行。

## Scheduler 使用範例
以下將針對上面提到的 Scheduler 類型進行說明。

### asapScheduler
```js
import { asapScheduler, of } from 'rxjs';

console.log('Before')

of(1, 2, 3, 4, 5, asapScheduler).subscribe(value => console.log(value));

console.log('After')

```

結果輸出

```
Before
After
1
2
3
4
5
```

asapScheduler 會讓 Observablue 在當前同步任務完成後盡快執行。


### asyncScheduler
```js
import { asyncScheduler, of } from 'rxjs';

console.log('Before');

of(1, 2, 3, 4, 5, asyncScheduler).subscribe(value => {
  console.log(value);
});

console.log('After');
```
輸出結果
```
Before
After
1
2
3
4
5
```
asyncScheduler 使得 Observable 在下個事件循環（使用 setTimeout）中執行。

### queueScheduler
```js
import { queueScheduler, of } from 'rxjs';

console.log('Before');

of(1, 2, 3, 4, 5, queueScheduler).subscribe(value => {
  console.log(value);
});

console.log('After');
```

輸出結果
```
Before
1
2
3
4
5
After
```

queueScheduler 使得 Observable 同步地執行，訂閱立即開始並按順序執行。

### animationFrameScheduler
```js
import { animationFrameScheduler, fromEvent } from 'rxjs';
import { map, takeWhile, observeOn } from 'rxjs/operators';

const clicks = fromEvent(document, 'click').pipe(
  map(event => ({ x: event.clientX, y: event.clientY })),
  takeWhile(({ x, y }) => x < window.innerWidth && y < window.innerHeight),
  observeOn(animationFrameScheduler)
);

clicks.subscribe(position => {
  console.log(`Clicked at position: ${position.x}, ${position.y}`);
});
```

animationFrameScheduler 使用 requestAnimationFrame，確保在每次瀏覽器重繪之前執行，適合用於動畫和高效能場景。

## 參考資料
[RxJS 官方文件](https://rxjs.dev/guide/overview)

## 🚀 RxJS 系列文章
[RxJS 基本概念 (1) - 概觀](https://bingfenghung.github.io/blog/articles/RxJS%3C_%3E%3ERxJS%20%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5%20(1))

[RxJS 基本概念 (2) - Observables](https://bingfenghung.github.io/blog/articles/RxJS%3C_%3E%3ERxJS%20%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5%20(2))

[RxJS 基本概念 (3) - Observer](https://bingfenghung.github.io/blog/articles/RxJS%3C_%3E%3ERxJS%20%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5%20(3))

[RxJS 基本概念 (4) - Subscription](https://bingfenghung.github.io/blog/articles/RxJS%3C_%3E%3ERxJS%20%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5%20(4))

[RxJS 基本概念 (5) - Subjects](https://bingfenghung.github.io/blog/articles/RxJS%3C_%3E%3ERxJS%20%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5%20(5))

[RxJS 基本概念 (6) - Scheduler](https://bingfenghung.github.io/blog/articles/RxJS%3C_%3E%3ERxJS%20%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5%20(6))

