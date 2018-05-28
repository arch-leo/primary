## 原型和原型链
* 原型
  > 对于所有的对象，都有__proto__属性，这个属性对应该对象的原型   
  > 对于函数对象，除了__proto__属性之外，还有prototype属性，当一个函数被用作构造函数来创建实例时，   
  > 该函数的prototype属性值将被作为原型赋值给所有对象实例（也就是设置实例的__proto__属性）
  ```js
    var a = {b: 1};
    console.log(a.__proto__ === a.constructor.prototype)    //true  => Object {}
    console.log(a.constructor)    //ƒ Object() { [native code] }
  ```
  ```js
    var a = function() {};
    console.log(a.__proto__ === a.constructor.prototype)    //true  => ƒ () { [native code] }
    console.log(a.constructor)    //ƒ Function() { [native code] }
  ```
* 原型链
  > 只要是对象就有原型, 并且原型也是对象, 因此只要定义了一个对象, 那么就可以找到他的原型,
  > 如此反复, 就可以构成一个对象的序列, 这个结构就被成为原型链
  
  > (当要访问实例的属性时，解析器会执行一次搜索。首先从实例对象开始，如果在实例中找到了这个属性，则返回该属性的值；
  > 重点是如果没有找到的话，则继续搜索[[Prototype]]指针指向的原型对象，在原型对象中查找该属性，如果找到，则返回该属性的值。
  > 所以通过这种方式多个实例就能共享保存在原型中的属性和方法。这也是js的原型链。)
  ```js
    var obj = new Object();
    //对象是有原型对象的，原型对象也有原型对象 对象的原型对象一直往上找，会找到一个null
      obj._proto_._proto_._proto_
    // 原型链示例
      var arr = [];
      arr -> Array.prototype ->Object.prototype -> null
      var o = new Object();
      o -> Object.prototype -> null;
  ```
  ```js
    function Person (name) {
      this.sname = name
    }
    Person.prototype.sayName = function() {   // <===== 向上查找
      console.log('Person:' + this.sname)
    }
    Function.prototype.sayName = function() {
      console.log('Function:' + this.sname)
    }
    Object.prototype.sayName = function() {
      console.log('Object:' + this.sname)
    }
    var p = new Person('小明')
    p.sayName()		//Person:小明
    
    解释：
      p.prototype.__proto__ === Person.prototype   //true
  ```
  ```js
    function Person (name) {
      this.sname = name
    }
    //Person.prototype.sayName = function() {
    //  console.log('Person:' + this.sname)
    //}
    Function.prototype.sayName = function() {   // <===== 跳过
      console.log('Function:' + this.sname)
    }
    Object.prototype.sayName = function() {   // <===== 向上查找
      console.log('Object:' + this.sname)
    }
    var p = new Person('小明')
    p.sayName()		//Object:小明  ==>  为什么不是 Function:小明 
    
    解释：
      Function.prototype.__proto__ !== Function.prototype   //true
      Function.prototype.__proto__ === Object.prototype   //true
        其实这一点我也有点困惑，不过也可以试着解释一下。
        Function.prototype是个函数对象，理论上他的__proto__应该指向 Function.prototype，
        就是他自己，自己指向自己，没有意义。
        JS一直强调万物皆对象，函数对象也是对象，给他认个祖宗，
        指向Object.prototype。Object.prototype.__proto__ === null，保证原型链能够正常结束。
  ```


