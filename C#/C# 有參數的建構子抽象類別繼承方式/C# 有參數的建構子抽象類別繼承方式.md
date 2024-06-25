# C# 有參數的建構子抽象類別繼承方式
如果抽象類別中，定義了句有參數的建構子，那麼再給別的類別繼承的時候，需要使用 base 關鍵字指派參數進去，程式碼如下所示：

```csharp
abstract class Animal 
{
   protected Animal(string name) 
   {
      Name = name;
   }

   public string Name { get; }
}

class Cat : Animal
{
   public Cat(string name) : base(name) 
   {
   }
   
   public override string ToString()
   {
      return $"My name is {Name}.";
   }
}

var cat = new Cat("Kitty");
cat.ToString();
```

如果說子類別有兩個參數並且想要讓 this 與 base 兩個關鍵字共用呢? 這個是不行的，這兩個只能擇一

```csharp
abstract class Animal 
{
   protected Animal(string name) 
   {
      Name = name;
   }

   public string Name { get; }
}

class Cat : Animal
{
   private int _age;
   public Cat(string name) : this(name, 12) 
   {
   }
   
   public Cat(string name, int age): base(name) 
   {
      this._age = age;
   }

   public override string ToString() 
   {
      return $"My name is {Name}, and I am {_age} years old.";
   }
}

var cat = new Cat("Kitty");
cat.ToString();
```