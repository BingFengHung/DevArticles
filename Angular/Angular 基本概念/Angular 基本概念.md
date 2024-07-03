# Angular 基本概念

## 前言

紀錄由 Angular 18 官方文件裡面提到的一些基本概念。

[Angular 18](https://angular.dev)

## Angular 組件基本架構

```ts
// ap/app.component.ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  template: `Hello World`,
  styles: `
    :host {
      color: blue;
      font-size: 25px;
    }
  `
  standalone: true,
})
export class AppComponent {}
```

上面就是一個 Angular 的組件，每個組件檔案包含三個部分：
- TypeScript 類別
- HTML 模板
- CSS 樣式

你也可以將 HTML 模板與 CSS 樣式分別拆成不同的檔案引入使用。

@Component 是一個裝飾器

## Component Class 
Component Class 裡面是用來定義組件的邏輯與行為的。

像是在 AppComponent 裡面定義屬性

```ts
export class AppComponent {
  name = "Joe";
}
```

然後如果要在網頁畫面上的組件呈現該屬性，需要使用到 **{{ }}**

```ts
@Component({
  selector: 'app-root',
  template: `Hello {{ name }}`,
  styles: `
    :host {
      color: blue;
      font-size: 25px;
    }
  `
  standalone: true,
})
```

🎆 **{{}}** 是一個插值表達式，裡面可以放表達式

## 如何在不同的組件中使用其他組件
在上面範例中的 html 模板中，有一個 selector 屬性，如 selector: 'app-root'，裡面的 app-root 就是組件的名稱，所以可以用 **<app-root />** 這樣的寫法引入使用。

假設現在有兩個組件定義，其中一個要引入另外一個組件，此時需要再要引用組件的該組件定義一個 imports 的陣列屬性，然後陣列中放置要引入的組件。

```ts
import { Component } from '@angular/core'

// cat component
@Component({
  selector: 'app-cat',
  template: `Meow, I am {{ name }}`,
  standalone: true
})
export class CatComponent {
  name = 'Kitty'
}

// garden component
@Component({
  selector: 'app-root',
  template: `
    <p>There has a cat</p>
    <app-cat />
  `,
  standalone: true,
  imports: [CatComponent]
})
export class AppComponent {}
```

## 組件控制流程 - @if
@if 可以用來決定組件是否要顯示在畫面上

```ts
import {Component} from '@angular/core';

@Component({
  selector: 'app-root',
  template: `
    @if (isServerRunning) {
      <span>Yes, the server is running</span>
    } @else { 
      <span>No, the server is borken</span>
    }
  `,
  standalone: true,
})
export class AppComponent {
  isServerRunning = false;
}
```

## 組件控制流程 - @for
@for 重複顯示某個陣列內的元素

假設有一組陣列元素如下

```ts
[{ id: 1, name: 'Joe' }, { id: 2, name: "Jack" }, { id: 3, name: "Jim"}]
```

@for 模板法法如下
```ts
@for (user of users; track user.id) {
  <p>{{ user.name }}</p>
}
```

這邊跟 Vue 一樣需要綁定一個唯一值給他，在 Vue 中通常是用 :key 的方式指定唯一值，在 Angular 裡面是使用 track 後面跟著指定的唯一值。

完整程式碼如下：

```ts
import {Component} from '@angular/core';

@Component({
  selector: 'app-root',
  template: `
    @for(user of users; track user.id) {
      <p>{{ user.name }}</p>
    }
  `,
  standalone: true,
})
export class AppComponent {
  users = [
    { id: 0, name: 'Joe' },
    { id: 1, name: 'Jack' },
    { id: 2, name: 'Jim' },
  ];
}
```

## 事件處理
透過事件的處理，讓我們可以與網頁進行互動

在 Angular 使用 **()** 進行事件的綁定，程式碼如下所示：

```ts
@Component({
  template: `<button (click)="greet()">`
})
class AppComponent {
  greet() {
    console.log('Hi, there')
  }
}
```

在 html 元素上面加上事件，都要前綴 on，在 Angualr 中並不需要

完整程式碼如下：

```ts
import {Component} from '@angular/core';

@Component({
  selector: 'app-root',
  template: `
    <section (mouseover)="onMouseOver()"
             (mouseleave)="onMouseLeave()">
      Hover to 👀
      {{ message }}
    </section>
  `,
  standalone: true,
})
export class AppComponent {
  message = '';

  onMouseOver() {
    this.message = 'Hi';
  }

  onMouseLeave() {
    this.message = '';
  }
}
```

## 屬性綁定
透過屬性綁定可以動態的改變 HTML 屬性的值

這邊要將要綁定的屬性用 **[]** 刮起來，後面跟著組件裡面指定的屬性

程式碼如下所示：

```ts
import {Component} from '@angular/core';

@Component({
  selector: 'app-root',
  styleUrls: ['app.component.css'],
  template: `
    <div [contentEditable]="isEditable"></div>
    <button (click)="onEditClick()">Edit</button>
  `,
  standalone: true,
})
export class AppComponent { 
  isEditable = false;
  
  onEditClick() {
    this.isEditable = !this.isEditable
  }
}
```

## 組件之間的溝通 - @Input
使用 **@Input** 裝飾器，指定哪些屬性是可以透過外部傳遞進來的。

並且透過傳遞進來的資料再決定組件內部要做怎麼樣的邏輯處理。

語法結構如下：

```ts
class CatComponent {
  @Input() species = '';
}
```

父組件要傳遞屬性值的話，撰寫方式如下：

```ts
@Component({
  template: `<app-cat species="Felis catus"/>`
})
class GardenComponent {}
```

完整程式碼如下：

子組件定義
```ts
import { Component, Input } from '@angular/core';

@Component({
  selector: 'app-cat',
  template: `
    <p>The cat's species is {{ species }}</p>
  `,
  standalone: true,
})
export class UserComponent {
  @Input() species = '';
}

```

父組件定義
```ts
import { Component } from '@angular/core';
import { CatComponent } from './cat.component';

@Component({
  selector: 'app-root',
  template: `
    <app-cat species="Felis catus" />
  `,
  standalone: true,
  imports: [CatComponent],
})
export class AppComponent {}
```

## 組件之間的溝通 - @Output
有時候子組件需要發出通知給父組件知道，此時就需要定義 **@Output** 裝飾器

其實這就是事件的觸發，當子組件達到某個條件時，要發出某個事件出來，讓外面有註冊該事件的父組件可以接收到通知，以此來作後需的邏輯處理。

子組件定義事件，其語法定義如下：

```ts
class CatComponent {
  count = 0;

  @Output() incrementCountEvent= new EventEmitter<number>();
  
  onClick() {
    this.count++;
    if (this.count === 5) {
      this.incrementCountEvent.emit(this.count);
    }
  }
}
```

父組件註冊子組件的事件，程式碼如下所示：

```ts
@Component({
  template: `
    <p>Cat total count: {{ count }}</p>
    <app-cat (incrementCountEvent)="addCatCount($event)" />
  `
})
class GardenComponent {
  count = 0;

  addCatCount(count: number) {
    this.count = count;    
  }
}
```