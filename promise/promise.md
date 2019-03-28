## 手工实现promise
```js
function _Promise(handler) {
  this.status = 'pending';
  this.result = '';
  this.handler = function(resolve, reject) {
    return handler.call(this, resolve, reject);
  }
  this.handler(this.resolve.bind(this), this.reject.bind(this))
}

_Promise.prototype.resolve = function(result) {
  if (this.status === 'pending') {
    this.status = 'fullfilled';
    this.result = result;
  }
}

_Promise.prototype.reject = function(result) {
  if (this.status === 'pending') {
    this.status = 'rejected';
    this.result = result;
  }
}

_Promise.prototype.then = function(isResolve, isReject) {
  var _isPromise;
  if (this.status === 'fullfilled') {
     _isPromise = isResolve(this.result);
  }
  if (this.status === 'rejected' && arguments[1]) {
    var err = new TypeError(this.result);
    _isPromise = isReject(err);
  }
  if (_isPromise instanceof _Promise) {
    return _isPromise;
  }
  return this;
}

_Promise.prototype.catch = function(isReject) {
  var _isPromise;
  if (this.status === 'rejected') {
    var err = new TypeError(this.result);
    _isPromise = isReject(err);
  }
  if (_isPromise instanceof _Promise) {
    return _isPromise;
  }
  return this;
}

var pm = new _Promise(function(resolve, reject) {
  var ss = 55;
  reject(ss)
}).catch(function(err) {
  console.log(err);
});

```


