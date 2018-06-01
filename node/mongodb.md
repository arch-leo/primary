## mongodb mongoose mysql理解与对比
* mongodb
> 概念：mongoDB与一些关系型数据库相比，它更显得轻巧、灵活，
    非常适合在数据规模很大、事务性不强的场合下使用。
    同时它也是一个对象数据库，没有表、行等概念。 
    也没有固定的模式和结构，所有的数据以文档的形式存储。 
```js
  1.下载： https://www.mongodb.com/download-center?ct=1000172319#community 
  
  2.创建数据库存储文件夹（E:\database\project1\可自定 data\db） E:\database\project1\data\db  
  
  3.cmd: echo logpath=F:\MongoDB\log\mongod.log> "F:\MongoDB\mongod.cfg"  
  
  4.指定数据库 cmd: mongod --dbpath E:\database\project1\data\db --logpath E:\database\project1\data\log\mongodb.log
  
  5.启动数据库 cmd: mongo
  mongod --dbpath=E:\database\project1\db --logpath=E:\database\project1\log\db.log --serviceName=MyDb --port=27018 --logappend --install
```
