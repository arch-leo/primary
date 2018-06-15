# 数组扁平化
* 二维数组
> const arr = [[1, 2], 3, 4]
```js

// apply
console.log([].concat.apply([], arr))    // [1, 2, 3, 4]
// ...扩展运算符
console.log([].concat(...arr))           // [1, 2, 3, 4]
```
* 多维数组
> const arr = [[1, 2], [[3, 4], 5, 6], [[[[[7, 8], 9, 10], 11, 12], 13, 14], 15, 16], 17, 18]
```js
// 完全扁平化 forEach
const res = []
function flatten(arr) {
  arr.forEach(function(i){
    if (i instanceof Array) {
      return flatten(i)
    } else {
      return res.push(i)
    }
  })
}
flatten(arr)
console.log(res)              // [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18]

// 完全扁平化 reduce
const flatten = arr => arr.reduce((a, b) => {
  if (Array.isArray(b)) {
    return a.concat(flatten(b))
  }
  return a.concat(b)
}, [])
console.log(flatten(arr))       // [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18]

// 完全扁平化 map
function flatten(arr){
  return Array.isArray(arr) ? [].concat(...arr.map(flatten)) : arr
}
console.log(flatten(arr))       // [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18]

// 可控扁平化 forEach
let res = []
function flatten (arr, depth) {
  let count = 0
  arr.forEach(function(i) {
    if (i instanceof Array && count < depth) {
      count++
      return flatten(i, depth, count)
    } else {
      count = 0
      return res.push(i)
    }
  })
}
flatten(arr, 3)
console.log(res)                //  [1, 2, 3, 4, 5, 6, [[7, 8], 9, 10], 11, 12, 13, 14, 15, 16, 17, 18]

// 可控扁平化 reduce
let count = 1
const flatten = (arr, depth) => arr.reduce((a, b) => {
  if (Array.isArray(b) && count < depth) {
    count++
    return a.concat(flatten(b, depth, count))
  } else {
    count = 1
    return a.concat(b)
  }
}, []);
console.log(flatten(arr, 3))    //  [1, 2, 3, 4, 5, 6, [[7, 8], 9, 10], 11, 12, 13, 14, 15, 16, 17, 18]

// 可控扁平化 map
function flatten(arr, deep = Infinity) {
  return deep && Array.isArray(arr) ? [].concat(...arr.map(x => flatten(x, deep - 1))) : arr;
}

console.log(flatten(arr, 3))    //  [1, 2, 3, 4, 5, 6, [[7, 8], 9, 10], 11, 12, 13, 14, 15, 16, 17, 18]

