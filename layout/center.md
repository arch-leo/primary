## 常见布局问题
>1.需求  
* A元素水平垂直居中
```js
HTML:
  <body>
    <div class="box">
      <div>哈哈哈哈啊哈</div>
    </div>
  </body>
CSS: 布局1
  *{margin:0;padding:0;}
  html{height:100%;}
  body{height:100%;}
  .box{height:100%;background:#000;position:relative;}
  .box div{height:50%;width:50%;background:#f54343;position:absolute;left:0;right:0;top:0;bottom:0;margin:auto;color:#fff;}
CSS: 布局2
  *{margin:0;padding:0;}
  html{height:100%;}
  body{height:100%;}
  .box{height:100%;background:#000;display:flex;justify-content:center;align-items:center;}
  .box div{height:50%;width:50%;background:#f54343;}
CSS: 布局3
  *{margin:0;padding:0;}
  html{height:100%;}
  body{height:100%;}
  .box{height:100%;background:#000;position:relative;}
  .box div{height:50%;width:50%;background:#f54343;position:absolute;left:50%;top:50%;transform:translate(-50%, -50%);}
CSS: 布局4
  *{margin:0;padding:0;}
  html{height:100%;}
  body{height:100%;}
  .box{height:100%;background:#000;position:relative;}
  .box div{height:50%;width:50%;background:#f54343;position:absolute;left:50%;top:50%;transform:translate(-50%, -50%);}
```
#### 效果预览
![preview](https://github.com/arch-leo/primary/blob/master/images/2.jpg)

>2.需求  
* A元素垂直居中
*	A元素距离屏幕左右各边各10px
*	A元素里的文字font—size:20px,水平垂直居中
*	A元素的高度始终是A元素宽度的50%
```js
HTML:
  <body>
    <div class="box">
      <div>哈哈哈哈啊哈</div>
    </div>
  </body>
CSS:
  *{margin:0;padding:0;}
  .box{margin:0 10px;height:0;padding-bottom:calc(50% - 10px);background:#f54343;position:relative;}
  .box div{position:absolute;left:0;top:0;bottom:0;right:0;display:flex;justify-content:center;align-items:center;}
```
#### 效果预览
![preview](https://github.com/arch-leo/primary/blob/master/images/1.jpg)

>3.需求  
* 容器定高定宽
* 图片水平垂直居中
* 图片不能缩放
```js
HTML:
  <body>
    <div class="box">
      <img src="http://via.placeholder.com/100x320"/>
    </div>
  </body>
CSS:
  *{margin:0;padding:0;}
  .box{width:200px;height:200px;background:#000;position:fixed;left:50%;top:50%;transform:translate(-50%, -50%);/*overflow:hidden;*/}
  .box img{position:absolute;left:50%;top:50%;transform:translate(-50%, -50%);}
```
#### 效果预览
![preview](https://github.com/arch-leo/primary/blob/master/images/4.jpg)

>4.需求  
* 容器定高定宽
* 图片水平垂直居中
* 图片若长大于宽 则水平方向100%；否则垂直方向100%
```js
HTML:
  <body>
    <div class="box">
      <img src="http://via.placeholder.com/100x320"/>
    </div>
  </body>
CSS:
*{margin:0;padding:0;}
.box{width:300px;height:300px;background:#000;position:fixed;left:50%;top:50%;transform:translate(-50%, -50%);}
.box{line-height: 300px;text-align:center;font-size:0;}
.box img{max-width: 100%;max-height: 100%;vertical-align: middle;}
```
#### 效果预览
![preview](https://github.com/arch-leo/primary/blob/master/images/5.jpg)


