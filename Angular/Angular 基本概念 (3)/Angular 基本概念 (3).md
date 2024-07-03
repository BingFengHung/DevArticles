# Angular 基本概念 (3)

## 前言
延續上一篇 [Angular 基本概念 (2)](https://bingfenghung.github.io/blog/articles/Angular%3C_%3E%3EAngular%20%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5%20(2))

紀錄由 Angular 18 官方文件裡面提到的一些基本概念。

[Angular 18](https://angular.dev)

## Forms
很常需要進行表單的開發，因為網頁需要從客戶端取得使用者的輸入資訊。

在 Angular 中，有兩種類型的表單：
- Template-driven forms
- Reactive forms

下面先說明 Template-driven 的方式

### Template-driven forms
1. 引入 FormsModule
為了要可以與 form 上面的欄位進行資料綁定，需要先引入 **FormsModule**。

```ts
import { Component } from '@angular/core';
import { FormsModule } from '@angular/forms';

@Component({
  standalone: true,
  imports: [FormsModule],
})
export class AppComponent {}
```

2. 與 input 元素進行資料綁定
假設我們有一個表單長得像下面程式碼一樣

```html
<label for="framework">
  Favorite Framework:
  <input id="framework" type="text" />
</label>
```

我現在想要將組件上面的屬性與 input 的 value 做綁定

此時，要用到 FormsModule 裡面的一個 ngModel 的指令，他可以將 input 的 value 綁定到類別中的屬性

因此，最終的寫法如下所示：

```html
<label for="framework">
  Favorite Framework:
  <input id="framework" type="text" [(ngModel)]="favoriteFramework"/>
</label>
```
✨ **[()]** 指的是雙向綁定

3. 取得控制項上面的值
就跟上面說的一樣，由於現在是與類別中的某個屬性進行雙向綁定了，因此在 UI 畫面做了什麼修改，底層的屬性也會修改；反之，底層屬性修改，也能在畫面中看到，程式碼如下所示：
```ts
import { Component } from '@angular/core';
import {FormsModule} from '@angular/forms';

@Component({
  selector: 'app-root',
  template: `
    <label for="framework">
      Favorite Framework: {{ favoriteFramework }}<br>
      <input id="framework" type="text" [(ngModel)]="favoriteFramework"/>
    </label>
    <button (click)="onReset()">Reset</button>
  `,
  standalone: true,
  imports: [FormsModule]
})
export class AppComponent {
  favoriteFramework = '';

  onReset() {
    this.favoriteFramework = ''
  }
}
```

在 input 欄位中輸入數值，可以在 Faveorite Framework: 字串中看到輸入了什麼，並且按下 Reset Button 可以將雙向綁定的屬性字串設定為空字串，UI 畫面字串也會變為空的。

### Reactive forms
Teamplate-driven forms 適合小型表單的開發，開發速度快好上手；但比較複雜一點的表單，就需要透過 Reactive forms 來完成了。

1. 引入 ReactiveFormsModule
從 @angular/forms 模組中引入 **ReactiveFormsModule**，並且加入到組件的 imports 陣列中。

```ts
import { ReactiveFormsModule } from '@angular/forms';

@Component({
  selector: 'app-root',
  standalone: true,
  template: `
    <form>
      <label>Name
        <input type="text" />
      </label>
      <label>Email
        <input type="email" />
      </label>
      <button type="submit">Submit</button>
    </form>
  `,
  imports: [ReactiveFormsModule],
})
```

2. 建立 FormGroup 物件，並透過 FormControl 綁定元素上的屬性
一樣需要從 @angular/forms 引入 FormControl 與 FormGroup 

接下來就可以將表單上面的元素進行綁定

```ts
import { Component } from '@angular/core';
import {ReactiveFormsModule, FormControl, FormGroup } from '@angular/forms';

@Component({
  selector: 'root-app'
  template: `
    <form [formGroup]="profileForm">
      <label>
        Name
        <input type="text" formControlName="name" />
      </label>
      <label>
        Email
        <input type="email" formControlName="email" />
      </label>
      <button type="submit">Submit</button>
    </form>
  `,
  standalone: true
})
export class AppComponent {
  profileForm = new FormGroup({
    name: new FormControl(''),
    email: new FormControl(''),
  });
}
```
裡面幾個重點，form 要有 **[formGroup]** 指令，然後要指向類別的 profileForm 屬性，其他的 formControlName 要對應到 profileForm 裡面的屬性名稱。

> <form [formGroup]="profileForm">  
> <input type="text" formControlName="name" />  
> <input type="email" formControlName="email" />  

### 存取 formGroup 上面的值
這邊有一個重要的一點，要存取 FormGroup 裡面的屬性，要記得要多加上 .value 才能繼續讀存取屬性值，程式碼如下所示：

```ts
import { Component } from '@angular/core';
import {ReactiveFormsModule, FormControl, FormGroup } from '@angular/forms';

@Component({
  selector: 'root-app'
  template: `
    <form [formGroup]="profileForm">
      <label>
        Name
        <input type="text" formControlName="name" />
      </label>
      <label>
        Email
        <input type="email" formControlName="email" />
      </label>
      <button type="submit">Submit</button>
    </form>
    <br>
    <h2>Profile Form</h2>
    <p>Name: {{ profileForm.value.name }}</p>
    <p>Email: {{ profileForm.value.email }}</p>
  `,
  standalone: true
})
export class AppComponent {
  profileForm = new FormGroup({
    name: new FormControl(''),
    email: new FormControl(''),
  });
  
  
}
```

如果我們要在按下 Submit 按鈕的時候進行處裡函式，需要用 ngSubmit 的事件，來綁定事件處理

```ts
<form
  [formGroup]="profileForm"
  (ngSubmit)="handleSubmit()">
```

## forms 輸入驗證
表單最重要的就是輸入正確，因此，知道怎麼驗證表單欄位很重要。

Angular 有提供一些驗證工具，這些驗證工具也是放在 @angular/forms 模組中，裡面的 Validators。

```ts
import {ReactiveFormsModule, Validators} from '@angular/forms';

@Component({...})
export class AppComponent {}
```

接下來就可以根據欄位的預設值，設定驗證工具上去

```ts
profileForm = new FormGroup({
  name: new FormControl('', Validators.required),
  email: new FormControl('', [Validators.required, Validators.email]),
});
```

另外，如果要限制只有在欄位都符合規定，才能點選按鈕，FormGroup 有一個 valid 的屬性，可以用來限制 submit 按鈕只有在輸入的欄位都通過驗證的時候，才可以點選。

```ts
<button type="submit" [disabled]="!profileForm.valid">Submit</button>
```

## 系列文章
[Angular 基本概念](https://bingfenghung.github.io/blog/articles/Angular%3C_%3E%3EAngular%20%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5)

[Angular 基本概念 (2)](https://bingfenghung.github.io/blog/articles/Angular%3C_%3E%3EAngular%20%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5%20(2))