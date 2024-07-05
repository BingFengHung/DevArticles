# RxJS 基本概念 (3) - Observer

## 前言
Observable 是可觀察的物件，那要叫誰去觀察這個物件呢? 也就是 Observer 觀察者。

## Observer
Observer 是 Obervable 傳遞各項值的消費者。

Observer 物件包含了一組 Callback 用來對應 Observable 傳遞的每種型別的通知：next、error 與 complete。

Observer 定義方式如下程式碼所示：

```js
import { Observable } from 'rxjs';

const myObservable = new Observable(subscriber => {
  subscriber.next(1);
  subscriber.next(2);
  subscriber.next(3);
  subscriber.complete();
});

const observer = {
  next: (x) => console.log('Got a next value: ' + x),
  error: (err) => console.error('Got an error: ' + err),
  complete: () => console.log('Got a complete notification'),
};

observable.subscribe(observer);
```

訂閱 Observable 時，可以只提供 next 的回呼 (Callback) 作為引數，不用附屬於 Observer 物件，例如：

```js
observable.subscribe((x) => console.log('Observer got a next value: ' + x));
```

## 錯誤處理
當 Observable 發送錯誤通知，error 方法會被呼叫：

```js
const errorObservable = new Observable(subscriber => {
  subscriber.next('Hello');
  subscriber.error('Something went wrong!');
  subscriber.next('World');
});

const errorObserver = {
  next: value => console.log(`Next value: ${value}`),
  error: err => console.error(`Error: ${err}`),
  complete: () => console.log('Completed')
};

errorObservable.subscribe(errorObserver);
```

當錯誤被通知的時候，他的執行流程就會被中斷，後面的 next 在呼叫也是不會被執行的。

## 完成通知
如果 Observable 發送完成通知，complete 方法被呼叫了，但是，在發出完成通知之後，又在發出 next，是不會有反應的

```js
const completeObservable = new Observable(subscriber => {
  subscriber.next('First value');
  subscriber.complete();
  subscriber.next('Second value');
});

const completeObserver = {
  next: value => console.log(`Next value: ${value}`),
  error: err => console.error(`Error: ${err}`),
  complete: () => console.log('Completed')
};

completeObservable.subscribe(completeObserver);
```

輸出結果

```bash
Next value: First value
Completed
```

## 參考資料
[RxJS 官方文件](https://rxjs.dev/guide/overview)

[Rxjs 學習筆記-Medium](https://medium.com/@chen090/rxjs-%E5%AD%B8%E7%BF%92%E7%AD%86%E8%A8%98-1acbc04e25d0)

# 🚀 RxJS 系列文章
[RxJS 基本概念 (1) - 概觀](https://bingfenghung.github.io/blog/articles/RxJS%3C_%3E%3ERxJS%20%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5%20(1))

[RxJS 基本概念 (2) - Observables](https://bingfenghung.github.io/blog/articles/RxJS%3C_%3E%3ERxJS%20%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5%20(2))

[RxJS 基本概念 (3) - Observer](https://bingfenghung.github.io/blog/articles/RxJS%3C_%3E%3ERxJS%20%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5%20(3))

[RxJS 基本概念 (4) - Subscription](https://bingfenghung.github.io/blog/articles/RxJS%3C_%3E%3ERxJS%20%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5%20(4))

[RxJS 基本概念 (5) - Subjects](https://bingfenghung.github.io/blog/articles/RxJS%3C_%3E%3ERxJS%20%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5%20(5))

[RxJS 基本概念 (6) - Scheduler](https://bingfenghung.github.io/blog/articles/RxJS%3C_%3E%3ERxJS%20%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5%20(6))

