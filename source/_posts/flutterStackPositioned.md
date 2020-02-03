---
title: flutter--Stack & Positioned & Align 层叠布局与定位
date: 2020-02-03 20:06:01
tags:
---
`Stack`在[flutter--模拟登录](/post/flutterDemoMockLogin)中出现过，这里来详细了解一下这个`Widget`，类似于`css`中的`position: relative`，但是会强制子`Widget`层叠显示，`Positioned`类似于`css`中的`position: absolute`，可以设置坐标。
<!--more-->
## Stack
```
Stack({
    Key key,
    AlignmentGeometry alignment: AlignmentDirectional.topStart,
    TextDirection textDirection,
    StackFit fit: StackFit.loose,
    Overflow overflow: Overflow.clip,
    List<Widget> children: const[]
})
```
先看一个只有`children`的demo
```
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return new MaterialApp(
      home: Scaffold(
        body: Stack(
          children: <Widget>[
            Text('1'),
            Text('2'),
            Text('3')
          ],
        )
      )
    );
  }
}
```

![](1.png)
1、2、3叠在了一起，哪一个在上哪一个在下，把`Text`换成`Container`再看一下，
```
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return new MaterialApp(
      home: Scaffold(
        body: Stack(
          children: <Widget>[
            Container(
              width: 100,
              height: 100,
              color: Colors.red
            ),
            Container(
              width: 50,
              height: 50,
              color: Colors.green
            )
          ],
        )
      )
    );
  }
}
```

![](2.png)
可以看见和`css`里一样，后写的在上。  
### alignment
子`Widget`的对齐方式  
`AlignmentDirectional.topStart` (默认值)、`AlignmentDirectional.topCenter`、`AlignmentDirectional.topEnd`、`AlignmentDirectional.centerStart`、`AlignmentDirectional.center`、`AlignmentDirectional.centerEnd`、`AlignmentDirectional.bottomStart`、`AlignmentDirectional.bottomCenter`、`AlignmentDirectional.bottomEnd`。  
简单易懂，看个demo。
```
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return new MaterialApp(
      home: Scaffold(
        body: Stack(
          alignment: AlignmentDirectional.bottomCenter,
          children: <Widget>[
            Container(
              width: 100,
              height: 100,
              color: Colors.red
            ),
            Container(
              width: 50,
              height: 50,
              color: Colors.green
            ),
            Container(
              width: 30,
              height: 30,
              color: Colors.blue
            )
          ],
        )
      )
    );
  }
}
```

![](3.png)
### textDirection
子`Widget`的排列方式，默认值是`TextDirection.ltr`，从左往右，当设置为`TextDirection.rtl`时效果如下：

![](4.png)
### fit
子`Widget`中未定位元素的大小，只有两个值：  
`StackFit.loose`：不对其大小进行约束(默认值)  
`StackFit.expand`：最大  
`StackFit.passthrough`：父级的约束直接传递给子`Widgert`
```
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return new MaterialApp(
      home: Scaffold(
        body: Container(
          width: 200,
          height: 200,
          child: Stack(
            fit: StackFit.passthrough,
            children: <Widget>[
              Container(
                width: 50,
                height: 50,
                color: Colors.red
              )
            ],
          )
        )
      )
    );
  }
}
```

![](5.png)
这里需要注意的是`fit`值如果不是默认值`StackFit.loose`，子`Widget`设置的尺寸将失去作用。
### overflow
和`css`里效果一样，只有两个值  
`Overflow.clip`：溢出将被剪切  
`Overflow.visible`：不对溢出的部分做处理
## Positioned
定位元素，用于`Stack`的子`Widget`
```
Positioned({
    Key key,
    double left,
    double top,
    double right,
    double bottom,
    double width,
    double height,
    @required Widget child
})
```
都是基础属性，`left`、`top`、`right`、`bottom`是相对于父级的坐标。
```
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return new MaterialApp(
      home: Scaffold(
        body: Stack(
          children: <Widget>[
            Positioned(
              width: 100,
              height: 100,
              left: 100,
              bottom: 100,
              child: Container(
                color: Colors.red
              )
            )
          ],
        )
      )
    );
  }
}
```

![](6.png)
相对于左下角的距离是100、100.
## Align
这个更简单一点，只是设定子`Widget`相对于`Align`的位置。
```
Align({
    Key key,
    AlignmentGeometry alignment: Alignment.center,
    double widthFactor,
    double heightFactor,
    Widget child
})
```
### alignment
子`Widget`相对于`Align`的位置。
### widthFactor & heightFactor
这两个相当于是系数，乘以子`Widget`的宽高，就是`Align`的尺寸。  
```
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return new MaterialApp(
      home: Scaffold(
        body: Align(
          widthFactor: 2,
          heightFactor: 2,
          alignment: Alignment.bottomRight,
          child: Container(
            width: 50,
            height: 50,
            color: Colors.blue
          )
        )
      )
    );
  }
}
```

![](7.png)
这里需要注意一点，就是如果`Align`的父级设有宽高`widthFactor`和`heightFactor`将市区作用，大小为最大。
```
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return new MaterialApp(
      home: Scaffold(
        body: Container(
          width: 300,
          height: 300,
          color: Colors.red,
          child: Align(
            widthFactor: 2,
            heightFactor: 2,
            alignment: Alignment.bottomRight,
            child: Container(
              width: 50,
              height: 50,
              color: Colors.blue
            )
          )
        )
      )
    );
  }
}
```

![](8.png)
