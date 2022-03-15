# js继承

```javascript
    // 定义一个动物类
    function Animal(name){
        // 属性
        this.name = name || 'Animal'
        // 实例方法
        this.sleep = function(){
            console.log(this.name + '正在睡觉！')
        }
    }
    // 原型方法
    Animal.prototype.eat = function(food){
        console.log(this.name + '正在吃' + food)
    }
```

+ 原型继承：将父类的原型作为子类的原型
  + 缺点：无法继承父类实例的属性/方法，但是可以继承父类原型的属性/方法

```javascript
    function Cat(){}
    Cat.prototype = Animal.prototype
    Cat.prototype.constructor = Cat
    Cat.prototype.name = 'cat';

    // Test Code
    let cat = new Cat();
    console.log(cat.name); // cat
    console.log(cat.eat('fish')) // cat正在吃fish
    console.log(cat.sleep()) // 报错
    console.log(cat instanceof Animal) // true
    console.log(cat instanceof Cat) // true
```

+ 构造继承：使用父类的构造函数来增强子类实例，相当于复制父类的实例属性给子类
  + 特点：  
    + 解决了原型链继承中，子类实例共享父类引用属性的问题
    + 创建子类实例时，可以向父类传递参数
    + 可以实现多继承（call多个父类对象）
  + 缺点：  
    + 实例并不是父类的实例，而是子类的实例
    + 只能继承父类的实例属性和方法，不能继承原型属性/方法
    + 无法实现函数复用，每个子类都有父类实例函数的副本，影响性能

```javascript
    function Cat(name){
        Animal.call(this)
        this.name = name || 'Tom'
    }

    // Test Code
    let cat = new Cat();
    console.log(cat.name); // Tom
    console.log(cat.eat('fish')) // 报错
    console.log(cat.sleep()) // Tom正在睡觉！
    console.log(cat instanceof Animal) // false
    console.log(cat instanceof Cat) // true
```

+ 原型实例继承：将父类的实例作为子类的原型  
  + 特点：  
    + 非常纯粹的继承关系，实例是子类的实例，也是父类的实例
    + 父类新增原型方法原型属性，子类都能访问到
    + 简单，易于实现
  + 缺点：  
    + 可以在Cat构造函数中，为Cat实例增加实例属性。如果要新增原型属性和方法，则必须放在 new Animal() 这样的语句之后执行
    + 无法实现多继承
    + 来自原型对象的所有属性被所有实例共享
    + 创建子类实例时，无法向父类构造函数传参

```javascript
    function Cat(){}
    Cat.prototype = new Animal()
    Cat.prototype.constructor = Cat
    Cat.prototype.name = 'cat';

    // Test Code
    let cat = new Cat();
    console.log(cat.name); // cat
    console.log(cat.eat('fish')) // cat正在吃fish
    console.log(cat.sleep()) // cat正在睡觉！
    console.log(cat instanceof Animal) // true
    console.log(cat instanceof Cat) // true
```

+ 拷贝继承
  + 特点：  
    + 支持多继承
  + 缺点：  
    + 效率较低，内存占用高（因为要拷贝父类的属性）
    + 无法获取父类不可枚举的方法（不可枚举方法，不能使用 for in 访问到）

```javascript
    function Cat(name){
        const animal = new Animal()
        for(let p in animal){
            Cat.prototype[p] = animal[p]
        }
        this.name = name || 'Tom'
    }
    // Test Code
    let cat = new Cat();
    console.log(cat.name); // cat
    console.log(cat.eat('fish')) // cat正在吃fish
    console.log(cat.sleep()) // cat正在睡觉！
    console.log(cat instanceof Animal) // false
    console.log(cat instanceof Cat) // true
```

+ 组合继承：通过调用父类构造，继承父类属性并保留传参的优点，然后通过将父类实例作为子类原型，实现函数复用  
  + 特点：
    + 可以继承实例属性/方法，也可以继承原型属性/方法
    + 既是子类的实例，也是父类的实例
    + 不存在引用属性共享问题
    + 可传参
    + 函数可复用
  + 缺点：调用了两次父类构造函数，生成了两份实例（子类实例将子类原型上的那份屏蔽了）

```javascript
    function Cat(name){
        Animal.call(this)
        this.name = name || 'Tom'
    }
    Cat.prototype = new Animal()
    Cat.prototype.constructor = Cat
    // Test Code
    let cat = new Cat();
    console.log(cat.name); // Tom
    console.log(cat.eat('fish')) // Tom正在吃fish
    console.log(cat.sleep()) // Tom正在睡觉！
    console.log(cat instanceof Animal) // true
    console.log(cat instanceof Cat) // true
```

+ 寄生组合继承：通过寄生方式，砍掉父类的实例属性，在调用两次父类构造的时候，就不会初始化两次实例方法/属性，避免组合继承的缺点

```javascript
    function Cat(name){
        Animal.call(this)
        this.name = name || 'Tom'
    }
    (function(){
        var Super = function(){}
        Super.prototype = Animal.prototype
        Cat.prototype = new Super()
        Cat.prototype.constructor = Cat
    })()
    // Test Code
    let cat = new Cat();
    console.log(cat.name); // Tom
    console.log(cat.eat('fish')) // Tom正在吃fish
    console.log(cat.sleep()) // Tom正在睡觉！
    console.log(cat instanceof Animal) // true
    console.log(cat instanceof Cat) // true
```
