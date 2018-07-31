## es6 Map对象
> Map对象就是简单的键值对映射。其中的键和值可以使任意值。  
* 创建方法1
```js
  let map = new Map()
  map.set('a', 1)
  map.set('b', 2)
  map.set('c', 3)
  或  
  let map = new Map([['a', 1], ['b', 2], ['c', 3]])
  
  console.log(map)            // {'a' => 1, 'b' => 2,'c' => 3}
  console.log(map.get('a'))   // 1
```
* 键的比较
> 键的比较规则：NaN 是与NaN是相同的（虽然NaN !== NaN）,除此之外所有的值都根据'==='判断。
```js
  let map = new Map();
  map.set(Number('aa111'), 'isNaN')
  map.set(Number('bb222'), 'also is NaN')
  map.get(NaN)       //'also is NaN'
```
* Map VS Object
```js
  一个对象通常都有自己的原型，所以一个对象总有一个"prototype"键。
  不过，从ES5开始可以使用map = Object.create(null)来创建一个没有原型的对象。 
  ( 不是所有的对象都有原型？ )
  一个对象的键只能是字符串或者Symbols，但一个Map的键可以是任意值。
  你可以通过size属性很容易地得到一个Map的键值对个数，而对象的键值对个数只能手动确认。
```
* Map属性
```js
  Map.length 属性length的值为0。
  Map.prototype 表示Map构造器的原型。允许添加属性从而应用与所有的Map对象。
```
* Map实例 - 所有Map对象的实例都会继承Map.prototype。
```js
1.属性
  Map.prototype.constructor 返回创建给map实例的构造函数，默认是Map函数。   
  Map.prototype.size 返回Map对象的键值对的数量。
  
  let map = new Map([['a',1], ['b', 2]]);
  console.log(map.constructor)    //function Map() { [native code] }
  console.log(map.size)           //2

2.方法  
  --- set(key, value) 设置键名key对应的键值为value，然后返回整个 Map 结构。如果key已经有值，则键值会被更新，否则就新生成该键。

      map.set('qwe', '123').set('new', 'fq').set('yu', 'li');
      console.log(map); //{"a" => 1, "b" => 2, "qwe" => "123", "new" => "fq", "yu" => "li"}
      
  --- get(key) get方法读取key对应的键值，如果找不到 key，返回undefined。

      console.log(map.get('new')); //true
      console.log(map.get('x')); //false
      
  --- delete(key) 删除某个键，返回true。如果删除失败，返回false。

      console.log(map.delete('a')); //true
      console.log(map); //{ "b" => 2, "qwe" => "123", "new" => "fq", "yu" => "li"}
      console.log(map.delete('a')); //false
  --- has(key) 方法返回一个布尔值，表示某个键是否在当前Map对象之中。

      console.log(map.has('yu')); //true
      console.log(map.has('a')); //false
      
  --- clear() 清除所有数据，没有返回值。
  
      map.clear(); console.log(map); // {}
      
  --- keys() 返回键名的遍历器
  
      console.log(map.keys());
      
  --- values() 返回键值的遍历器
  
      console.log(map.values());
      
  --- entries() 返回键值对的遍历器
      
      var map = new Map([[1, 1], [2, 2], [3, 3]])
      console.log(map.entries());
      for(let item of map.entries()) {
          console.log(item); //[1, 1] [2, 2] [3, 3]
      }
      
  --- forEach() 使用回调函数遍历每个成员

      map.forEach(function(value, key, mapObj) {
          console.log(value + '---' + key + '---' + mapObj);
          //value - Map对象里每一个键值对的值
          //key - Map对象里每一个键值对的键
          //mapObj - Map对象本身
          console.log(this); //this === window
      });

      map.forEach(function(value, key, mapObj) {
          console.log(value + '---' + key + '---' + mapObj);
          console.log(this);    //this === map
      }, map)

```
