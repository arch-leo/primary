# 数组扁平化
* 二维数组
```js
const arr = [[1, 2], 3, 4]
// apply
console.log([].concat.apply([], arr))    // [1, 2, 3, 4]
// ...扩展运算符
console.log([].concat(...arr))           // [1, 2, 3, 4]
