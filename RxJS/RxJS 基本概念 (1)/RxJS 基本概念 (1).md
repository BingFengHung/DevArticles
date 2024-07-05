# RxJS 基本概念 (1)

## 前言
RxJS 是 Reactive Extensions for JavaScript 的縮寫，用來處理非同步程式與事件驅動的程式問題。

在 RxJS 中，使用 Observable 來管理任何的事件，這些事件可以是 DOM 事件、HTTP 請求、計時器或是非同步的操作。

透過 RxJS 有以下優點：
1. 處理非同步資料流： RxJS 可以建立並組合多個非同步資料流，提供一種簡潔的方式來處理這些資料。
2. 事件驅動： RxJS 提供了許多的操作符，可用來轉換、過濾、合併與處理事件流，這讓事件驅動的邏輯變得更簡單。

[RxJS 官方文件](https://rxjs.dev/guide/overview)

## RxJS 用到的東西
- Observable 可觀察物件: 一個資料集合，代表值 (可以是數字、字串、物件等等) 或事件的資料集合。
- Observer 觀察者物件：接收可觀察者的資料。具有 next()、error()、complete() 三個方法，可以處理接收到的資料。
- Subscription 訂閱物件：代表`可觀察物件`與`觀察者`之間的訂閱關係，是一個物件，具有取消訂閱的方法。
- Operator 操作符： 是一些方法可以在 Obersver 收到 Observable 之前可以用來修飾這些資料，例如map, filter, concat, reduce 等等。
- Subject 主體物件： 主要的特性是可以傳送資料給多個 Observer，類似 EventEmitter。
- Scheduler 排程器：控制某個訂閱何時開始以及何時傳遞通知。

Reactive X 使用了 **Observer pattern**、**Iterator pattern**、**Functional programming** 這三大概念

- Observer Pattern：可針對特定的主題進行訂閱，當主題有任何的更新，就可以將訊息發送給所有的訂閱者
- Iterator Pattern：一種迭代的方式，可以根據使用的迭代演算法，決定指定的陣列或是集合，要如何進行迭代。
- Functional Programming：一種程式編程範式，主要的概念是純函式、無副作用、第一公民函式，這種範式強調不可變數據結構和聲明式編程。

## First Examples
傳統要註冊元素事件，註冊方式如下：

```js
document.addEventListener('click', () => console.log('Hi'));
```

<hr>
使用 RxJS 的話，可以把事件變成可觀察物件 (Observable)

```js
import { fromEvent } from 'rxjs';

fromEvent(document, 'click').subscribe(() => console.log('Hi'));
```
## Purity  
RxJS 使用純函式的方式產生值。

以往撰寫程式的時候，或多或少都有可能會建立出一個不純的函式，導致一些副作用的發生，如下程式碼所示：

```js
let count = 0;
document.addEventListener('click', () => console.log(`Count: ${++count}`))
```

在上面程式碼中，如果 count 變數有在其他地方修改，可能會導致程式碼的執行與你預期的不一樣，這是很嚴重的問題。

<hr>

使用 RxJS，可以確保不會有副作用發生

```js
import { fromEvent, scan } from 'rxjs';

fromEvent(document, 'click')
   .pipe(scan((count) => count + 1, 0))
   .subscribe((count) => console.log(`Count: ${count}`));
```

- pipe 方法裡面通常會放 operator
- scan 類似 reduce

## Flow
RxJS 有很多 Operator 可以幫助你控制事件如何在 observables 中流動。

```js
import { fromEvent, throttleTime, scan } from 'rxjs';

fromEvent(document, 'click')
  .pipe(
    throttleTime(1000),
    scan((count) => count + 1, 0)
  )
  .subscribe((count) => console.log(`Clicked ${count} times`));
```

## Values
透過你的 Observables 傳來的值進行轉換。

```js
import { fromEvent, throttleTime, map, scan } from 'rxjs';

fromEvent(document, 'click')
  .pipe(
    throttleTime(1000),
    map((event) => event.clientX),
    scan((count, clientX) => count + clientX, 0)
  )
  .subscribe((count) => console.log(count));
```

## 結論
目前體驗下來，覺得 RxJS 的好處是避免副作用以及裡面有預設一些常用的方法，讓自己不用再手動刻。

> [!TIP]
> 可觀察物件.訂閱物件(觀察者)

## 參考資料
[RxJS 官方文件](https://rxjs.dev/guide/overview)

[Rxjs 學習筆記-Medium](https://medium.com/@chen090/rxjs-%E5%AD%B8%E7%BF%92%E7%AD%86%E8%A8%98-1acbc04e25d0)