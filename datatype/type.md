## 基本数据类型 
> 存放在栈(stack)内存中的简单数据段，数据大小确定，内存空间大小可以分配。
> Number,String, Null, Undefined, Boolean, Object, Symbol
```js
用typeof可以检测出变量的基本数据类型，但是有个特例，就是null的typeof返回的是object，这个是javascript的历史Bug
ES6新增数据类型Symbol: 表示独一无二的值 不能用new去实例
console.log(typeof Symbol()) 		    // 'symbol'
console.log(typeof 1) 				      // 'number'
console.log(typeof '1') 			      // 'string'
console.log(typeof function a() {}) // 'function'
console.log(typeof {}) 				      // 'object'
console.log(typeof true) 			      // 'boolean'
console.log(typeof null) 			      // 'object'
console.log(typeof undefined) 		  // 'undefined'
```
## 引用数据类型
> 存放在堆(heap)内存中的对象，变量实际保存的是一个指针，这个指针指向另一个位置。 
> Array, Function, Date, RegExp, Buffer, Set, Map...
```js
栗子 
  new Array(3) 
  new Function(a, b, 'alert("a + b = " + a + b)')
  ...
```
> es6 Set对象
>   Set对象允许你存储任意类型的唯一值（不能重复），无论它是原始值或者是对象引用。
```js
  new Set([1, 2])  // {1, 2}        
```
> es6 Map对象
>   Map对象就是简单的键值对映射。其中的键和值可以使任意值。
```js
> 创建方法1
  let map = new Map()
  map.set('a', 1)
  map.set('b', 2)
  map.set('c', 3)
  
  创建方法2
  let map = new Map([['a', 1], ['b', 2], ['c', 3]])
  console.log(map)            // {'a' => 1, 'b' => 2,'c' => 3}
  console.log(map.get('a'))   // 1
> 键的比较
  键的比较规则：NaN 是与NaN是相同的（虽然NaN !== NaN）,除此之外所有的值都根据'==='判断。
  let map = new Map();
  map.set(Number('aa111'), 'isNaN')
  map.set(Number('bb222'), 'also is NaN')
  map.get(NaN)       //'also is NaN'
> Map VS Object
  * 一个对象通常都有自己的原型，所以一个对象总有一个"prototype"键。
    不过，从ES5开始可以使用map = Object.create(null)来创建一个没有原型的对象。 
    ( 不是所有的对象都有原型？ )
  * 一个对象的键只能是字符串或者Symbols，但一个Map的键可以是任意值。
  * 你可以通过size属性很容易地得到一个Map的键值对个数，而对象的键值对个数只能手动确认。


```













