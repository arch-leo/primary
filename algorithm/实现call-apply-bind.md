## 原生js实现 bind
```js
Function.prototype.bind2 = function(ctx = window) {
  if (typeof this !== 'function') {
    return this;
  }
  const args = [...arguments].slice[1];
  const fn = this;
  function tmp() {}
  let res = function() {
    return fn.apply(this instanceof tmp ? this : ctx, args.concat([...arguments]));
  }
  tmp.prototype = this.prototype;
  res.prototype = new tmp();
  return res;
}
```

## 原生js实现 call
```js
Function.prototype.call2 = function(ctx = window) {
  ctx.fn = this;
  const args = [...arguments].slice(1);
  const res = ctx.fn(...args);
  delete ctx.fn;
  return res;
}
```

## 原生js实现 apply
```js
Function.prototype.apply2 = function(ctx = window) {
  ctx.fn = this;
  let res;
  if (arguments[1]) {
    res = ctx.fn(...arguments[1]);
  } else {
    res = ctx.fn();
  }
  delete ctx.fn;
  return res;
}
```
