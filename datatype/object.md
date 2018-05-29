## 什么是对象？
> js 中的所有事物都是对象：字符串、数字、数组、日期，等等。
> 在 js 中，对象是拥有属性和方法的数据。   
> ECMA-262把对象定义为：无序属性的集合，其属性可以包含基本值，对象或者函数。
## 对象分类
> 本地对象：独立于宿主环境的 ECMAScript 实现提供的对象   
```js
   常见的 Object, Function, Array, String, Boolean, Number, Date, RegExp, Error,   
   不常见的 EvalError, RangeError, ReferenceError, SyntaxError, TypeError, URIError   
```
> 内置对象：由 ECMAScript 实现提供的、独立于宿主环境的所有对象，在 ECMAScript 程序开始执行时出现    
> 宿主对象：由 ECMAScript 实现的宿主环境提供的对象。所有 BOM 和 DOM 对象都是宿主对象。
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
## Object 属性（原型链）
* Object.prototype.writable：属性是否可以被赋值 默认false
* Object.prototype.enumerable：属性是否可以被枚举 默认false
  > 影响到的函数 for…in Object.keys() JSON.stringify()
* Object.prototype.configurable：属性是否可配置 默认为false 
* Object.prototype.constructor：用于创建一个对象的原型。 

## Object 方法（原型链）

* hasOwnProperty  返回一个布尔值，表示某个对象是否含有指定的属性，而且此属性非原型链继承。
```js
  var obj = {a: 0}
  obj.hasOwnProperty('a')   //true
  obj.hasOwnProperty('b')   //false
```
* isPrototypeOf   返回一个布尔值，表示指定的对象是否在本对象的原型链中。
```js
  var a = {
    b: 0
  }
  var c = [1, 2]
  var d = function() {}
  console.log(Object.prototype.isPrototypeOf(a)) //true
  console.log(Array.prototype.isPrototypeOf(c)) //true
  console.log(Object.prototype.isPrototypeOf(c)) //true
  console.log(Function.prototype.isPrototypeOf(d)) //true
  console.log(Object.prototype.isPrototypeOf(d)) //true
```
* propertyIsEnumerable  判断指定属性是否可枚举。通常原型链或预定义的属性 返回false，自定义返回true
```js
  var obj = {a: 1}
  var arr = [1, 2, 3]
  Object.prototype.hhhh = 'hello world'
  console.log(obj.a, obj.hhhh)  //1 "hello world"
  console.log(obj.propertyIsEnumerable('a'))  //true
  console.log(obj.propertyIsEnumerable('hhhh')) //false
  console.log(arr.propertyIsEnumerable(1))  //true
```
* toString 返回对象的字符串表示。
```js
  var a = {
    b: 0
  }
  var c = [1, 2]
  var d = function() {}
  console.log(a.toString()) //[object Object]
  console.log(c.toString()) //1,2
  console.log(d.toString()) //function () {}
```
* valueOf() 返回指定对象的原始值。
```js
  var a = {
    b: 0
  }
  var c = [1, 2]
  var d = function() {}
  console.log(a.valueOf()) //{b: 0}
  console.log(c.valueOf()) //[1, 2]
  console.log(d.valueOf()) //ƒ () {}
```
* watch 给对象的某个属性增加监听; unwatch 移除对象某个属性的监听。 不推荐使用，性能、兼容性问题
## Object 方法（自身）
* Object.assign(target, …sources) 把任意多个的源对象自身的可枚举属性拷贝给目标对象，然后返回目标对象。
```js
  var obj = {a: 1};
  var copy = Object.assign({}, obj);
  copy.name = '111'
  console.log(copy); // {a: 1};

  var o1 = {a: 1};
  var o2 = {b: 2};
  var o3 = {c: 3};

  var obj = Object.assign(o1, o2, o3);
  console.log(obj); //{a: 1, b: 2, c: 3}
  console.log(o1); //{a: 1, b: 2, c: 3}, 目标对象被改变了
```
* 属性描述符 分 数据描述符和存取描述符
> 数据描述符和存取描述符是一个具有值的属性，该值可能是可写的，也可能不是可写的。
> 存取描述符是由getter-setter函数对描述的属性。
> 描述符必须是这两种形式之一；不能同时是两者。
```js
  configurable
    当且仅当该属性的 configurable 为 true 时，该属性描述符才能够被改变，
    同时该属性也能从对应的对象上被删除。默认为 false。
    
  enumerable
    当且仅当该属性的enumerable为true时，该属性才能够出现在对象的枚举属性中。默认为 false。
    数据描述符同时具有以下可选键值：

  value
    该属性对应的值。可以是任何有效的 JavaScript 值（数值，对象，函数等）。默认为 undefined。
    
  writable
    当且仅当该属性的writable为true时，value才能被赋值运算符改变。默认为 false。
    存取描述符同时具有以下可选键值：

  get
    一个给属性提供 getter 的方法，如果没有 getter 则为 undefined。
    该方法返回值被用作属性值。默认为 undefined。
    
  set
    一个给属性提供 setter 的方法，如果没有 setter 则为 undefined。
    该方法将接受唯一参数，并将该参数的新值分配给该属性。默认为 undefined。
```
> ![props](https://github.com/arch-leo/primary/blob/master/images/3.jpg)  
>    
* Object.defineProperty(obj, prop, descriptor) 直接在一个对象上定义一个新属性，或者修改一个已经存在的属性， 并返回这个对象。
> obj：需要定义属性的对象。prop：需定义或修改的属性的名字。descriptor：将被定义或修改的属性的描述符。





* Object.create(proto, props) 创建一个拥有指定原型和若干个指定属性的对象。
> props 对应Object.defineProperties(obj, props)的第二个参数
```js
  var obj = Object.create(null)	// 无原型的对象	
  console.log(obj)	//{} 无原型
  obj.name = '1111'
  console.log(obj)	//{name: "1111"}
  console.log(obj.__proto__)	//undefined

  function Person () {}
  var o = Object.create(Person.prototype, {
    a: {
      value: 0,
      configureable: true,
      enumerable: true,
      writable: false
    },
    b: {
      value: 1,
      configureable: true,
      enumerable: false,
      writable: true
    }
  })
  console.log(o) //Person {a: 0, b: 1}
  console.log(o.a) //0
  console.log(o.b) //1
  o.a = 2
  o.b = 3
  console.log(o) //Person {a: 0, b: 3}
  console.log(o.a) //0
  console.log(o.b) //3

  for(var key in o) {
    console.log('key: ' + key)	// key: a
  }
```










### 未完待续
