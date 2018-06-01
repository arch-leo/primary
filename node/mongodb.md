## mongodb mongoose mysql理解与对比
* mongodb
> 概念：mongoDB与一些关系型数据库相比，它更显得轻巧、灵活，
    非常适合在数据规模很大、事务性不强的场合下使用。
    同时它也是一个对象数据库，没有表、行等概念。 
    也没有固定的模式和结构，所有的数据以文档的形式存储。 
```js
  Windows 10 v = 3.6.5
  1.下载： https://www.mongodb.com/download-center?ct=1000172319#community  => msi文件
  
  2.安装 选项 我接受 => custom => 选择安装路径 => 取消勾选左下角compass => 安装完成
  
  3.创建项目数据库文件位置 E:\DbsForMongo\project1\db
  
  4.创建项目数据库日志文件文件 E:\DbsForMongo\project1\log\mongodb.log
  
  5.创建mongodb服务 cmd进入 D:\Program Files\MongoDB\Server\3.6  => 安装目录
  mongod --dbpath=E:\DbsForMongo\project1\db --logpath=E:\DbsForMongo\project1\log\db.log --serviceName=MyDb --port=27018 --logappend --install
  
  6.启动mongodb net start MyDb   => 对应第5步 --serviceName=MyDb
  
  7.cmd mongo --port=27018  => 启动成功 会出现 WARNING: Access control is not enabled for the database  不是报错只是警告 
    意思就是需要创建用户名 和 密码  
    
  8.紧接着  show dbs  
        admin   0.000GB
        config  0.000GB
        local   0.000GB
        
        //数据库创建成功
    
  9.紧接着  执行下面代码 （复制或者直接手动输入）
  use admin
  db.createUser(
    {
      user: "leo",
      pwd: "123",
      roles: [ { role: "userAdminAnyDatabase", db: "admin" } ]
    }
  )
  
  //成功显示下面界面
  Successfully added user: {
    "user" : "leo",
    "roles" : [
      {
        "role" : "userAdminAnyDatabase",
        "db" : "admin"
      }
    ]
  }
  10.Ctrl + C => net start MyDb => mongod
  mongod --dbpath=E:\DbsForMongo\project1\db --logpath=E:\DbsForMongo\project1\log\db.log --serviceName=MyDb --port=27018 --logappend --install
  
  // 删除mongodb服务
   mongod.exe --remove --serviceName "MongoDB"
  // 强制删除mongodb服务
  sc delete mongodb
  
  //可视化工具 
  mongodb compass
  robo3t 
  
  
  
```
* mongoose
> Mongoose是在node.js异步环境下对mongodb进行便捷操作的对象模型工具。  
> 封装了mongoDB对文档的一些增删改查等常用方法，让nodejs操作mongoDB数据库变得更加容易。
```js
  1.安装
    npm install mongoose
  2.引用
    var mongoose = require('mongoose')
  3.连接
    var db = mongoose.connect('mongodb://user:pass@localhost:port/database')
```


