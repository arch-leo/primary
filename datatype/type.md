## 基本数据类型 
> 存放在栈(stack)内存中的简单数据段，数据大小确定，内存空间大小可以分配。  
> Number,String, Null, Undefined, Boolean, Symbol
```js
// 用typeof可以检测出变量的基本数据类型，但是有个特例，
// 就是null的typeof返回的是object，这个是javascript的历史Bug
// ES6新增数据类型Symbol: 表示独一无二的值 不能用new去实例
console.log(typeof Symbol())      // 'symbol'
console.log(typeof 1)     // 'number'
console.log(typeof '1')     // 'string'
console.log(typeof function a() {})     // 'function'
console.log(typeof true)      // 'boolean'
console.log(typeof null)      // 'object'
console.log(typeof undefined)     // 'undefined'
```
## 引用数据类型
> 存放在堆(heap)内存中的对象，变量实际保存的是一个指针，这个指针指向另一个位置。     
> Object
```js
// 栗子 
new Array(3) 
new Function(a, b, 'alert("a + b = " + a + b)')
...
```
