## express koa koa2 理解与对比
* express
> 对应js版本 es5   
> 异步处理方式 callback 回调嵌套   
> 特点：内置中间件丰富，功能强大，不过回调嵌套不利于代码维护和可读性
```js
  fs.readFile('/etc/passwd', function (err, data) {
    if (err) throw err;
    fs.readFile('/etc/passwd2', function (err, data2) {
      if (err) throw err;
      // 在这里处理data和data2的数据
    })
  })
```
* koa
> 对应js版本 es6  又称 es2015,
> 异步处理方式 Generator函数+yield语句+Promise
> 特点：1.不在核心方法中绑定任何中间件，相比express更轻小  
>      2.使用内部co中间件将异步变同步，代码可读性增强
>      3.Generator可以暂停函数执行，返回任意表达式的值。
```js
  //代码构成
  function* g() {
    var o = 1;
    yield o++;
    yield o++;
    yield o++;
  }
  var gen = g();
  gen.next() // {value: 1, done: false}
  gen.next() // {value: 2, done: false}
  gen.next() // {value: 3, done: true}
  gen.next() // {value: undifined, done: false}
  
  //函数解析 栗子
  function* g() {
    var o = 1;
    var a = yield o++;
    console.log('a = ' + a);
    var b = yield o++;
  }
  var gen = g();
  console.log(gen.next()) //{value: 1, done: false}
  console.log('------') //---------
  console.log(gen.next(11)) //a=11 {value: 2, done: false}
  console.log(gen.next()) //value: undifined, done: true}
  /*
  解释：
    首先说，console.log(gen.next());的作用就是输出了{value: 1, done: false}，注意var a = yield o++;  
    由于赋值运算是先计算等号右边，然后赋值给左边，所以目前阶段，只运算了yield o++，并没有赋值。  
    然后说，console.log(gen.next(11));的作用，首先是执行gen.next(11)，得到什么？  
    首先：把第一个yield o++重置为11，然后，赋值给a，再然后，console.log('a = ' + a)；  
    打印a = 11，继续然后，yield o++，得到2，最后打印出来。    
    从这我们看出了端倪：带参数跟不带参数的区别是，带参数的情况，  
    首先第一步就是将上一个yield语句重置为参数值，  
    然后再照常执行剩下的语句。  
    总之，区别就是先有一步先重置值，接下来其他全都一样。  
  */
  //for of 遍历迭代器
  function* foo() {
    yield 1;
    yield 2;
    yield 3;
    yield 4;
    yield 5;
    return 6;
  }
  let a = foo();
  for (let v of a) {
    console.log(v);
  }
  // 1 2 3 4 5
```
* koa2
> 对应js版本 es7  又称 es2016   
> 异步处理方式 async/await+Promise   
> 特点：相比koa去除了co中间件，进一步精简内核 ，代码可读性增强
```js
  //栗子
  async function asyncAwaitFn(str) {
    return await new Promise((resolve, reject) => {
      setTimeout(() => {
        resolve(str)
      }, 1000);
    })
  }
  const parallel = async () => { //并行执行
    console.time('parallel')
    const parallelOne = asyncAwaitFn('string 1');
    const parallelTwo = asyncAwaitFn('string 2')
    //直接打印
    console.log(await parallelOne)
    console.log(await parallelTwo)
    console.timeEnd('parallel')
  }
  parallel()
  
  //co中间件简易实现
  function co(generator){
    var gen = generator();
    var next = function(data){
      var result = gen.next(data);
      if(result.done) return;
      if (result.value instanceof Promise) {
        result.value.then(function (d) {
          next(d);
        }, function (err) {
          next(err);
        })
      }else {
        next();
      }
    };
    next();
  }
  
  //使用
  co(function*(){
    var text1 = yield new Promise(function(resolve){
      setTimeout(function(){
        resolve("I am text1");
      }, 1000);
    });
    console.log(text1);
    var text2 = yield new Promise(function(resolve){
      setTimeout(function(){
        resolve("I am text2");
      }, 1000);
    });
    console.log(text2);
  });
  // I am text1 
  // I am text2
  
```
