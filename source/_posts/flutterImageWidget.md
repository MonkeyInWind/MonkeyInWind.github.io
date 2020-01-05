---
title: 图片Image
date: 2020-01-05 18:49:05
tags:
---
先看一下constructor
<!--more-->
```
Image({
  Key key,
  @required ImageProvider image,                        //抽象的构造函数，提供图片加载入口
  String semanticLabel,                                  //语义标签
  bool excludeFromSemantics: false,                    //看文档好像是语义化
  double width,                                           //宽
  double height,                                         //高            
  Color color,                                            //混合色值
  BlendMode colorBlendMode,                              //混合模式
  BoxFit fit,                                           //填充模式
  AlignmentGeometry alignment: Alignment.center,          //对齐方式
  ImageRepeat repeat: ImageRepeat.noRepeat,              //重复方式
  Rect centerSlice,                                      //图片拉伸
  bool matchTextDirection: false,                        //是否按书写方向绘制图片
  bool gaplessPlayback: false,                          //图片路径发生改变后，加载新图片过程中是否显示旧图
  FilterQuality filterQuality: FilterQuality.low        //看官网说貌似和图片质量有关系
});
```
## 一、加载一张图片
flutter提供了4中图片的加载方式
### 1、本地图片
首先新建一个`images`的文件夹，随便放一张图片进去，我这里在官网下载了flutter的logo
![image.png](1.png)

打开`pubspec.yaml`在`flutter`下添加`assets`
```
flutter:
  uses-material-design: true
  assets:
    - images/logo.png
```
接下来上代码
```
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Demo',
      home: Scaffold(
        body: new Center(
          child: new Image(
            image: AssetImage("images/logo.png")
          )
        )
      )
    );
  }
}
```
这样一张本地图片就加载完成了。如果有2倍图3倍图需要在images文件夹下建`2.0x/3.0x`文件夹。  
flutter还提供了简写的方式
```
body: new Center(
  child: new Image.asset('images/logo.png')
)
```
是不是有问题，一张两张图片还可以，静态文件多了都要添加在`pubspec.yaml`是不是很麻烦，fultter支持只写文件夹路径，注意`/`不要忘了。
```
flutter:
  uses-material-design: true
  assets:
    - images/
```
### 2、网络图片
本地图片需要添加到`pubspec.yaml`网络图片直接按上边的方法写是肯定不行的，需要用`NetworkImage `方法。
```
body: new Center(
  child: new Image(
    image: NetworkImage('https://cdn.jsdelivr.net/gh/flutterchina/website@1.0/images/flutter-mark-square-100.png')
  )
)
```
对于网络图片flutter同样提供了简写
```
body: new Center(
  child: new Image.network('https://cdn.jsdelivr.net/gh/flutterchina/website@1.0/images/flutter-mark-square-100.png')
)
```
### 3、FileImage

### 4、Image.momery

## 二、设置样式/属性
### 1、semanticLabel & excludeFromSemantics
`semanticLabel`看文档应该是和html里img标签的alt属性类似。  
`excludeFromSemantics`如果为true，则`semanticLabel`被忽略。
### 2、width & height
```
body: new Center(
  child: new Image(
    image: new AssetImage('images/logo.png'),
    width: 50.0,
    height: 50.0
  )
)
```
这里需要注意的是，图片的宽高是等比缩放，并且图片显示不会超过原图的尺寸，下面解释一下。  
比如一张尺寸为`200.0*200.0`的图片。  
1、单方向设置尺寸，例：设置`width`为`100.0`，则`height`等比例调整为`100.0`。  
2、如果同时设置宽高，设置的尺寸小于等于图片尺寸，但是与原图比例不同：`width: 100.0, height: 200.0`，则`image`这个节点为设置的尺寸，但是图片显示为宽`100.0`高`100.0`，居中显示。  
3、如果同时设置尺寸，并且两个尺寸都大于图片的实际尺寸，例：`width: 300.0, height: 300.0`，则`image`这个节点为设置的尺寸即`300.0*300.0`，图片显示为图片本身的尺寸即`200.0*200.0`居中显示。
### 3、color
```
color: Colors.red
```
如果和我一样用了flutter官网的logo，会发现图片变成了红色，这个是设置图片的前景色，会覆盖图片，如果不是背景色透明的图片，会把整张图片覆盖。
### 4、colorBlendMode
混合模式，这里先不说单开一篇分析一下混合模式。
### 5、fit
上边说`width&height`的时候，如果设置的宽高大于图片本身的尺寸，图片会以本身的尺寸居中显示，如果想让他以设置的尺寸显示，就需要这个fit。
```
new Image(
  image: new AssetImage('images/logo.png'),
  width: 300.0,
  height: 300.0,
  fit: BoxFit.fill
)
```
`fill`：设置多少就是多少，图片会被拉伸。  
`contain`: 缩放图片以完全装图片，可能有部分空白。  
`cover`: 缩放图片以完全覆盖图片区域，图片可能有部分看不见。  
`fitHeight`: 充满高，可能有部分图片无法显示。  
`fitWidth`：充满宽，可能有部分图片无法显示。  
`scaleDown`：在不大于原图尺寸的情况下，与`contain`效果相同，如果超过原图尺寸，则以原图大小居中显示。  
经过总结发现这个`fit`有一些属性和css中`background-size`效果相同，比如`cover/contain`。
### 6、alignment
对齐方式
```
child: new Image(
  image: new AssetImage('images/logo.png'),
  width: 100.0,
  height: 100.0,
  alignment: Alignment.center,
)
```
这里不详细说了，和`Container`的`alignment`写法效果一样，`需要注意的是，image的alignment作用在自己身上，Container的alignment作用在子节点身上`。
### 7、repeat
图片的重复方式
```
body: new Center(
  child: new Image(
    image: new AssetImage('images/logo.png'),
    width: 400.0,
    height: 400.0,
    repeat: ImageRepeat.repeat,
  )
)
```
`repeat`：重复。  
`repeatX`：X方向重复。  
`repeatY`：Y方向重复。  
`noRepeat`：不重复，默认值。
### 8、centerSlice
当图片需要被拉伸时，`centerSlice`定义了一个矩形区域，这个矩形区域有9个点，拉伸作用在这9个点上。上、下、左、右、左上、右上、左下、右下、正中心。
### 9、matchTextDirection
是否按书写方向绘制，据说需要配合`Directionality`使用，但是`Image`并没有`Directionality`，如果说`Directionality`要加在父节点上，`Container`也没有，没整明白怎么用。
### 10、gaplessPlayback
当图片路径发生改变时，重新加载图片过程中原图是否保留展示。  
`true`：保留。  
`false`：不保留，空白等待新图片加载完成。
### 11、filterQuality
貌似是图片质量相关，加了之后没看出来有什么效果。

`Image`就到这里了，有一些属性和css里的`background`效果基本相同，可以对比来看，还有不知道怎么用的，有大佬知道希望告知。
