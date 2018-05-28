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
