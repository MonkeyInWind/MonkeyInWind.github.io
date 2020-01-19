---
title: flutter--容器组件Container
date: 2020-01-05 18:46:12
tags:
---
上一篇笔记用了个`Container`组件，这一篇详细介绍一下，整体来说这个组件类似`div`，但是与`div`又不同，具体有哪些特性，先了解一下`Container`有哪些属性再总结，因为不了解有哪些属性直接看的话，会有点懵逼。
<!--more-->
```
Container({
  Key key,
  AlignmentGeometry alignment,        //子组件的对齐方式
  EdgelnsetsGeometry padding,          //内边距
  Color color,                          //背景色
  Decoration decoration,                //背景装饰  相当于样式  在child之下
  Decoration foregroundDecoration,      //前景装饰  覆盖在child之上  如半透明遮罩效果
  double width,                         //宽
  double height,                         //高
  BoxConstraints constraints,           //容器的大小限制
  EdgelnsetsGeometry margin,            //外边距
  Matrix4 transform,                     //变换
  Widget child                            //子节点
})
```
## 1、alignment
子组件的对齐方式，`有子组件并且当前Container尺寸大于子组件尺寸时有效`。
```
new Container(
  color: Colors.red,
  width: 400.0,
  height: 400.0,
  alignment: Alignment.center,
  child: new Container(
    color: Colors.blue,
    width: 50.0,
    height: 50.0,
  )
)
```
`center`：居中  
`centerLeft`：靠左垂直居中  
`centerRight`：靠右垂直居中  
`bottomCenter`：底部居中  
`bottomLeft`：左下  
`bottomRight`：右下  
`topCenter`：顶部居中  
`topLeft`：左上  
`topRight`：右上  
可以看见除了`center`，都是两个单词拼在一起很好记，垂直方向在前，水平方向在后。  
子组件尺寸要小于当前组件尺寸时才会生效。
## 2、padding & margin
这两个放一起，设置方式相同，这里以padding为例。
```
new Container(
  padding: EdgeInsets.all(10.0)
)
```
`EdgeInsets.all`：四个内边距值相同  
`EdgeInsets.only`：四个边距单独设置，不设置则为0，用法如下
```
padding: EdgeInsets.only(
  top: 10.0,
  left: 20.0
)
```
可以只写一个，也可以四个全都写。  
`EdgeInsets.symmetric`：可以设置同方向上的两个内边距
```
padding: EdgeInsets.symmetric(
  horizontal: 10.0,        //水平方向两个边距
  vertical: 20.0          //垂直方向两个边距
)
```
`EdgeInsets.zero`：设置内边距为0  
`EdgeInsets.fromLTRB`：`only`的简写，顺序为左上右下
```
padding: EdgeInsets.fromLTRB(10.0, 20.0, 30.0, 40.0)
```
`EdgeInsets.fromWindowPadding`：具体用法不知道  
`***EdgeInsets这个组件是个公用组件，并不是只有padding可以用，比如margin也可以`
## 3、color
背景色。
```
new Container(
  color: Colors.red
)
```
## 4、decoration
`***decoration不能和color同时用，会报错。`
```
const BoxDecoration({
  Color color,
  DecorationImage image,
  BoxBorder border,
  BorderRadiusGeometry borderRadius,
  List<BoxShadow> boxShadow,
  Gradient gradient,
  BlendMode backgroundBlendMode,
  BoxShape shape: BoxShape.rectangle
})
```
#### color
和color一样，没啥说的。
#### image
背景图，可以和背景色一起使用。
```
decoration: BoxDecoration(
  image: DecorationImage(
  //这里先用网图，怎么加载本地资源以后再细说
    image: new NetworkImage('https://cdn.jsdelivr.net/gh/flutterchina/website@1.0/images/flutter-mark-square-100.png')
  )
)
```
#### border
边框，在container内部，和`margin/padding`类似，可以单独设置也可以设置为统一样式。
```
decoration: BoxDecoration(
  //border: Border.all(width: 2.0, color: Colors.blue)          设置为相同样式
  top: BorderSide(width: 2.0, color: Colors.blue),
  left: BorderSide(width: 4.0, color: Colors.yellow),
  right: BorderSide(width: 8.0, color: Colors.green),
  bottom: BorderSide(width: 16.0, color: Colors.purple)
)
```
#### borderRadius
`*圆角不能和border同时设置，同时存在时可以运行但是会抛出一个异常`
```
decoration: new BoxDecoration(
  borderRadius: BorderRadius.only(                   //only对每个角单独设置
    bottomLeft: Radius.circular(20.0),                  //圆角值为20.0
    topLeft: Radius.zero,                                      //圆角为0
    bottomRight: Radius.elliptical(20.0, 40.0)       //圆角x方向为20.0，y方向为40.0
  )
)
```
`Radius.all`：四个角同时设置，参数和但方向的参数相同（`borderRadius: new BorderRadius.all(Radius.circular(20.0))`）。  
`Radius.lerp`: 做动画用的多一点暂时先不深入。
#### boxShadow
容器阴影
```
decoration: new BoxDecoration(
  boxShadow: [
    new BoxShadow(
      color: Colors.red,                      //阴影颜色
      offset: Offset(10.0, 20.0),          //偏移量x, y
      blurRadius: 10.0,                      //模糊
      spreadRadius: 10.0                  //延伸
    ),
  ]
)
```
#### gradient
背景色渐变
```
 //径向渐变
decoration: BoxDecoration(
  gradient: RadialGradient(                         
    colors: [Colors.red, Colors.blue],            //渐变颜色
    center: Alignment.topLeft,                      //渐变中心点，可以设具体数值（center: const Alignment(0.7, -0.6)）
    radius: 5.0                                              //渐变半径
    stops: [0.4, 1.0],                                    //渐变颜色的比例
  ),
)

//线性渐变
gradient: LinearGradient(
  begin: Alignment.topLeft,                            //渐变起始点
  end: Alignment(0.1, 0.1),                            //结束点
  colors: [Colors.red, Colors.blue],              
  tileMode: TileMode.mirror                          //模式有三个值（mirror：镜像，clamp: 单纯的单次渐变不做其他处理，repeated: 重复）
)

//扇形渐变
gradient: SweepGradient(
  center: FractionalOffset.center,    //渐变的圆心
  startAngle: 0.0,                      //渐变的起始角度
  endAngle: math.pi * 2,          //渐变的角度范围
  colors: const <Color>[
    Colors.blue,
    Colors.green,
    Colors.red,
    Colors.yellow,
    Colors.pink
  ],
  stops: const <double>[0.0, 0.25, 0.5, 0.75, 1.0],      //每种颜色所占的比例，总数为2，多出部分被初始颜色覆盖
)
```
扇形渐变每种颜色的基准线为改颜色的中心线（不理解的自己试一下就知道了）。  
`tileMode`是这三种渐变都有的属性，不仅限于线性渐变。  
径向渐变、扇形渐变的圆心位置和线性渐变的起始/结束坐标都是可以设置具体数值的。
#### backgroundBlendMode
图像混合模式，这个东西有点多，要单开一篇。
#### shape
当前Container的形状
```
decoration: BoxDecoration(
  shape: BoxShape.rectangle
)
```
有两个值  
`rectangle`：矩形  
`circle`：圆形，不会出现椭圆的情况，宽高不同时直径按小的那个值算。

