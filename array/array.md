## 检测数组
> const arr = [1, 2, 3]
```js
//isArray	true
console.log(Array.isArray(arr))

//instanceof	true
console.log(arr instanceof Array)

//原型链    true
console.log(arr.__proto__.constructor == Array)
```
## arguments 转数组？
> function transfer(a, b, c){console.log(arguments)}
  这里的arguments 不是数组 是对象 
  Arguments(3) [1, 2, 3, callee: ƒ, Symbol(Symbol.iterator): ƒ]
```js
//Array.prototype.slice.call(arguments)能将具有 length 属性的对象转成数组，
//除了IE下的节点集合（因为ie下的dom对象是以com对象的形式实现的，js对象与com对象不能进行转换）

transfer(1, 2, 3)
console.log(Array.from(arguments))                            // [1, 2, 3]
console.log([...arguments])                                   // [1, 2, 3]
console.log(Array.prototype.slice.call(arguments, 0))         // [1, 2, 3]
console.log(Array.prototype.slice.apply(arguments, [0]))      // [1, 2, 3]
```
## 数组 去重
> const arr = [1, 5, 1, 6, 2, 8, 2, 3, 10]
```js
// 方法1 indexOf 
let tempArr = []
for (let i = 0; i < arr.length; i++) {
  if (tempArr.indexOf(arr[i]) < 0) {
    tempArr.push(arr[i])
  }
}
console.log(tempArr)  // [1, 5, 6, 2, 8, 3, 10]

// 方法2    es6 Set对象	似于数组，特性就是所有元素都是唯一的，没有重复。
let tempArr = [...new Set(arr)]
console.log(tempArr)  // [1, 5, 6, 2, 8, 3, 10]

// 方法3    同上	Array.from
let set = new Set(arr)
let tempArr = Array.from(set)
console.log(tempArr)  // [1, 5, 6, 2, 8, 3, 10]
```
## 数组 查重
> const arr = [1, 5, 1, 6, '1', 1, 2, '8', 8, '1', 18, '8', 2, 3, 10, '5']
```js
// 方法1
let obj = {}
let tempArr = []
arr4.forEach(v => {
  if (obj4[v + typeof v]) {
    tempArr.push(v)
  } else {
    obj[v + typeof v] = 1
  }
})
console.log(tempArr)    //  [1, 1, '1', '8', 2]
// 方法2 原理同上 key-value
let getRepeat = arr => 
  ((m) => arr.reduce((acc, cur) => m.has(cur) ? acc.concat(cur) : m.set(cur, 1) && acc, []))(new Map())
let tempArr = getRepeat(arr)
console.log(tempArr)    //  [1, 1, '1', '8', 2]
```
## 数组 排序
> const arr = [1, 5, 1, 6, 3]
```js
// 方法1  冒泡排序
function bubbleSort(array) {
  var len = array.length
  var d
  for(i = 0; i < len; i++) {
    for(j = 0; j < len; j++) {
      if(array[i] < array[j]) {
        d = array[j];
        array[j] = array[i];
        array[i] = d;
      }
    }
  }
  return array;
}
console.log(bubbleSort(arr))    //  [1, 1, 3, 5, 6]

// 方法2 插入排序
function insertSort(array) {
  var j, temp, key, len = array.length;
  for (i = 1; i < len; i++) {
    temp = j = i;
    key = array[j];
    while(--j > -1) {
      if(array[j] > key) {
        array[j + 1] = array[j];
      } else {
        break;
      }
    }
    array[j + 1] = key;
  }
  return array;
}
console.log(insertSort(arr))

// 方法2 sort排序
arr.sort(function(a, b){
  return a-b
})
console.log(arr)            // [1, 1, 3, 5, 6]
```
