---
title: flutter--按钮 各种Button
date: 2020-01-05 19:20:10
tags:
---
flutter提供了以下几种按钮  
`RaisedButton 、OutlineButton 、FlatButton 、IconButton 、FloatingActionButton 、MaterialButton` 。  
<!--more-->
其中`IconButton`在[图标Icon](/post/flutterIconWidget)里已经说过了，这里就不重复了。  
接下来挨个看一下。

## 一、MaterialButton
悬浮按钮自带灰色背景和灰色阴影，按下时阴影变大。
首先还是看一下`constructor`。
```
const MaterialButton({
  Key key,
  @required VoidCallback onPressed,              //点击按钮的回调函数
  ValueChanged<bool> onHighlightChanged,         //高亮变化的回调
  ButtonTextTheme textTheme,                     //按钮的字体主题
  Color textColor,                               //字体颜色
  Color disabledTextColor,                      //禁用时的字体颜色
  Color color,                                  //按钮背景色
  Color disabledColor,                          //禁用时的背景色
  Color focusColor,                              //联动节点获得焦点时的颜色
  Color hoverColor,                              //鼠标悬停时的颜色
  Color highlightColor,                          //按下背景颜色（长按，不是点击）
  Color splashColor,                            //水波纹颜色
  Brightness colorBrightness,                   //按钮亮度
  double elevation,                              //阴影尺寸
  double focusElevation,                        //联动节点获得焦点时的阴影尺寸
  double hoverElevation,                        //鼠标悬停时阴影尺寸
  double highlightElevation,                    //长按阴影尺寸
  double disabledElevation,                    //禁用时的阴影尺寸
  EdgeInsetsGeometry padding,                  //内边距
  ShapeBorder shape,                            //按钮的形状
  Clip clipBehavior: Clip.none,                //裁剪
  FocusNode focusNode,                         //联动节点
  MaterialTapTargetSize materialTapTargetSize,  //有效的点击区域大小
  Duration animationDuration,                  //动画时间  
  double minWidth,                              //最小宽
  double hight,                                //高度
  Widget child                                  //子节点
})
```
下面看demo
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
        body: new Container(
          alignment: Alignment.center,
          child: new MaterialButton(
            child: new Text('Button'),
            textTheme: ButtonTextTheme.primary,
            textColor: Colors.red,
            highlightColor: Colors.yellow,
            color: Colors.green,
            splashColor: Colors.purple,
            elevation: 10.0,
            highlightElevation: 20.0,
            padding: EdgeInsets.all(20.0),
            onHighlightChanged: (data) {
              print(data);
            },
            onPressed: () {
              print(1);
            },
          )
        )
      )
    );
  }
}
```
一个花里胡哨的按钮，下面挨个属性看一下，看效果的话不要像上边一样同时写太多，建议每次只写一个。
### onPressed
这个没啥说的，按钮的回调，手指抬起时触发，点击或者长按都会触发。
`********特别提醒，值为null的时候，按钮为禁用状态。`
### onHighlightChanged
和`onPressed`不同，这个按下按钮和松开按钮都会触发，有一个参数，按下为true，松开为false。
### textTheme
有三个值  
`normal`：没看出来有啥效果。  
`primary`：没看出来有啥效果+1。  
`accent`：字体变成了主题同一颜色。  
### textColor
字体颜色，如果`Text`的`style`设置了`color`，则显示为`Text`设置的值。
### color
背景色
### focusColor & focusElevation & focusNode
还没试，看文档的意思是需要一个联动元素，比如输入框，获得焦点的时候按钮改变状态。
### disabledTextColor & disabledTextColor & disabledElevation
禁用时按钮的状态，不挨个说了，和非禁用状态写法一样。   
`按钮是否禁用，没有单独的属性，看onPressed，如果为null，或者不写onPressed则为禁用。`
### hoverColor
推测是桌面端和web端用的，移动端不知道怎么触发hover。
### highlightColor
按钮按下的背景色，不是点击是长按。
### splashColor
水波纹的颜色，这个是点击
### colorBrightness
亮度有两个值：  
`Brightness.light`：默认的状态，文字黑色，`splashColor&highlightColor`都是灰色。  
`Brightness.dark`：文字白色，`splashColor&highlightColor`变亮了？？？  
你没看错，是不是写反了，这是个bug？  
这不是个bug，这里的`colorBrightness`是整个主题的亮度，当app主题亮度为`Brightness.light`的时候，在这里也设成`light`，按钮会变暗；相反如果主题亮度为`Brightness.dark`，这里也设成`dark`，按钮会变亮。  
这是个坑。。。从字面意思来看，谁能看出来这是主题亮度。  
### elevation & highlightElevation
double类型，阴影和按下按钮状态下的阴影尺寸。
### padding
这个之前介绍过，就不重复了。
### shape
基于`ShapeBorder`的类有四个，下面看一下这四个类。
```
//圆形
const CircleBorder({
  BorderSide side: BorderSide.none
})
//斜角（了解过机械的话可以理解成倒角）
const BeveledRectangleBorder({
  BorderSide side: BorderSide.none,
  BorderRadiusGeometry borderRadius: BorderRadius.zero
})
//圆角
const RoundedRectangleBorder({
  BorderSide side: BorderSide.none,
  BorderRadiusGeometry borderRadius: BorderRadius.zero
})
//也是圆角，但是和RoundedRectangleBorder不同，有最大值，超过最大值以最大值显示。
const ContinuousRectangleBorder({
  BorderSide side: BorderSide.none,
  BorderRadiusGeometry borderRadius: BorderRadius.zero
})
```
可以看见，除了`CircleBorder`其他三个都是一样的，为啥这个特殊，少了个属性？  
因为`CircleBorder`是设置按钮为圆形，`borderRadius`是圆角，根本不需要。。。  
下面看一下怎么用：  
`borderRadius`之前的笔记介绍过[容器组件Container](/post/flutterContainerWidget)，这里就不重复了，看一下demo。
```
new RaisedButton(
  child: new Text('button'),
  color: Colors.grey[200],
  shape: ContinuousRectangleBorder(
    borderRadius: BorderRadius.circular(10.0),
   ),
   onPressed: () {
     print(1);
  },
)
```
这样就给按钮加了圆角。  
下面看一下`BorderSide`
```
const BorderSide({
  Color color: const Color(0xFF000000),
  double width: 1.0,
  BorderStyle style: BorderStyle.solid
})
```
这个就比较简单了，之前的也都介绍过这几个属性。
### clipBehavior
裁剪，有四个值`antiAlias、antiAliasWithSaveLayer 、hardEdge、none `，都是对边缘的处理，看文档说`antiAlias`处理速度要比`antiAliasWithSaveLayer`快，但是要比`hardEdge`慢，还没整明白怎么用，以后补充。
### materialTapTargetSize
`padded`：有效点击区域最小为`48px * 48px`。  
`shrinkWrap`：可点击组件的大小。
### animationDuration
设置`shape`和`elevation `的动画时间，看一下怎么用。
```
new MaterialButton(
  child: new Text('button'),
  color: Colors.grey[200],
  animationDuration: new Duration(seconds: 10),
  highlightElevation: 200.0,
  textTheme: ButtonTextTheme.accent,
  onPressed: () {
    print(1);
  },
)
```
这里为了效果明显值写的都比较大，按住按钮不要松手可以看到阴影逐渐变大。  
`Duration`不只可以写`seconds`，`days、hours、minutes、seconds、milliseconds、microseconds`都可以。  
想不出来在什么情况下一个动画会以天为时间单位。。。
### minWidth & height
最小宽度和高度。
### child
子节点。
## 二、OutlineButton
默认有一个边框，不带阴影且背景透明。按下后，边框颜色会变亮、同时出现背景和阴影(较弱)。  
看一下`constructor`
```
const OutlineButton({
  Key key,
  @required VoidCallback onPressed,
  ButtonTextTheme textTheme,
  Color textColor,     
  Color disabledTextColor,    
  Color color,  
  Color focusColor,  
  Color hoverColor,   
  Color highlightColor,
  Color splashColor,  
  double highlightElevation,
  BorderSide borderSide,
  Color disabledBorderColor,
  Color highlightedBorderColor,
  EdgeInsetsGeometry padding,
  ShapeBorder shape,
  Clip clipBehavior,
  FocusNode focusNode,
  Widget child
})
```
和`RaisedButton`相同的就不说了，这里说一下`RaisedButton`没有的。  
啥是`RaisedButton`没有的，自己写一下就知道了，多了个边框。。。
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
        body: new Container(
          alignment: Alignment.center,
          child: new OutlineButton(
            child: new Text('Button'),
            borderSide: BorderSide(color: Colors.red, width: 4.0, style: BorderStyle.solid),
//            disabledBorderColor: Colors.blue,
            onPressed: () {
              print(1);
            }
          )
        )
      )
    );
  }
}

```
### disabledBorderColor
禁用时边框颜色，默认为灰色。
### borderSide
边框样式，`color`和`width`没啥说的，一个颜色一个宽度，`style`要说一下，这个只有`solid`和`none`，也就是说只有实线边框和没有边框。