## 5、width & height
这俩放一起，`double`类型，宽高。
## 6、constraints
对Container的尺寸约束
```
constraints: new BoxConstraints(
  minWidth: 200.0,
  maxWidth: 300.0,
  minHeight: 200.0,
  maxHeight: 300.0
)
```
## 7、transform
对Container进行变换操作，和css的transform类似，值是一个矩阵(Matrix4)。  
这里暂时跳过，开一篇单独说。
## 8、child
子节点，只能有一个子节点，没有children。

## 9、关于`Container`的特性
#### 关于尺寸的自我调节
1、如果设置了`width/height/constraints`，并且尺寸小于父组件的尺寸，则为设置的宽高，如果大于父组件的尺寸则为父组件的宽高，如果父组件有`padding`，或当前组件有`margin`需要减去`padding/margin`。  
2、如果没有子节点并且没有`width/height`以及`constraints `约束，Container的尺寸会撑到最大，尽可能占满父节点。  
2、如果有子节点并且没有`width/height`以及`constraints `约束，Container会尽量缩小。
#### 关于渲染过程
生成节点：
`padding > decoration > constraints (width/height) > margin `  
绘制节点：
`transform > decoration > foregroundDecoration `

`Container`差不多就这些东西，不是太深入，深入的东西就需要自己在项目或者demo中尝试了，还有一些即使是粗略的说也不少，放这里感觉不太合适，后边单独分析。
