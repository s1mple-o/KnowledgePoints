<h1 align = "center">canvas</h1>

## 1. canvas是什么？

h5新增canvas标签，是一张画布

新增canvas 2d 绘图功能



### 1.1 canvas 上下文对象

是canvas 的画笔

如何获取？

```javascript
canvas.getContexe("2d")
//三维
canvas.getContexe("webgl")
```



### 1.2 如何画画

颜色  

形状

绘图方法

### 1.3 canvas 尺寸有极限

canvas尺寸不应太大

## 2. canvas绘图方法

### 2.1坐标系和栅格

![./images/p1.png](C:\Users\10465\Desktop\学习\canvas\images\p1.png)

### 2.2 绘图方法

```javascript
//填充矩形
ctx.fillRect(x,y,w,h)
//描边矩形
ctx.strokeRect(x,y,w,h)
//清理矩形
ctx.clearRext(x,y,w,h)
```

![./images/p2.png](C:\Users\10465\Desktop\学习\canvas\images\p2.png)

 



### 2.3 绘制路径

   1.创建路径 beginPath();

2. 向路径集合中添加子路径
3. 显示路径: 填充 fill() , 描边 stroke()



```javascript
//创建路径
ctx.beginPath();

//添加子路径
ctx.moveTo(50,50); //起点
ctx.lineTo(400,50); //形状
//closePath() 可选 连接起点和终点

//显示路径
ctx.stroke();
```

![](C:\Users\10465\Desktop\学习\canvas\images\p3.png)

#### 2.3.1 子路径形状

直线：lineTo(x,y); 

圆弧：arc(x,y,半径，开始弧度，结束弧度，方向)

切线圆弧：arcTo(x1,y1,x2,y2,半径)

二次贝塞尔曲线：quadraticCurverTo(cpx1,cpy1,x,y)

三次贝塞尔曲线：bezierCurverTo(cpx1,cpy1,cpx2,cpy2,x,y)

矩形：rect(x,y,w,h)



## 3. canvas图形样式

### 3.1 着色

```javascript
//纯色
ctx.fillStyle = 'red'|'#333'|'rgba(0,0,0)'; //填充颜色

//渐变色

gradient = createLinearGradient(x1,y1,x2,y2); //线性渐变
createLinearGradient(x1,y1,r1,x2,y2,r2); //径向渐变 


//创建渐变对象
const linerGradient = ctx.createLinearGradient(x1,y1,x2,y2);

//渐变颜色节点
linerGrandient.addColorStop(0,'red');

//赋值
ctx.fillStyle = linerGradient;

```

### 3.2 纹理

```javascript
//创建纹理对象
const image = new() Image();
img.src = 'dir';
img.onload = loadedFn;
function loadedFn(){
    let pattern = context.createPattern(image,'repeat|repeat-x|repeat-y|no-repeat');

	//赋值
	ctx.fillStyle|strokeStyle = pattern;
}

```

### 3.3 影响描边样式的因素

strokeStye : 描边颜色

lineWidth: 描边宽

lineCap ：描边端点样式

![](C:\Users\10465\Desktop\学习\canvas\images\p4.png)

lineJoin ：描边拐角类型

![](C:\Users\10465\Desktop\学习\canvas\images\p5.png)

miterLimit ：拐角最大厚度

![](C:\Users\10465\Desktop\学习\canvas\images\p6.png)

setlineDash(segments) ：将描边设置为虚线

![](C:\Users\10465\Desktop\学习\canvas\images\p7.png)

lineDashOffset ：虚线偏移量 

![](C:\Users\10465\Desktop\学习\canvas\images\p8.png)

### 3.4 投影属性

位置：

​	shadowOffsetX =  float

​	shadowOffsetY =  float

模糊度：

​	shadowBlur = float

颜色：

shadowColor  = color



## 4. 文本

### 4.1 文本属性

字体： font

水平对齐：textAlign

垂直对齐：textBaseLine

![](C:\Users\10465\Desktop\学习\canvas\images\p9.png)

### 4.2 文本绘制方法

填充文字 fillText(text,x,y,maxWidth)

描边文字 strokeText(text,x,y,maxWidth)

lineWidth 描边宽度

measureText(text)  获取文字宽度

 

## 5. 图像

### 5.1 图像操作方法drawImage

绘图 + 位移：drawImage(image,x,y)

绘图 + 位移 + 缩放：drawImage(image,x,y,width,height)

绘图 + 裁剪 + 位移 + 缩放：drawImage(image,x1,y1,w1,x2,y2,w2,h2)

```javascript
//创建图片对象实例
const img = new Image();

//图片地址赋值
img.src = 'dir';

//图片加载成功执行
img.onload = function(){
    ctx.drawImage(img,x,y)
}

```

### 5.2 像素级操作

imageDada 图片的数据化

属性：data  width  heigth

```javascript
//创建ImageData
new ImageData(width,height);
new ImageData(Unit8ClampedArray,width,heigth)

ctx.createImageData(width,heigth)
ctx.createImageData(ImageData)


//获取
ctx.getImageData(x,y,width,heigth)

//显示
ctx.putImageData(ImageData,x,y,dirtyX,dirtyY,dirtyW,dirtyH)
```

灰度算法

lm = 0.299*r+0.587*g + 0.114b

马赛克效果

 

## 6.变换

### 6.1 管理上下文状态方法

保存当前状态：save()

恢复上一次保存状态：restore()

eg：避免当前图形样式影响以后样式



### 6.2 变换操作

eg：对坐标轴的操作

移动：translate(x,y)

旋转：rotate(angle)

缩放：scale(x,y)



矩阵变换

相对矩阵变换：transform(a,b,c,d,e,f)

绝对矩阵变换：setTransform(a,b,c,d,e,f)

   

## 7. 动画 

### 7.1 步骤

清空 canvas （clearRect）

保存canvas状态

绘制动画图形

恢复canvas状态



### 7.2 驱动动画方法

setTimeOut(fn,time)

setInteral(fun,time)

requestAnimationFranme(fn)

  

### 7.3 补间动画

tween.js

d3-color.js



### 7.4 图形交互

canvas无监听事件，只能用鼠标位置和图像位置进行对比判断

```javascript
//获取鼠标位置

canvas.addEventListener('mousedown',function(event){
    //鼠标在窗口位置
    const {clientX,clientY} = event;
    //canvas相对于窗口边界
    const {left,top} = canvas.getBoundingClientRect();
    
    //鼠标在canvas中的位置
    const [x,y] =[clientX-left,clientY-top]
})

//获取触摸点方法

canvas.addEventListener('mousedown',function(event){
    //鼠标在窗口位置
    const {pageX,pageY} = event.changedTouches[0];
    //canvas相对于窗口边界
    const {left,top} = canvas.getBoundingClientRect();
    
    //鼠标在canvas中的位置
    const [x,y] =[clientX-left,clientY-top]
})
```



## 8. 合成

### 8.1 透明度合成 globalAlpha

globalAlpha 是全局对象透明度，全局对象就是canvas的上下文对象

使用方法：ctx.globalAlpha = 0.6;

### 8.2 路径裁剪clip()

路径裁剪是在画布上设置一个路径，之后绘制的图像只显示在这个路径之中

步骤：

1.定义路径

2.ctx.clip()

3.绘制其他图形

### 8.3 全局合成 globalCompositeOperation

![](C:\Users\10465\Desktop\学习\canvas\images\p10.png)

​	