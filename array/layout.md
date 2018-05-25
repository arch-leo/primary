## 常见布局问题
>1.需求  
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
  <style>
    *{margin:0;padding:0;}
    .box{margin:0 10px;height:0;padding-bottom:calc(50% - 10px);background:#f54343;position:relative;}
    .box div{position:absolute;left:0;top:0;bottom:0;right:0;display:flex;justify-content:center;align-items:center;}
  </style>
```
![preview](https://github.com/arch-leo/primary/blob/master/images/1.jpg)
