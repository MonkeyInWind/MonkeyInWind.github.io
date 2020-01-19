---
title: flutter--复选框CheckBox、CheckboxListTile
date: 2020-01-05 19:27:55
tags:
---
`CheckBox`继承自`StatelessWidget`，是个无状态组件，本身不会保存状态，所以需要一个有状态的父组件用来保存这个状态。
<!--more-->
## 一、CheckBox
先看constructor
```
CheckBox({
  Key key,
  @required bool value,                  //复选框的值
  bool tristate: false,                     //为true时复选框会多一个值为null的状态，复选框内显示为横线
  @required ValueChanged<bool> onChanged,    //点击复选框的回调
  Color activeColor,                      //选中时复选框的颜色
  Color checkColor,                      //选中时对号的颜色
  MaterialTapTargetSize materialTapTargetSize    //有效点击区域的大小
})
```
看demo
```
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return new MaterialApp(
      home: Scaffold(
        body: CheckboxDemo()
      )
    );
  }
}

class CheckboxDemo extends StatefulWidget {
  @override
  _CheckboxDemoState createState() => new _CheckboxDemoState();
}

class _CheckboxDemoState extends State<CheckboxDemo> {
  bool _checkboxSelected = false;

  @override
  Widget build(BuildContext context) {
    return Container(
      alignment: Alignment.center,
      child: Checkbox(
        value: _checkboxSelected,
        activeColor: Colors.red,
        checkColor: Colors.yellow,
        tristate: true,
        onChanged:(value){
          print(value);
          setState(() {
            _checkboxSelected=value;
          });
        } ,
      )
    );
  }
}
```
屏幕正中间一个小方块，就是`Checkbox`。  
这个比较简单，`Color`已经很熟练了，基本上大部分可视组件都有，一个`value`还有个`onChanged`回调。

`tristate`之前没见过，值为`false`的时候，复选框有两种状态，对应两个值`true`和`false`；值为`true`的时候有三种状态，对应三个值`true`、`null`、`false`。

这里用了有状态组件`StatefulWidget`作为父组件，又在父组件里创建了state，状态有子组件管理，在`onChanged`事件里`setState`改变状态，这里和`react`很像。

上边已经用了`value、onChanged`接下来看一下`Switch`的其他属性。  
一个复选框就这么写完了，但是不能就放一个框啊，要有文字，没有文字谁知道这个框选中是要干啥，有可能还需要个图标，后边学了布局用`flex`很容易可以实现。  
有人说我不想每次都重复写这么个布局，虽然不多但是每次都一样写着烦，`flutter`在这里体现出了人性化，提供了一个可以带文字和图标的复选框组件`CheckboxListTile`。
## 二、CheckboxListTile
先来看一下constructor
```
const CheckboxListTitle({
  Key key,
  @required bool value,
  @required ValueChanged<bool> onChanged,
  Color activeColor,
  Widget title,                    //复选框的主标题
  Widget subtitle,                  //复选框的副标题
  bool isThreeLine: false,          //文字是否为三行
  bool dense,                        //是否为垂直密集列表的一部分
  Widget secondary,                //图标
  bool selected: false,              //文字和图标颜色是否为选中的颜色(activeColor)
  ListTileControlAffinity controlAffinity: ListTileControlAffinity.platform    //文字、图标、复选框的排列顺序
});
```
看一下demo
```
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return new MaterialApp(
      home: Scaffold(
        body: CheckboxDemo()
      )
    );
  }
}

class CheckboxDemo extends StatefulWidget {
  @override
  _CheckboxDemoState createState() => new _CheckboxDemoState();
}

class _CheckboxDemoState extends State<CheckboxDemo> {
  bool _checkboxSelected = false;

  @override
  Widget build(BuildContext context) {
    return Center(
      child: CheckboxListTile(
        value: _checkboxSelected,
        title: Text('this is title'),
        subtitle: Text('this is subtitle'),
        activeColor: Colors.red,
        dense: true,
        selected: true,
        isThreeLine: false,
        secondary: Icon(Icons.book),
        controlAffinity: ListTileControlAffinity.leading,
        onChanged:(value){
          print(value);
          setState(() {
            _checkboxSelected=value;
          });
        } ,
      )
    );
  }
}


```
重复的就不说了。
### title & subtitle
这两个放一起，都是复选框的文字描述，一主一次。
### dense
如果为`true`，表示这个复选框是一个列表的一部分，会缩小字体。  
为`false`时则是默认大小
### isThreeLine
是否为三行，这是个坑。  
首先要搭配`subtitle`使用，没有`subtitle`的话报错。  
然后就是这个坑了，不是文字以三行显示，而是告诉程序，我现在的文字(`title`)是不是有三行。。。  
`title`只有一行  
`isThreeLine`为`false`
![image.png](1.png)
`isThreeLine`为`true`
![image.png](2.png)
可以看到为`false`的时候，文字会垂直局中，为`true`的时候，文字偏上，这是因为告诉程序这段文字有三行，会把三行文字整体垂直居中显示，这就造成了上边的情况。  
`title`为三行  
`isThreeLine`为`false`
![image.png](3.png)
`isThreeLine`为`true`
![image.png](4.png)
可以看到显示上并没有什么区别，这是因为设置为`true`的时候，告诉了程序这是三行文字，会居中显示，为`false`的时候实际上是把整个`CheckboxListTile`的区域撑满，居中不居中并没有什么区别。

`title`为两行和一行的情况显示是相同的，最多就是三行，超过三行的文字不会显示，会被剪切。  
实际上`isThreeLine`是设置`title`和`subtitle`同时存在时文字在垂直方向的显示方式。
### secondary
图标，可以不设置。
### controlAffinity
复选框、文字、图标的排列顺序，有三个值。  
`leading`：复选框在前，文字在中间图标在最后。  
`trailing`：复选框在后，文字在中间图标在前。  
`platform`：这个是根据不同平台的默认情况自己调整。

`CheckBox`和`CheckboxListTile`相对其他组件来说简单一点，但是这里涉及到了`StatefulWidget`，在之前的笔记里没出现过，需要仔细想一下。
