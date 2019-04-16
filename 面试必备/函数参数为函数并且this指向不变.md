## 函数的参数为函数，函数内部如何实现this指向不变

```js

function G(fn) {
  var de = function() {
    console.log('2', this); // 2 G {}
  }
  this.fn = function (fnc) {
    return fn.call(this, fnc);
  };
  console.log('1', this); // 1 G {}
  this.fn(de.bind(this))
}

new G(function(de) {
  console.log('3', this) // 3 G {}
  de();
});
```
