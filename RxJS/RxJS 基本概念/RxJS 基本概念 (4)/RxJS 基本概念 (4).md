# RxJS 基本概念 (4) - Subscription

## 前言
前兩篇說明了 Observable 與 Observer 的建立與操作，接下來本篇將說明如何進行訂閱與取消訂閱。

## Subscription
要訂閱就是對一個 Observable 物件，後面加上 **.subscribe()** 方法，然後 subscribe 方法裡面可以放 Observer 物件或是一個 callback 函數，這兩種做法，這樣就完成訂閱的動作了。

有訂閱當然就會有需要取消的時候，如果說針對沒有在使用的訂閱沒有取消的話，可能會導致無用的計算或是記憶體洩漏的問題。

Subscription 有一個很重要的方法 **unsubscribe()**，用此方法就可以取消訂閱。

取消方法如下程式碼所示：

```js
import { interval } from 'rxjs';

const observable = interval(1000);
const subscription = observable.subscribe(x => console.log(x));

subscription.unsubscribe();
```

也可以一次取消多個訂閱，只需要將要取消的訂閱加入至另外一個訂閱即可。

```js
import { interval } from 'rxjs';

const observable1 = interval(400);
const observable2 = interval(300);

const subscription = observable1.subscribe(x => console.log('first: ' + x));
const childSubscription = observable2.subscribe(x => console.log('second: ' + x));

subscription.add(childSubscription);

setTimeout(() => {
  subscription.unsubscribe();
}, 1000);
```
訂閱也有一個 remove(otherSubscription) 方法，以撤消新增進來的子訂閱。

## 參考資料
[RxJS 官方文件](https://rxjs.dev/guide/overview)
