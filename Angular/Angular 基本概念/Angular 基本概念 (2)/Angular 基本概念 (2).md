# Angular 基本概念 (2)

## 前言
延續上一篇 [Angular 基本概念](https://bingfenghung.github.io/blog/articles/Angular%3C_%3E%3EAngular%20%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5)

紀錄由 Angular 18 官方文件裡面提到的一些基本概念。

本篇主要介紹，針對資料載入需要比較久的組件，可以用組件延遲的功能，延遲組件載入、圖像優化以及 SPA 路由的設定與控制。

[Angular 18](https://angular.dev)

## 組件延遲載入 - @defer
有些組件並不需要立即的載入至畫面中，可能因為是後續才會使用到，透過 @defer，讓我們可以延遲載入組件。

要延遲載入很簡單，只需要在 **@defer { }** 區塊中，放入要延遲載入的組件即可。

```ts
@defer {
  <large-component />
}
```

🎆 @defer 會在瀏覽器閒置的時候，載入指定的組件。

### @plcaeholder 
在 @placeholder { } 區塊內可以寫一些 html，他可以在延遲載入開始前顯示指定的 html。 

```ts
@defer {
  <large-component />
} @placeholder {
  <p>Wait sometime</p>
}
```


### @loading
跟上面的 @placeholder 很像，但是，他會在要延遲的組件，正在取得資料但是還沒有取得完成時顯示，

```ts
@defer {
  <large-component />
} @placeholder {
  <p>Wait sometime</p>
} @loading {
  <p>Loading...</p>
}
```
### minimum
@placeholder 與 @loading 透過此參數可以避免畫面在快速載入時發生閃爍的問題。

@placeholder : minimum
@loading : minimum、after

```ts
@defer {
  <comments />
} @placeholder {
  <p>Future comments</p>
} @loading (minimum 2s) {
  <p>Loading comments...</p>
}
```

加上 minimum 2s 在 @loading 上面，可以讓裡面區塊的 html 至少顯示兩秒以上。

### on viewport
@defer 有許多觸發的選項可用，加上一個 viewport，這樣內容延遲載入直到進入 viewport

```ts
@defer (on viewport) {
  <large-component />
}
```

## 圖像優化 - NgOptimizedImage
在網頁中圖像是很重要的，但是，卻是導致應用程式效能低下的主要原因。

Angular 有針對圖像進行優化處裡，透過使用 NgOptimizedImage。

1. 引入 NgOptimizedImage 指令
NgOptimizedImage 指令放在 @angular/common 函式庫中，並且組件的 imports 陣列中

```ts
import { NgOptimizedImage } from '@angular/common';

@Component({
  ...
  imports: [NgOptimizedImage],
  ...
})
```

2. 將 img 的 src 屬性換成 ngSrc
使用 ngSrc 就可以支援圖像優化的功能

```ts
import { NgOptimizedImage } from '@angular/common';
@Component({
  standalone: true,
  template: `
    ...
    <li>
      Static Image:
      <img ngSrc="/assets/logo.svg" alt="Angular logo" width="32" height="32" />
    </li>
    <li>
      Dynamic Image:
      <img [ngSrc]="logoUrl" [alt]="logoAlt" width="32" height="32" />
    </li>
    ...
  `,
  imports: [NgOptimizedImage],
})
```

3. 優先載入重要的圖像
如果有某張圖像需要優先載入的話，可以在 img 元素上面，加上 `priority` 屬性

```ts
<img ngSrc="www.example.com/image.png" height="600" width="800" priority />
```

4. 圖像載入器
如果說圖像的路徑比較常，可以使用圖像載入器，指定圖像的基本 url，然後圖像就能使用相對路徑取得圖像。

```ts
import { NgOptimizedImage } from '@angular/common';
@Component({
  standalone: true,
  providers: [
    provideImgixLoader('https://my.base.url/'),
  ],
  template: `
    ...
    <li>
      <img ngSrc="image.png" alt="Angular logo" width="32" height="32" />
    </li>
    ...
  `,
  imports: [NgOptimizedImage],
})
```

## Angular Router 設定
路由允許我們在 SPA 開發中，可以進行頁面之間的切換，而不需要重新載入整個頁面，有效提升應用程式性能。

1. 建立一個 app.routes.ts 的檔案
```ts
// app.route.ts
import { Routes } from '@angular/router';
export const routes: Routes = [];
```

2. 提供路由給 provider
再來配置路由，這個設定需要再 app.config.ts 檔案中進行設定

```ts
import { ApplicationConfig } from '@angular/core';
import { provideRouter } from '@angular/router';
import { routes } from './app.routes';

export const appConfig: ApplicationConfig = {
  providers: [provideRouter(routes)],
};
```

3. 在組件中引入 RouterOutlet 
再來就是要決定要在哪邊顯示這些路由的畫面，這邊需要使用 RouterOutlet 組件來顯示指定的畫面。

```ts
import {RouterOutlet} from '@angular/router';
@Component({
  ...
  template: `
    <nav>
      <a href="/">Home</a>
      |
      <a href="/user">User</a>
    </nav>
    <router-outlet />
  `,
  standalone: true,
  imports: [RouterOutlet],
})
export class AppComponent {}
```

## 定義路由
經過上一段的專案配置之後，接下來就可以定義路由要顯示的內容了

1. 在 app.routes.ts 檔案中引入組件並指定路由路徑
```ts
import {Routes} from '@angular/router';
import {HomeComponent} from './home/home.component';
export const routes: Routes = [
  {
    path: '',
    component: HomeComponent,
  },
];
```

2. 新增 title 屬性
Angular 允許你為每個路由中的組件，定義 title 屬性，這樣在切換路由時，就能顯示指定的頁面標題。

```ts
import {Routes} from '@angular/router';
import {HomeComponent} from './home/home.component';
export const routes: Routes = [ 
  { 
    path: '', 
    title: 'App Home Page', 
    component: HomeComponent,
  }, 
];
```

## 使用 RouterLink
在前一段使用 `<a href='/'>` 這種方式畫面會有一個明顯的閃爍，這是因為每次在點選路由的時候，組件都要在重新下載比較沒有效率，使用 RouterLink 可以有效解決此問題。

1. 組件中引入 RouterLink
在 app.component.ts 組件中引入 RouterLink，並把 a 標籤中的 href 替換成 routerLink

```ts
import { RouterOutlet, RouterLink } from '@angular/router';
@Component({
  ...
  template: `
    <nav>
      <a routerLink="/">Home</a>
      |
      <a routerLink="/user">User</a>
    </nav>
    <router-outlet />
  `,
  standalone: true,
  imports: [RouterOutlet, RouterLink],
})
export class AppComponent {}
```

可以發現在做頁面切換時，不會有閃爍的問題。
