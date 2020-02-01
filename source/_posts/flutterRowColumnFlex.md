---
title: flutter--Row & Column线性布局、Flex & Expanded弹性布局
date: 2020-02-01 21:58:23
tags:
---
## Row & Column
这两个属性都一样，用法也一样，一个横向一个竖向，放一起，这里以`Row`为例。
<!--more-->
```
Row({
    Key key,
    MainAxisAlignment mainAxisAlignment: MainAxisAlignment.start,  //对齐方式
    MainAxisSize mainAxisSize: MainAxisSize.max,    //主轴方向占用的空间
    CrossAxisAlignment crossAxisAlignment: CrossAxisAlignment.center,   //交叉轴上的对齐方式
    TextDirection textDirection,    //主轴方向上的排列顺序
    VerticalDirection verticalDirection: VerticalDirection.down,    交叉轴上排列的开始和结束
    TextBaseline textBaseline,  //文本基线
    List<>Widget children: const []     //子组件
})
```
### mainAxisAlignment
子组件在主轴上的对齐方式。  
`MainAxisAlignment.start`：正序  
`MainAxisAlignment.end`：反序  
`MainAxisAlignment.center`：居中  
`MainAxisAlignment.spaceAround`：分散对齐，第一个组件和最后一个组件和父组件之间存在间距，为子组件之间间距的一半  
`MainAxisAlignment.spaceBetween`：分散对齐，第一个和最后一个子组件和父组件之间没有间距  
`MainAxisAlignment.spaceEvenly`：分散对齐，子组件以及父组件之间的间距相等
### mainAxisSize
`Row`在主轴方向上所占用的空间，`MainAxisSize.max`在父组件内占用最大空间，`MainAxisSize.min`最小，占用空间为子组件撑开的大小。  
前边两个属性看demo。
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
          child: Row(
            mainAxisAlignment: MainAxisAlignment.end,
            mainAxisSize: MainAxisSize.max,
            children: <Widget>[
              Container(
                width: 50,
                height: 100,
                color: Colors.green
              ),
              Container(
                width: 80,
                height: 50,
                color: Colors.red
              ),
              Container(
                  width: 50,
                  height: 50,
                  color: Colors.pinkAccent
              ),
              Container(
                  width: 50,
                  height: 50,
                  color: Colors.blue
              ),
            ],
          )
        )
      )
    );
  }
}
```

![](1.png)
### crossAxisAlignment
自组件在交叉轴上的对齐方式。  
`CrossAxisAlignment.center`：居中（默认）  
`CrossAxisAlignment.start`：正序   
`CrossAxisAlignment.end`：倒序  
`CrossAxisAlignment.center`：子组件拉伸为父组件的高度  
`CrossAxisAlignment.baseline`：基线对齐，要配合`textBaseline`同时使用  
### textDirection
`TextDirection.ltr`：从左到右（默认值）  
`TextDirection.ltr`：从右到左
### verticalDirection
交叉轴上排列的开始和结束，配合`crossAxisAlignment`使用。  
`VerticalDirection.up`：竖直方向从下往上排列  
`VerticalDirection.down`：竖直方向从上往下排列  
### textBaseline
文本基线，没看出来效果。
### children
子组件。
## Flex
`Row`和`Column`都是继承自`Flex`。
```
Flex({
    Key key,
    @required Axis direction,
    MainAxisAlignment mainAxisAlignment: MainAxisAlignment.start,  //对齐方式
    MainAxisSize mainAxisSize: MainAxisSize.max,    //主轴方向占用的空间
    CrossAxisAlignment crossAxisAlignment: CrossAxisAlignment.center,   //交叉轴上的对齐方式
    TextDirection textDirection,    //主轴方向上的排列顺序
    VerticalDirection verticalDirection: VerticalDirection.down,    交叉轴上排列的开始和结束
    TextBaseline textBaseline,  //文本基线
    List<>Widget children: const []     //子组件
})
```
`Flex`只多了一个`direction`属性。
### direction
排列方式，两个值  
`Axis.horizontal`：水平排列  
`Axis.vertical`：竖直排列
## Expanded
```
Expanded({
    Key key,
    int flex: 1,
    @required Widget child
})
```
这个很简单，只有一个`flex`和`child`。  
`Flex`和`Expanded`与`css`里的`display: flex`、`flex: 1`效果是一样的，看一下demo。
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
          child: Flex(
            direction: Axis.horizontal,
            children: <Widget>[
              Expanded(
                flex: 1,
                child: Container(
                  height: 50,
                  color: Colors.red
                )
              ),
              Expanded(
                flex: 2,
                child: Container(
                  height: 50,
                  color: Colors.blue
                )
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
`flex`属性表示所占的比例。
