# Angular 基本概念 (5)

## 前言
紀錄由 Angular 18 官方文件裡面提到的一些基本概念。

[Angular 18](https://angular.dev)

## Pipe
pipe 就像是在下終端機指令一樣，可以把前一個結果丟給後面的處理。

在 Angular 裡面 pipe 被用來在模板中傳遞資料用的，並且 pipe 要是純函式，因為這樣才不會有副作用的問題。

Angular 有內建幾個有用的 pipe 讓開發人員可以在組件中引入使用。也可以自行建立客製化 pipe。

這邊使用內建的 LowerCasePipe 方法在組件中使用，程式碼如下所示：

```ts
import {Component} from '@angular/core';
import {LowerCasePipe} from '@angular/common';

@Component({
  selector: 'app-root',
  template: `
    {{ username | lowercase }}
  `,
  standalone: true,
  imports: [LowerCasePipe],
})
export class AppComponent {
  username = 'JOE';
}
```

可以看到在 template 裡面用一個 | 的符號，就可以串接 lowercase 的 pipe 了。

## 透過 pipe 進行資料格式化
pipe 可以接受參數傳遞，要使用 **:** 語句後面跟著參數值就可以了。

一個簡單的例子如下：
```ts
template: `{{ date | date:'medium' }}`;
```

輸出的格式會是： **Dec 31, 2099, 11:59:59 PM**

## 客製化 pipe - @Pipe
我們可以自行客製化我們需要的 pipe，一樣需要讓我們的類別裝飾 @Pipe 裝飾器

製作方式如下程式碼所示：

```ts
import {Pipe, PipeTransform} from '@angular/core';
@Pipe({
  standalone: true,
  name: 'star',
})
export class StarPipe implements PipeTransform {
  transform(value: string): string {
    return `⭐️ ${value} ⭐️`;
  }
}
```

之後就能在組件中引入這個 pipe 使用，程式碼如下所示：

```ts
import { Component } from '@angular/core';
import { StarPipe } from './star.pipe';

@Component({
  selector: 'app-root',
  template: `
    Greet: {{ word | star }}
  `,
  standalone: true,
  imports: [StarPipe],
})
export class AppComponent {
  word = 'Hello World';
}
```

以上就是所有 Angular 基本概念的所有內容了。

## 系列文章
[Angular 基本概念 (1)](/blog/articles/Angular%3C_%3E%3EAngular%20%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5%20(1))

[Angular 基本概念 (2)](/blog/articles/Angular%3C_%3E%3EAngular%20%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5%20(2))

[Angular 基本概念 (3)](/blog/articles/Angular%3C_%3E%3EAngular%20%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5%20(3))

[Angular 基本概念 (4)](/blog/articles/Angular%3C_%3E%3EAngular%20%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5%20(4))

[Angular 基本概念 (5)](/blog/articles/Angular%3C_%3E%3EAngular%20%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5%20(5))