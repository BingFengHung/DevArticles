# TypeScript 快速上手指南
## 前言
TypeScript 與 JavaScript 的差別在於多了靜態型別檢查，原先 JavaScript 在型別錯誤可能只會在執行時期才會被發現，但 TypeScript 好處在於這些型別錯誤可以在編譯時期的時候就被發現了。

## 基本程式語法
1. 變數宣告
```ts
let isOk: boolean = false;
let age: number = 18;
let name: string = 'Joe';
```

2. 陣列宣告
```ts
let digits: number[] = [1, 2, 3];
let list: Array<number> = [4, 5, 6];
```

3. 元組宣告
```ts
let tup: [string, number];
tup = ['Joe', 18];
```

4. 列舉宣告
```ts
enum Colors { Red, Green, Blue };

let color: Colors = Colors.Blue;
```

5. 函數
```ts
function multiply(a: number, b: number): number {
  return a * b
}
```

6. 介面
```ts
interface Person {
  name: string;
  age: number;
}

let joe: Person = { name: "Joe", age: 18 };
```

7. 類別
```ts
class Animal {
  name: string;

  constructor(name: string) {
    this.name = name
  }
  
  greet(text: string = 'Hi') {
    console.log(`${greet}! My name is ${this.name}`)
  }
}

class Cat extends Animal {
  constructor(name: string) {
    super(name)
  }

  greet(text = 'Hello') {
    console.log('Miew, Miew');
    console.log(super.greet(text));
  }
}
```

8. 類別實作介面
```ts
interface Car {
  brand: string;
  isElectric: boolean;
  describe(): void;
}

class NewCar implements Car {
  brand: string;
  isElectric: boolean;  
  
  constructor(brand: string, isElectric: boolean) {
    this.brand = brand;
    this.isElectric = isElectric;
  }

  describe() {
    console.log(`Brand: ${brand}/ isElectric: ${isElectric}`)    
  }
}

const car = new NewCar("Tesla Model Y", true);
car.describe();
```

8. 泛型
```ts
function swap<T>(items: T[], index1: number, index2: number) {
  let temp = items[index1];
  items[index1] = items[index2];
  items[index2] = temp;
  return items;
}

const numbers = [1, 3, 2, 4]
console.log(swap(numbers, 1, 2))  // [1, 2, 3, 4]
```
