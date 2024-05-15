# JavaScript 函式的 property 屬性說明
在 JavaScript 中，每個函數都有 property 屬性，這個屬性指向一個物件。當使用 new 關鍵字建立物件的時候，會自動繼承這個 property 物件上面的屬性與方法。代表所有透過這個函式建立出來的物件，都是共享函式 property 物件上面的所有屬性與方法。

程式碼如下所示：
```js
function Car(brand, model) {
   this.brand = brand
   this.model = model
}

// 透過函式的 property 物件定義方法
Car.prototype.print() {
   console.log(`Brand: ${this.barnd}; Model: ${this.model}`);
}

// 建立物件
const toyota = new Car('Toyota', 'Yaris');
const tesla = new Car('Tesla', 'Model Y');

// 呼叫共用的 print 方法
toyota.print();  // Brand: Toyota; Model: Yaris
tesla.print();   // Brand: Tesla; Model: Model Y
```

這邊要注意使用 prototype 定義的屬性與方法在物件之間是共用的，如果是在建構函式裡面定義屬性與方法，會是該物件所有，也就是當你 new 了幾個物件出來，代表同樣類型的屬性與方法，也是會有多少個，並沒有共享的效果。