另外这个按钮和`MaterialButton`相比少了几个属性，对比看吧这里就不列出来了。
## 三、RaisedButton
和`MaterialButton`默认样式不同，默认带有阴影和灰色背景。按下后，阴影会变大没有`height`和`minWidth`。
## 四、FlatButton
扁平按钮，默认背景透明并不带阴影。按下后，会有背景色。  
和`MaterialButton`相比少了以下属性：`elevation、focusElevation 、hoverElevation 、highlightElevation 、disabledElevation 、animationDuration 、minWidth 、height`。
## 五、FloatingActionButton
页面的悬浮按钮。  
看一下`constructor`
```
const FloatingActionButton({
  Key key,
  Widget child,
  String tooltip,
  Color foregroundColor,          //前景色，不会覆盖文字，加了之后改变了文字的颜色
  Color backgroundColor,        //背景色
  Color focusColor,
  Color hoverColor,
  Object heroTag: const _DefaultHeroTag(),      //动画效果
  double elevation,
  double focusElevation,
  double hoverElevation,
  double highlightElevation,  
  double disabledElevation,   
  @required VoidCallback onPressed,
  bool mini: false,                    //为true时按钮尺寸会变小
  ShapeBorder shape,
  Clip clipBehavior: Clip.none,
  FocusNode focusNode,
  MaterialTapTargetSize materialTapTargetSize,
  bool isExtended: false              //是否扩展
})
```
什么前景色背景色就不说了，说一下没见过的。
### heroTag
设置为null，则不启用过渡动画，如果悬浮按钮在两个页面内的位置不同，页面切换时动作生硬。不设置的话为默认值，平滑过渡。
### mini
为`true`时为按钮会缩小，但是要注意的是，这个缩小只是尺寸缩小，不是整体缩放，内容大小还是需要手动设置。
### isExtended
是否扩展，默认为false。  
不知道怎么用，但是发现个特性。

把`FloatingActionButton`写在`Container`里，同时`Container`不写`alignment`，并且`Container`设有`width`和`height`。  
`false`：按钮会尽量放大到宽高为`width&height`之中小的那个尺寸，并保持圆形。  
`true`：按钮宽高会变成`Container`的宽高，不会保持圆形，但是有圆角。  
为啥要说写在`Container`里，因为这货就不是用在`Container`里的。。。  
怎么用往下看
```
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return new MaterialApp(
      home: Scaffold(
        floatingActionButton: FloatingActionButton(
          onPressed: () {
             print(1);
          },
          child: Icon(Icons.add)
        ),
      )
    );
  }
}
```
这才是常规用法，默认悬浮在右下角，不影响布局。  
你说不想放右下角？可以，放哪都行，但是我不打算在这写，因为这个位置不是在按钮这设置的。

如有错误欢迎指出，很多不足欢迎补充。
