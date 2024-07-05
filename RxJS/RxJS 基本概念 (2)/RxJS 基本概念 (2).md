# RxJS 基本概念 (2)

## 前言
前一篇對 RxJS 有了一個基本的認知，後面將針對 RxJS 的幾個核心概念進行介紹。

## Observable
Observable 是多值的，也就是 Observable 可以發送多個值。

Observable 是惰性的，意思就是說只有在訂閱者訂閱的時候，才會發送值。這與陣列不一樣，陣列會在建立的時候就包含所有的值了。

Observable 是 push 集合，也就是資料是由 Observable 主動推送給訂閱者。相對於 push 集合，pull 集合是使用者主動拉取資料，而 push 集合是由資料源主動推送資料給使用者。

### 什麼是拉取
在拉取的系統中，資料的消費者會主動請求資料，資料的生產者會被動的回應這些請求。

- 消費者：主動
- 生產者：被動

消費者主動拉取範例：

```js
function getValue() {
  return 50;
}

console.log(getValue()); // 主動拉取資料
```

```js
function* generator() {
  yield 1;
  yield 2;
  yield 3;
}

const iter = generator();
console.log(iter.next().value);  // 主動拉取值 1
console.log(iter.next().value);  // 主動拉取值 2
console.log(iter.next().value);  // 主動拉取值 3
```

上面兩個範例都是需要消費者自行決定什麼時候要從資料生產者那邊取得資料。

### 什麼是推送
在推送的系統中，資料的生產者主動發送資料給消費者，消費者被動接收資料。

- 消費者：被動
- 生產者：主動

範例：Observable 是 RxJS 中的推送系統，可以連續推送多個值。

```js
import { interval } from 'rxjs';

const observable = interval(1000); // 每秒發送一個值
observable.subscribe(value => console.log(value)); // 每秒接收並輸出一個值
```

💡 在 RxJS 中，Observable 是一個推送系統。

## Observable 剖析
Observable 可以用 new Observable 或建立的操作符如 of、fromEvent 等來建立。

如果有 Observer 訂閱後，執行時就會像 Observer 傳送 next/error/complete 通知。

最後在不需要的時候，在進行取消訂閱的動作。

- 建立 Observables
- 訂閱 Observables
- 執行 Observables
- 釋放 Observables

### 建立 Observables
可以用 new Observable 或用創建操作符，of、fromEvent

```js
import { Observable, of } from 'rxjs';

// 用 new Observable 建立
const observable = new Observable((subscriber) => {
  subscriber.next('Hello');
  subscriber.next('World');
  subscriber.complete();
});

// 用 of 操作符建立
const observable2 = of(1, 2, 3);
```

### 訂閱 Observables
訂閱時，需要提供一個觀察者 (Observer) 來接收通知。Observer 可以是物件，包含 next、error 和 complete 方法。

```js
const observer = {
  next: value => console.log(`Next: ${value}`),
  error: err => console.error(`Error: ${err}`),
  complete: () => console.log('Completed')
};

// 訂閱 observable1
const subscription1 = observable1.subscribe(observer);

// 訂閱 observable2
const subscription2 = observable2.subscribe(observer);
```

### 執行 Observables
Observables 執行可以傳遞三種類型的值：
- next（下一個） 通知：傳送數值、字串、物件等。
- error（出錯） 通知：傳送 JavaScript 錯誤或例外。
- complete（完成） 通知：不傳送值。

next 通知是最重要和最常見的型別，它們代表要傳遞給訂閱者的實際資料。**在 Observable 執行期間，error 和 complete 通知可能只發生一次，並且只能有其中之一。**

```js
import { Observable } from 'rxjs';

const observable = new Observable(function subscribe(subscriber) {
  subscriber.next(1);
  subscriber.next(2);
  subscriber.next(3);
  subscriber.complete();
  subscriber.next(4); // Observer 不會再收到資料，因為已經通知已經設定為完成
});
```

### 釋放 Observables
正確釋放 Observables 很重要，避免浪費計算能力以及記憶體洩漏問題。

取消註冊的程式碼如下所示：

```js
import { from } from 'rxjs';

const observable = from([10, 20, 30]);
const subscription = observable.subscribe((x) => console.log(x));

subscription.unsubscribe();
```

## 結論
- Observable 是一個推送系統
- 正確的釋放沒有在使用的 Observables 很重要

## 參考資料
[RxJS 官方文件](https://rxjs.dev/guide/overview)

# 🚀 RxJS 系列文章
[RxJS 基本概念 (1) - 概觀](https://bingfenghung.github.io/blog/articles/RxJS%3C_%3E%3ERxJS%20%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5%20(1))

[RxJS 基本概念 (2) - Observables](https://bingfenghung.github.io/blog/articles/RxJS%3C_%3E%3ERxJS%20%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5%20(2))

[RxJS 基本概念 (3) - Observer](https://bingfenghung.github.io/blog/articles/RxJS%3C_%3E%3ERxJS%20%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5%20(3))

[RxJS 基本概念 (4) - Subscription](https://bingfenghung.github.io/blog/articles/RxJS%3C_%3E%3ERxJS%20%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5%20(4))

[RxJS 基本概念 (5) - Subjects](https://bingfenghung.github.io/blog/articles/RxJS%3C_%3E%3ERxJS%20%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5%20(5))

[RxJS 基本概念 (6) - Scheduler](https://bingfenghung.github.io/blog/articles/RxJS%3C_%3E%3ERxJS%20%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5%20(6))

