
## 圣杯布局
```js
<!--圣杯布局-->
<style type="text/css">
  *{margin:0;padding:0;}
  .box{width:600px;height:300px;margin:0 auto;padding:0 100px 0 200px;background:black;}
  .main,.left,.right{min-height: 150px;float:left;}
  .main{background:red;width:100%;}
  .left{width:200px;margin-left:-100%;position:relative;left:-200px;background:blue;}
  .right{width:100px;margin-left:-100px;position:relative;right:-100px;background:green;}
</style>
<div class="box">
  <div class="main">main</div>
  <div class="left">left</div>
  <div class="right">right</div>
</div>
```

## 双飞翼布局
```js
<!--双飞翼-->
<style type="text/css">
  *{margin:0;padding:0;}
  .box{width:900px;height:300px;margin:0 auto;background:black;}
  .main,.left,.right{min-height: 150px;float:left;}
  .main{background:red;width:100%;}
  .cont{margin:0 100px 0 200px;}
  .left{width:200px;margin-left:-100%;background:blue;}
  .right{width:100px;margin-left:-100px;background:green;}
</style>
<div class="box">
  <div class="main">
    <div class="cont">main</div>
  </div>
  <div class="left">left</div>
  <div class="right">right</div>
</div>
```
