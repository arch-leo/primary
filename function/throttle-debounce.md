## 函数节流和函数防抖 (throttle-debounce)
* 函数节流
> 定义：多次触发事件后，事件处理函数只执行一次，并且是在触发操作结束时执行。
> 原理：对处理函数进行延时操作，若设定的延时到来之前，再次触发事件，则清除上一次的延时操作定时器，重新定时。
```js
  // 函数节流 以监听 滚动条滚动为例
  var canRun = true;
  document.getElementById("throttle").onscroll = function(){
    if(!canRun){
      // 判断是否已空闲，如果在执行中，则直接return
      return;
    }

    canRun = false;  //核心点就是 预设 标识
    setTimeout(function(){
      console.log("函数节流");
      canRun = true;
    }, 300);
  };
  
  // 简单实现
  function throttle (fn, wait, mustRun) {
    let start = new Date()
    let timeout
    return () => {
      // 在返回的函数内部保留上下文和参数
      let context = this
      let args = arguments
      let current = new Date()

      clearTimeout(timeout)

      let remaining = current - start
      // 达到了指定触发时间，触发该函数
      if (remaining > mustRun) {
        fn.apply(context, args)
        start = current
      } else {
        // 否则wait时间后触发，闭包保留一个timeout实例
        timeout = setTimeout(fn, wait);
      }
    }
  }
  
  // underscore.js throttle
  _.throttle = function(func, wait, options) {
    var context, args, result;
    var timeout = null;
    var previous = 0;
    if (!options) options = {};
    var later = function() {
      previous = options.leading === false ? 0 : _.now();
      timeout = null;
      result = func.apply(context, args);
      if (!timeout) context = args = null;
    };
    return function() {
      var now = _.now();
      if (!previous && options.leading === false) previous = now;
      var remaining = wait - (now - previous);
      context = this;
      args = arguments;
      if (remaining <= 0 || remaining > wait) {
        if (timeout) {
          clearTimeout(timeout);
          timeout = null;
        }
        previous = now;
        result = func.apply(context, args);
        if (!timeout) context = args = null;
      } else if (!timeout && options.trailing !== false) {
        timeout = setTimeout(later, remaining);
      }
      return result;
    };
  };
```
* 函数防抖
> 定义：触发函数事件后，短时间间隔内无法连续调用，只有上一次函数执行后，过了规定的时间间隔，才能进行下一次的函数调用。
> 原理：对处理函数进行延时操作，若设定的延时到来之前，再次触发事件，则清除上一次的延时操作定时器，重新定时。
```js
  // 函数防抖 以监听 滚动条滚动为例
  var timer = false;
  document.getElementById("debounce").onscroll = function(){
    clearTimeout(timer); // 清除未执行的代码，重置回初始化状态

    timer = setTimeout(function(){  //核心点是延迟 执行
      console.log("函数防抖");
    }, 300);
  };
  
  // 简单实现
  function debounce(fn, wait) {
    let t
    return () => {
      let context = this
      let args = arguments
      if (t) clearTimeout(t)
      t= setTimeout(() => {
          fn.apply(context, args)
      }, wait)
    }
  }
  
  // underscore.js debounce
  _.debounce = function(func, wait, immediate) {
    var timeout, args, context, timestamp, result;

    // 处理时间
    var later = function() {
      var last = _.now() - timestamp;

      if (last < wait && last >= 0) {
        timeout = setTimeout(later, wait - last); // 10ms 6ms 4ms
      } else {
        timeout = null;
        if (!immediate) {
          result = func.apply(context, args);
          if (!timeout) context = args = null;
        }
      }
    };
```
