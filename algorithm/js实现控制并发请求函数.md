## js控制并发请求
> 题目：请实现如下的函数，可以批量请求数据，  
> 所有的URL地址在 urls 参数中，  
> 同时可以通过 max 参数控制请求的并发数，  
> 当所有的请求结束之后，需要执行 callback 回调函数。  
> 发请求的函数可以直接使用 fetch 即可  
>

```js
function sendRequest(urls, max, callback) {
  const len = urls.length;
  let idx = 0;
  let counter = 0;

  function _request() {
    // 有请求，有通道
    while (idx < len && max > 0) {
      max--; // 占用通道
      fetch(urls[idx++]).finally(() => {
        max++; // 释放通道
        counter++;
        if (counter === len) {
          return callback();
        } else {
          _request();
        }
      });
    }
  }
  _request();
}
var urls = new Array(7).fill(true).map((x, i) => `https://github.com/hstarorg/HstarDoc/issues/${i + 1}`);

sendRequest(urls, 5, function() {
  console.log('done');
});

```
