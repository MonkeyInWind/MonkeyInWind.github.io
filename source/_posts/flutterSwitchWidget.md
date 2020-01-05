---
title: 单选开关Switch、SwitchListTile
date: 2020-01-05 19:37:21
tags:
---
## 一、Switch
先看一下`Switch`的`constructor`
<!--more-->
```
const Switch({
  Key key,
  @required bool value,
  @required ValueChanged<bool> onChanged,
  Color activeColor,
  Color activeTrackColor,
  Color inactiveThumbColor,
  Color inactiveTrackColor,
  ImageProvider activeThumbImage,
  ImageProvider inactiveThumbImage,
  MaterialTapTargetSize materialTapTargetSize,
  DragStartBehavior dragStartBehavior: DragStartBehavior.start
})
```
怎么用看demo
```
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return new MaterialApp(
      home: Scaffold(
        body: SwitchDemo()
      )
    );
  }
}

class SwitchDemo extends StatefulWidget {
  @override
  _SwitchDemoState createState() => new _SwitchDemoState();
}

class _SwitchDemoState extends State<SwitchDemo> {
  bool _switchSelected = false;

  @override
  Widget build(BuildContext context) {
    return Container(
      alignment: Alignment.center,
      child: Switch(
        value: _switchSelected,
        onChanged: (value) {
          setState(() {
            _switchSelected = value;
          });
        }
      )
    );
  }
}
```
可以看见模拟器中心有个蓝色的小开关，点击可以改变状态，这是最基础的用法。  
这里需要注意的是`value`只能是`bool`类型，并且写死之后点击开关是没有效果的。
### activeColor & activeTrackColor & inactiveThumbColor & inactiveTrackColor
先看一下这几个颜色，`active`对应开关打开，也就是`value`为`true`的状态，`inactive`对应开关关闭，`value`为`false`的状态。
```
Switch(
  value: _switchSelected,
  activeColor: Colors.red,
  activeTrackColor: Colors.yellow,
  inactiveThumbColor: Colors.green,
  inactiveTrackColor: Colors.purple,
  onChanged: (value) {
  setState(() {
    _switchSelected = value;
  });
  }
)
```
![image.png](1.png)
![image.png](2.png)
### activeThumbImage & inactiveThumbImage
这两个放一起，开关圆点的图片，和颜色一样`active、inactive`对应开关的两种状态。
```
Switch(
  value: _switchSelected,
  activeThumbImage: AssetImage('./images/logo.png'),
  inactiveThumbImage: AssetImage('./images/logo.png'),
  onChanged: (value) {
    setState(() {
      _switchSelected = value;
    });
  }
)
```
同一张图片，为啥不放两个不同的图片，因为我懒。
![image.png](3.png)
![image.png](4.png)
### materialTapTargetSize
有效点击区域的大小，在[按钮 各种Button](/post/flutterButtonWidget)介绍过
### dragStartBehavior
这个需要注意一下，直接写会报错`undefind`，需要`import 'package:flutter/gestures.dart';`。  
看一下完整demo
```
import 'package:flutter/material.dart';
import 'package:flutter/gestures.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return new MaterialApp(
      home: Scaffold(
        body: SwitchDemo()
      )
    );
  }
}

class SwitchDemo extends StatefulWidget {
  @override
  _SwitchDemoState createState() => new _SwitchDemoState();
}

class _SwitchDemoState extends State<SwitchDemo> {
  bool _switchSelected = false;

  @override
  Widget build(BuildContext context) {
    return Container(
      alignment: Alignment.center,
      child: Switch(
        value: _switchSelected,
        dragStartBehavior: DragStartBehavior.down,
        onChanged: (value) {
          setState(() {
            _switchSelected = value;
          });
        }
      )
    );
  }
}
```
可以设置成`start`或者`down`。  
源码里边对这个的解释是设置为`start`的时候拖拽会在开始拖动时开始触发，设置为`down`则在手指按下时开始触发，区别是`start`动画更平滑，`down`反应更灵敏。  
试了一下，`start`和`down`并没有看出有什么区别。。。

## 二、SwitchListTile
先看一下`constructor`
```
const Switch({
  Key key,
  @required bool value,
  @required ValueChanged<bool> onChanged,
  Color activeColor,
  Color activeTrackColor,
  Color inactiveThumbColor,
  Color inactiveTrackColor,
  ImageProvider activeThumbImage,
  ImageProvider inactiveThumbImage,
  Widget title,
  Widget subtitle,  
  bool isThreeLine: false,
  bool dense,   
  Widget secondary,  
  bool selected: false,
})
```
`SwitchListTile`一部分和`Switch`是重合的，另一部分和`CheckboxList`是重合的（忘了的话看这里：[复选框CheckBox、CheckboxListTile](/post/flutterCheckWidget)
）。。。这里就不重复了。
