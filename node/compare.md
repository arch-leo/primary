## express koa koa2 fastify理解与对比
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
* fastify
> 对应js版本 es7
> 异步处理方式 async/await+Promise + JSON Schema
> 特点：更高效（3W+请求/s）
```js
  //栗子
  const fastify = require('fastify')()
  fastify.route({
    method: 'GET',
    url: '/',
    schema: {
      // request needs to have a querystring with a `name` parameter
      querystring: {
        name: { type: 'string' }
      },
      // the response needs to be an object with an `hello` property of type 'string'
      response: {
        200: {
          type: 'object',
          properties: {
            hello: { type: 'string' }
          }
        }
      }
    },
    // this function is executed for every request before the handler is executed
    beforeHandler: async (request, reply) => {
      // E.g. check authentication
    },
    handler: async (request, reply) => {
      return { hello: 'world' }
    }
  })

  const start = async () => {
    try {
      await fastify.listen(3000)
      fastify.log.info(`server listening on ${fastify.server.address().port}`)
    } catch (err) {
      fastify.log.error(err)
      process.exit(1)
    }
  }
  start()
  
  //为什么更高效 处理速度更快？
  答案就是　JSON　schema！
  其比较独有的特色其实是针对输出 JSON 的场景。其内置了基于 JSON schema 的 validation 和 serialization。这是其他框架没有的。  
  通常 JSON.stringify 的执行逻辑是什么样的呢？你需要对参数判断下类型，根据不同的类型执行不同的序列化方法，  
  比如对于字符串你就需要两头加双引号，同时对一些特殊字符（如双引号、反斜杠、所有小于32的 Unicode 字符等）做 escape；  
  如果是对象，你需要两头加大括号，遍历所有的 key 和 value，并把某些（比如所有的函数）筛掉，   
  然后递归的对 key 和 value 做合适的序列化……
  
  而当你有 JSON schema 时，你就已经知道了类型，知道了对象上需要序列化的 key，  
  可以直接生成序列化代码，省下了类型判断、遍历key之类的开销。固然你生成的是 JS 代码，
  但是拜现代 JS 引擎的 JIT 优化所赐，其直接的属性访问、方法调用之类的效率完全不输于原生实现。  
 
  说白了当一个请求过来的时候 http://www.abc.com?a=1&b=2 
  服务器需要进行相应的url解析并序列化参数 然后进行查找数据库之类的操作
  但是 JSON　schema 做法就是 我把你需要解析的数据我提前给你序列化好，你直接用就好了
```



