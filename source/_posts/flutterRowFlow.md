---
title: flutter--Wrap & Flow流式布局
date: 2020-02-02 19:18:49
tags:
---
在[flutter--Row & Column线性布局、Flex & Expanded弹性布局](/post/flutterRowColumnFlex)介绍了线性布局和弹性布局，这两种布局有时候并不能满足我们的需求，比如子`Widgert`的尺寸超出父`Widget`的时候会报错，可能我们需要的是自动排在下一行，这一篇来就看一下流式布局。
<!--more-->
## Wrap
```
Wrap({
    Key key,
    Axis direction: Axis.horizontal,
    WrapAlignment alignment: WrapAlignment.start,
    double spacing: 0.0,
    WrapAlignment runAlignment: WrapAlignment.start,
    double runSpacing: 0.0,
    WrapCrossAlignment crossAxisAlignment: WrapCrossAlignment.start,
    TextDirection textDirection,
    VerticalDirection verticalDirection: VerticalDirection.down,
    List<Widget> children: const[]
})
```
### direction
排列方向  
`Axis.horizontal`：横向排列（默认值）  
`Axis.vertical`：纵向排列
### alignment
这个和`Row`的`mainAxisAlignment`效果是一样的，这里就不重复了，看[flutter--Row & Column线性布局、Flex & Expanded弹性布局](/post/flutterRowColumnFlex)  
有一点需要注意的就是`Wrap`的尺寸，横向只会被子`Widget`撑开，纵向如果父组件有高度则是父组件的高度，这里看一下demo。
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
          child: Wrap(
            alignment: WrapAlignment.spaceAround,
            children: <Widget>[
              Container(
                width: 100,
                height: 50,
                color: Colors.red,
              ),
              Container(
                width: 100,
                height: 50,
                color: Colors.blue
              ),
              Container(
                width: 100,
                height: 50,
                color: Colors.red,
              ),
              Container(
                  width: 80,
                  height: 50,
                  color: Colors.blue
              ),
              Container(
                width: 50,
                height: 50,
                color: Colors.red,
              ),
              Container(
                  width: 70,
                  height: 50,
                  color: Colors.blue
              )
            ],
          )
        )
      )
    );
  }
}
```

![](1.png)
### spacing
子`Widget`之间的间距
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
          child: Wrap(
            spacing: 10,
            children: <Widget>[
              Container(
                width: 100,
                height: 50,
                color: Colors.red,
              ),
              Container(
                width: 100,
                height: 50,
                color: Colors.blue
              ),
              Container(
                width: 100,
                height: 50,
                color: Colors.red,
              )
            ],
          )
        )
      )
    );
  }
}
```

![](2.png)
### runAlignment
一行子`Widget`纵向的对齐方式，属性值与`alignment`相同。  
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
          height: 300,
          child: Wrap(
            runAlignment: WrapAlignment.spaceBetween,
            children: <Widget>[
              Container(
                width: 100,
                height: 50,
                color: Colors.red,
              ),
              Container(
                width: 100,
                height: 50,
                color: Colors.blue
              ),
              Container(
                width: 100,
                height: 50,
                color: Colors.green,
              ),
              Container(
                  width: 80,
                  height: 50,
                  color: Colors.yellow
              ),
              Container(
                width: 50,
                height: 50,
                color: Colors.pinkAccent,
              ),
              Container(
                width: 70,
                height: 50,
                color: Colors.deepPurple
              )
            ],
          )
        )
      )
    );
  }
}
```

![](3.png)
### runSpacing
每一行子`Widget`纵向上的间距
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
          child: Wrap(
            runSpacing: 10,
            children: <Widget>[
              Container(
                width: 100,
                height: 50,
                color: Colors.red,
              ),
              Container(
                width: 100,
                height: 50,
                color: Colors.blue
              ),
              Container(
                width: 100,
                height: 50,
                color: Colors.green,
              ),
              Container(
                  width: 80,
                  height: 50,
                  color: Colors.yellow
              ),
              Container(
                width: 50,
                height: 50,
                color: Colors.pinkAccent,
              ),
              Container(
                width: 70,
                height: 50,
                color: Colors.deepPurple
              )
            ],
          )
        )
      )
    );
  }
}
```

![](4.png)
### crossAxisAlignment
子`Widget`在纵向上的对齐方式，看着好像和`runAlignment`一样，看demo。
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
          height: 300,
          child: Wrap(
            crossAxisAlignment: WrapCrossAlignment.center,
            children: <Widget>[
              Container(
                width: 100,
                height: 100,
                color: Colors.red,
              ),
              Container(
                width: 100,
                height: 50,
                color: Colors.blue
              ),
              Container(
                width: 100,
                height: 50,
                color: Colors.green,
              ),
              Container(
                  width: 80,
                  height: 50,
                  color: Colors.yellow
              ),
              Container(
                width: 50,
                height: 100,
                color: Colors.pinkAccent,
              ),
              Container(
                width: 70,
                height: 50,
                color: Colors.deepPurple
              ),
            ],
          )
        )
      )
    );
  }
}
```

![](5.png)
这个属性是对每一行内的`Widget`进行对齐操作，而`runAlignment`是以行为单位进行对齐操作。

### textDirection
子`Widget`横向排列方式，从左往右还是从右往左，不了解的话看这里[flutter--Row & Column线性布局、Flex & Expanded弹性布局](/post/flutterRowColumnFlex)。
### verticalDirection
子`Widget`纵向排列方式
## Flow
```
Flow({
    Key: key,
    @required FlowDelegate delegate,
    List<Widget> children: const[]
})
```
`Flow`的属性只有两个，都是必须的，貌似很简单，其实很复杂，`children`没啥说的，主要是`delegate`，属性值是一个函数，利用矩阵变换设置坐标来控制每一个`Widget`的位置。  
大多数情况下`Wrap`已经满足需求，关于`Flow`就不深入来，有兴趣的话可以看[官方的文档](https://api.flutter.dev/flutter/widgets/Flow-class.html)。
