## 检测是否为数组
> const arr = [1, 2, 3]
```js
//isArray	true
console.log(Array.isArray(arr))

//instanceof	true
console.log(arr instanceof Array)

//原型链    true
console.log(arr.__proto__.constructor == Array)

## arguments 转数组？
> function transfer (a, b, c) {
>   console.log(arguments)   // 这里的arguments 不是数组 是对象 Arguments(3) [1, 2, 3, callee: ƒ, Symbol(Symbol.iterator): ƒ]
> }
```js
//Array.prototype.slice.call(arguments)能将具有 length 属性的对象转成数组，
//除了IE下的节点集合（因为ie下的dom对象是以com对象的形式实现的，js对象与com对象不能进行转换）

transfer(1, 2, 3)
console.log(Array.from(arguments))                            // [1, 2, 3]
console.log([...arguments])                                   // [1, 2, 3]
console.log(Array.prototype.slice.call(arguments, 0))         // [1, 2, 3]
console.log(Array.prototype.slice.apply(arguments, [0]))      // [1, 2, 3]
