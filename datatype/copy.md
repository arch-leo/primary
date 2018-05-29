## 浅拷贝
> 复制引用，复制后的引用都是指向同一个对象的实例，彼此之间的操作会互相影响
```js
  var o = { a:1, b: { c: 1} };
  var copy = o
  console.log(o) //{a: 1, b: { c: 1 }}
  console.log(copy) //{a: 1, b: { c: 1 }}
  copy.b.c = 2
  console.log(o.b) //{a: 1, b: { c: 2 }}
  console.log(copy.b) //{a: 1, b: { c: 2 }}
```
```js
  var o = { a:1, b: { c: 1} };
  var copy = shallowCopy(obj);

  function shallowCopy(obj) {
    var res = {};
    for (var i in obj) {
        res[i] = obj[i];
    }
    return res;
  }
  console.log(o) //{a: 1, b: { c: 1 }}
  console.log(copy) //{a: 1, b: { c: 1 }}
  copy.b.c = 2
  console.log(o.b) //{a: 1, b: { c: 2 }}
  console.log(copy.b) //{a: 1, b: { c: 2 }}
```
```js
  var o = { a:1, b: { c: 1} };
  var copy = Object.assign({}, o);
  console.log(o) //{a: 1, b: { c: 1 }}
  console.log(copy) //{a: 1, b: { c: 1 }}
  copy.b.c = 2
  console.log(o.b) //{a: 1, b: { c: 2 }}
  console.log(copy.b) //{a: 1, b: { c: 2 }}
```
## 深拷贝
> 深复制不是简单的复制引用，而是在堆中重新分配内存，   
> 并且把源对象实例的所有属性都进行新建复制，   
> 以保证深复制的对象的引用图不包含任何原有对象或对象图上的任何对象，   
> 复制后的对象与原来的对象是完全隔离的
```js
  //仅支持 {} []对象的复制 不支持Date RegExp...
  var o = {
    a: 1,
    b: {
      c: 2 
    },
    d: [
      { 
        e: 3
      },
      { 
        f: 4
      },
      { 
        g: null
      }
    ]
  }

  function deepCopy(obj) {
    if(obj === null || typeof obj !== 'object'){
       return obj;
    }
    let res = Array.isArray(obj) ? [] : {}
    for (let i in obj) {
      //如果obj[i]不是原始类型数据，就递归
      if (obj[i] !== null && obj[i] !== undefined && typeof obj[i] !== 'boolean' && typeof obj[i] !== 'string' && typeof obj[i] !== 'number') {
        res[i] = deepCopy(obj[i])
      } else {
        res[i] = obj[i]
      }
    }
    return res
  }
  var copy = deepCopy(o)
  console.log(o)
  console.log(copy)

  o.a = 10
  o.b.c = 10
  o.d[0].e = 10
  console.log(o)
  console.log(copy)
```
```js
  var o = {
    a: 1,
    b: {
      c: 2 
    },
    d: [
      { 
        e: 3
      },
      { 
        f: 4
      },
      { 
        g: null
      }
    ]
  }

  function deepCopy(obj) {
    return JSON.parse(JSON.stringify(obj))
  }
  var copy = deepCopy(o)
  console.log(o)
  console.log(copy)

  o.a = 10
  o.b.c = 10
  o.d[0].e = 10
  console.log(o)
  console.log(copy)
```
