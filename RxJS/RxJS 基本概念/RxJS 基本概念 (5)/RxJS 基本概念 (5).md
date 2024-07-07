# RxJS 基本概念 (5) - Subjects

## 前言
接下來要接紹 Subjects，他跟 Observable 一樣，可以傳送值給 Observer，只不過他的做法比較不一樣。 

## Subjects
Subjects 是一個特殊型別的 Observable，他可以將值傳送給多個 Observer。

與 Observer 差別在於，Observable 是獨立的資料流，代表每個 Observer 收到的值都是從頭開始的，並不會影響到其他 Observer 接收到的值；反之，Subjects 是共享資料流，代表相同的資料流會發送給所有的 Observer。

### Observable 獨立資料流
```js
import { Observable } from 'rxjs';

const observable = new Observable(subscriber => {
  console.log('Observable executed');
  subscriber.next(Math.random());
  subscriber.complete();
});

observable.subscribe(value => console.log(`Observer 1: ${value}`));
observable.subscribe(value => console.log(`Observer 2: ${value}`));
```

輸出結果

```
Observable executed
Observer 1: 0.123456 (example)
Observable executed
Observer 2: 0.789012 (example)
```

可以看到兩個 Observer 收到的值是不一樣的。

### Subject 共享資料流
```js
import { Subject } from 'rxjs';

const subject = new Subject();

subject.subscribe(value => console.log(`Observer 1: ${value}`));
subject.subscribe(value => console.log(`Observer 2: ${value}`));

subject.next(Math.random());
```
輸出結果

```
Observer 1: 0.345678 
Observer 2: 0.345678 
```

## 每個 Subject 也都是 Observer
它是一個具有方法 next(v)、error(e) 和 complete() 的物件。要為 Subject 提供一個新值，只需呼叫 next(theValue)，它將被多播到註冊進來監聽 Subject 的 Observer。

## 參考資料
[RxJS 官方文件](https://rxjs.dev/guide/overview)
