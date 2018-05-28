## 什么是对象？
> js 中的所有事物都是对象：字符串、数字、数组、日期，等等。
> 在 js 中，对象是拥有属性和方法的数据。   
> ECMA-262把对象定义为：无序属性的集合，其属性可以包含基本值，对象或者函数。

## 创建对象方式
* 通过Object构造函数创建对象
```js
  var a=new Object();
  a.name = 'a';
  a.sayName = function() { console.log(this.name) }
  console.log(a)  // {name: "a", sayName: ƒ}
  console.log(a.__proto__)  // Object {}
  console.log(a.__proto__ === a.constructor.prototype) // true
  console.log(a.constructor)  // ƒ Object() { [native code] }
  
  描述：简单
```
* 通过字面量创建对象
```js
  var person={
    name: 'a',
    sayName: function() { console.log(this.name) }
  };
  console.log(a)  // {name: "a", sayName: ƒ}
  console.log(a.__proto__)  // Object {}
  console.log(a.__proto__ === a.constructor.prototype) // true
  console.log(a.constructor)  // ƒ Object() { [native code] }
  
  描述：对象字面量相对于Object构造函数代码量少了一点点。
        但是这2种方法通过一个接口创建很多对象，会产生大量重复代码。   
        我们需要对重复的代码进行抽象。工厂模式就是在这种情况下出现的。
```
* 工厂模式
```js
  function createPerson(name) {
      var o = new Object();
      o.name = name;
      o.sayName = function() { console.log(this.name) }
    return o;
  }
  var a=createPerson('a');
  console.log(a)  // {name: "a", sayName: ƒ}
  console.log(a.__proto__)  // Object {}
  console.log(a.__proto__ === a.constructor.prototype) // true
  console.log(a.constructor)  // ƒ Object() { [native code] }
  
  描述：工厂模式减少了重复代码，但是不能够识别对象，所有实例都是object类型的。
        这时构造函数模式就出现了。
```
* 构造函数模式
```js
  function Person(name) {
    this.name = name;
    this.sayName = function() { console.log(this.name) }
  }
  var a = new Person('a');
  var b = new Person('b');
  console.log(a)  // Person  {name: "a", sayName: ƒ}
  console.log(a.__proto__)  // Person {}
  console.log(a.__proto__ === a.constructor.prototype) // true
  console.log(a.constructor)  //  ƒ Person(name){this.name=name;this.sayName = function() { alert(this.name)}}
  console.log(a.constructor === Person) //true
  console.log(a.sayName === b.sayName) //false
  
  描述：a和b是Person的实例，同时也是Object的实例。因为所有的对象都继承自Object。
        创建自定义构造函数意味着将来可以将它的实例标识为一种特定的类型；而这正是构造函数胜过工厂模式的地方。
        使用构造函数创建对象，每个方法在每个实例上都要重新实现一遍，
        一是耗资源，二是创建两个或者多个完成同样任务的Function没有必要，
        三是有this在，没必要在代码执行前就把函数绑定到特定对象上。
```
* 原型模式
```js
  function Person() {
  }
  Person.prototype.name = 'a';
  Person.prototype.likes = ['c', 'd'];
  Person.prototype.sayName = function() {
      console.log(this.name);
  }

  var a = new Person();
  var b = new Person();
  console.log(a)  // Person  {}
  console.log(a.__proto__)  // {name: "a", likes: Array(2), sayName: ƒ, constructor: ƒ}
  console.log(a.__proto__ === a.constructor.prototype) // true
  console.log(a.constructor)  //  ƒ Person() {}
  console.log(a.constructor === Person) //true
  console.log(a.sayName === b.sayName) //true
  console.log(a.likes === b.likes);    //true  c, d
  a.likes.push('e');
  console.log(a.likes === b.likes);    //true  c, d, e
  
  描述：使用原型的好处是可以让所有的实例共享它所包含的属性和方法。完美的解决了构造函数的问题。
        因为原型是js中的一个核心内容，其信息量很大
        原型也有它本身的问题，共享的属性值如果是引用类型，一个实例对该属性的修改会影响到其他实例。
        这正是原型模式很少单独被使用的原因。
```
* 构造函数和原型混合模式
```js
  function Person(name) {
    this.name = name;
    this.likes = ['c', 'd'];
  }

  Person.prototype = {
    constructor: Person,
    sayName: function() {
      console.log(this.name);
    }
  }

  var a = new Person('a');
  var b = new Person('b');
  console.log(a)  // Person   {name: "a", likes: Array(2)}
  console.log(a.__proto__)  // {constructor: ƒ, sayName: ƒ}
  console.log(a.__proto__ === a.constructor.prototype) // true
  console.log(a.constructor)  //  ƒ Person(name) {this.name = name; this.likes = ['c', 'd'];}
  console.log(a.constructor === Person) //true
  console.log(a.likes === b.likes);    //false 内存指向不同 c, d
  a.likes.push('e');
  console.log(a.likes === b.likes);   //false 内存指向不同  c, d, e  | c, d
  
  描述： 这种构造函数与原型混合模式，是目前使用最广泛、认同度最高的一种创建自定义类型的方法。
         可以说，这是用来定义引用类型的一种默认模式。
         其实原型就是为构造函数服务的，配合它来创建对象，想要只通过原型一劳永逸的创建对象是不可取的，
         因为它只管创建共享的属性和方法，剩下的就交给构造函数来完成。
```
* 动态原型模式
```js
  function Person(name){
    this.name = name;
    this.likes = ['c', 'd'];
    if(typeof this.sayName != 'function') {
      Person.prototype.sayName = function() {
        console.log(this.name);
      }
    }
  }

  var a=new Person('a');
  a.sayName();
  var b=new Person('b');
  b.sayName();
  
  描述：动态原型模式的提出是因为混合模式中用了构造函数对象居然还没创建成功，还需要再操作原型。
        把所有信息都封装到构造函数中，即通过构造函数必要时初始化原型，
        在构造函数中同时使用了构造函数和原型，这就成了动态原型模式。
        真正用的时候要通过检查某个应该存在的方法是否有效，来决定是否需要初始化原型。。
```

### 未完待续
