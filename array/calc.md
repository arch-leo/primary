##  从一个无序，任意的正数数字数组中，选取2个数，使其和为M实现算法
```js
// 直接排除for 多层循环  性能太低
//正确做法 先排序 然后二分查找
function choose(arr, sum){
  arr.sort(function(a, b) {
    return a - b
  })
  let i = 0
  let j = arr.length - 1
  let res = []
  for (let k = 0; k < arr.length; k++) {
    if (arr[i] + arr[j] == sum) {
      res.push([arr[i], arr[j]])
      i++
      j--
      if (i == j) {
        break
      }
    } else if (arr[i] + arr[j] < sum) {
      i++
    } else {
      j--
    }
  }
  return res
}
console.log(choose([0, 14, 11, 1, 2, 12, 8, 9, 5, 7, 6, 4, 3], 12))
// [0, 12] [1, 11] [3, 9] [4, 8] [5, 7]
```
```js
// 对象接受已经遍历过的元素 获取index 数组
function getItem(arr, sum){
  const res = [];
  const tmp = [];
  for (let i = 0; i < arr.length; i++) {
    const diff = sum - arr[i];
    if (tmp[diff] !== undefined) {
      res.push([diff, arr[i]]);
    } else {
      tmp[arr[i]] = diff
    }
  }
  return res
}
console.log(getIdx([0, 14, 11, 1, 2, 12, 8, 9, 5, 7, 6, 4, 3], 12))
// [11, 1], [0, 12], [5, 7], [8, 4], [9, 3]
```
```js
// 对象接受已经遍历过的元素 获取index 数组
function getIdx(arr, sum){
  const res = [];
  const tmp = [];
  for (let i = 0; i < arr.length; i++) {
    const diff = sum - arr[i];
    if (tmp[diff] !== undefined) {
      res.push([tmp[diff], i]);
    } else {
      tmp[arr[i]] = i
    }
  }
  return res
}
console.log(getIdx([0, 14, 11, 1, 2, 12, 8, 9, 5, 7, 6, 4, 3], 12))
// [2, 3], [0, 5], [8, 9], [6, 11], [7, 12]
```
##  实现二维数组里面的元素排列组合
```js
var arrays = [
  ['a0', 'a1', 'a2'],
  ['b0', 'b1'],
  ['c0'],
  ['d0', 'd1']
];
function getArrayByArrays(arrays) {
  var arr = [''];
  for (var i = 0; i < arrays.length; i++) {
    arr = getValuesByArray(arr, arrays[i]);
  }
  return arr;
}

function getValuesByArray(arr1, arr2) {
  var arr = [];
  for (var i = 0; i < arr1.length; i++) {
    var v1 = arr1[i];
    for (var j = 0; j < arr2.length; j++) {
      var v2 = arr2[j];
      var value = v1 + v2;
      arr.push(value);
    }
  } 
  return arr;
}
getArrayByArrays(arrays);
// ['a0b0c0d0', 'a0b0c0d1', 'a0b1c0d0', 'a0b1c0d1', 'a1b0c0d0', 'a1b0c0d1', 'a1b1c0d0', 'a1b1c0d1', 'a2b0c0d0', 'a2b0c0d1', 'a2b1c0d0', 'a2b1c0d1']
```

##  随机生成1-50之间的不重复的10个元素的数组
```js
//方法一
function getArr (arr) {
  arr = arr ? arr : [] 
  if (arr.length < 10) {
    var num = Math.floor(Math.random() * 50) + 1
      if (arr.indexOf(num) < 0) {
        arr.push(num)

      }
      getArr(arr)
  }
    return arr
}
console.log(getArr())

//方法二
var arr = []
while(arr.length < 10) {
  for (var i = 0; i > -1; i++) {
    var num = Math.floor(Math.random() * 50) + 1
    if (arr.indexOf(num) < 0) {
      arr.push(num)
      break;
    }
  }
}
console.log(arr)
```

### 扩展知识 01背包问题 （百度搜索）
