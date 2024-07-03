# Angular 基本概念 (4)

## 前言
紀錄由 Angular 18 官方文件裡面提到的一些基本概念。

[Angular 18](https://angular.dev)

## DI in Angular
依賴注入 (Dependency Injection) 讓 Angular 可以在執行時期，提供應用程式所需要的資源。

依賴可以是一個服務也可能是其他的資源。

## 建立 injectable service - @Injectable
其中一種使用 service 的方式是做為資料與 API 交握的方式。

要讓 service 能夠重複的被使用的話，最好將邏輯寫在 service 中，這樣在應用程式需要的時候，就能使用到哦。

假設現在有一個類別，想要可以被注入到系統的其他地方，需要在該類別上面裝飾 @Injectable 裝飾器，程式碼如下所示：

```ts
import { Injectable } from '@angular/core';

@Injectable({
  providedIn: 'root',
})
export class UserService {}
```

在上面的程式碼中可以看到 Injectable 丟入的物件有一個 `provbidedIn` 的屬性，其值為 root，這代表這個 UserService 可以在整個應用程式中被使用。

以上就完成一個簡單的 service 建立了。

## 使用 inject() 方法在組件中注入 service
Angular 有一個方法可以用來注入 service，就是 **inject()** 方法。

接下來將會示範如何注入 service 到 app.component 中，程式碼如下：

首先先定義一個 animal 的 service。

```ts
// animal.service.ts
import { Injectable } from '@angular/core';

@Injectable({
  providedIn: 'root',
})
export class AnimalService {
  animals = [ 'Cat', 'Dog', 'Bird', 'Mouse' ]
  
  getAnimals(): string[] {
    return this.animals;
  }
  
  getAnimal(index: number) {
    return this.animal[index]; 
  }
}
```

接下來，在 app.component 裡面注入 animal service 的使用。

```ts
import { Component, inject } from '@angular/core';
import { AnimalService } from './animal.service';

@Component({
  selector: 'app-root',
  template: `
    <p>Animal Listing: {{ display }}</p>
  `,
  standalone: true,
})
export class AppComponent {
  display = '';
  animalService = inject(AnimalService);

  constructor() {
    this.display = this.animalService.getAnimals().join(' ⭐️ ');
  }
}
```

使用了 inject(AnimalService) 的方法，讓我們取得 AnimalService 的實例，就可以在組件中，使用該 service 的方法與屬性了。

## 以建構子的方式注入 service
除了用 inject() 方法注入 service 之外，還有另外一種透過建構子注入 service 的方式。

只需要在組件的建構子的地方，定義要使用的 service，那麼 Angular 就會主動注入該 service 給其組件使用。

不過要使用建構子注入的方式，程式碼比需要如下所示：
```ts
import { Component } from '@angular/core';
import { AnimalService } from './animal.service';

@Component({
  selector: 'app-root',
  template: `
    <p>Animal Listing: {{ display }}</p>
  `,
  standalone: true,
})
class AppComponet {
    constructor(private animalService: AnimalService) {
      this.display = this.animalService.getAnimals().join(' ⭐️ ');
    }
}
```

要有 private 關鍵字，然後參數名稱就會當作該組件的私有屬性了。

## 系列文章
[Angular 基本概念](https://bingfenghung.github.io/blog/articles/Angular%3C_%3E%3EAngular%20%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5)

[Angular 基本概念 (2)](https://bingfenghung.github.io/blog/articles/Angular%3C_%3E%3EAngular%20%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5%20(2))

[Angular 基本概念 (3)](https://bingfenghung.github.io/blog/articles/Angular%3C_%3E%3EAngular%20%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5%20(3))

[Angular 基本概念 (4)](https://bingfenghung.github.io/blog/articles/Angular%3C_%3E%3EAngular%20%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5%20(4))