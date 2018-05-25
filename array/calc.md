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
