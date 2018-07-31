## es6 Set对象
> Set对象允许你存储任意类型的唯一值（不能重复），无论它是原始值或者是对象引用。   
* Set实例 - 所有Set对象的实例都会继承Set.prototype。
```js
1.属性
  Set.prototype.constructor 返回创建给set实例的构造函数，默认是Set函数。   
  Set.prototype.size 返回Set对象的成员数量。
  
  let set = new Set([1, 2]);
  console.log(set.constructor)    //function Set() { [native code] }
  console.log(set.size)           //2

2.方法  
  --- add(value) 添加一个值，返回Set结构本身

      set.add(1).add(2)   //链式
      console.log(set);   //{1, 2}
      
  --- delete(value) 删除某个值，返回布尔值

      set.delete(1)       //true
      console.log(set);   //{2}
   
   --- has(value) 返回布尔值，表示是否是成员

      set.has(1)          //false
      set.has(2)          //true
      console.log(set);   //{2}
      
   --- clear() 清除所有成员，无返回值
   
      console.log(set);     //Set(2) {1, 2}
      set.clear()           //false
      console.log(set);     //Set(0) {}
      
  --- keys() 返回键名的遍历器
  
      console.log(set.keys());
      
  --- values() 返回键值的遍历器
  
      console.log(map.values());
      
  --- entries() 返回键值对的遍历器
  
      var set = new Set([1, 2, 3])
      console.log(set.entries());   //Iterator 迭代器对象 {1, 2, 3}
      for(let item of set.entries()) {
          console.log(item); //[1, 1] [2, 2] [3, 3]
      }
      
  --- forEach() 使用回调函数遍历每个成员
      
      var set = new Set([1, 2, 3])
      set.forEach(function(value, key, setObj) {
          console.log(value + '---' + key + '---' + setObj);
          //value - Map对象里每一个键值对的值
          //key - Map对象里每一个键值对的键
          //mapObj - Map对象本身
          console.log(this); //this === window
      });

      set.forEach(function(value, key, setObj) {
          console.log(value + '---' + key + '---' + setObj);
          console.log(this);    //this === map
      }, set)
```
