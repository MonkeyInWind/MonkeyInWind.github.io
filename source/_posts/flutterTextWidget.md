---
title: flutter--hello world和文本组件Text、TextSpan
date: 2020-01-05 17:50:37
tags:
---
# Hello World
代码的世界从`hello world`开始，flutter也一样。
创建一个项目，将`lib`文件夹下的`main.dart`改为如下代码：
<!--more-->
```
void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Demo',
      home: Scaffold(
      body: new Center(
        child: new Text(
          'hello world'
          )
        )
      )
    );
  }
}
```
跑起来
![image.png](1.png)
`hello world`完成了。

先简单介绍两个组件
### 1、Scaffold
打开任意一款app，顶部标题栏、侧边抽屉、底部导航等...，基本上都有这里边的一个或者几个，基于这个现状，material很贴心的提供了`scaffold`这个组件，相当于一个页面的骨架，可以把上边说的那些东西拼到里边。
### 2、Center
很简单的一句话，这个组件的子组件水平垂直居中。
这两个只是简单的介绍一下，以后再详细说，接下来是这篇笔记的主角。
# 文本Widget
## 一、Text
`hello world`这个demo里用过了，相当于`html`里边的`<p></p>`，但是又有所不同，都知道`p`标签独占一行，宽度如果没有限制则为父级宽度，`Text`也是独占一行，但是宽度为内容宽度，并且没有`width`属性。
看上边的demo，是不是感觉字体有点小，还想换个颜色，加个下划线...还有其他各种骚操作。
先看一下官网给出的Text
```
const Text(
  String data,                                   //文本上边demo的hello world
  {
    Key key,                                   //唯一标识，相当于react中map渲染节点的key
    TextStyle style,                            //样式
    StrutStyle strutStyle,                    //？？？不知道干啥的
    TextAlign textAlign,                      //对齐方式
    TextDirection textDirection,          //文本的书写顺序
    Locale locale,                                //设置语言环境  就是国际化，多语言支持
    bool softWrap,                              //文本过长是否自动换行
    TextOverflow overflow,                  //对溢出文本的显示方式
    double textScaleFactor,                //每个逻辑像素的字体像素数
    int maxLines,                                //文本的最大行数
    String semanticsLabel,                  //图像的语义描述，用于向Andoid上的TalkBack和iOS上的VoiceOver提供图像描述
  }
)
```
接下来挨个看
### 1、TextStyle
设置字体的样式
```
const TextStyle({
  bool inherit: true,                                  //是否继承父级
  Color color,                                           //字体颜色
  Color backgroundColor,                        //背景色
  double fontSize,                                      //字体大小
  FontWeight fontWeight,                          //字体粗细
  FontStyle fontStyle,                                //正常/斜体
  double letterSpacing,                              //字符间距可为负
  double wordSpacing,                              //字间距（英文单词间距）
  TextBaseline textBaseline,                        //文本对齐基线
  Height height,                                          //Text的高度，相当于行高
  Local locale,                                            //设置语言环境  就是国际化，多语言支持
  Paint foreground,                                      //不知道是啥
  Paint background,                                      //文本背景色作用和backgroundColor相同
  List<Shadow> shadows,                            //文字阴影
  TextDecoration decoration,                        //划线
  Color decorationColor,                                //划线颜色
  TextDecoration decorationStyle,                 //划线种类
  double decorationThickness,                    //划线的粗细
  String debugLabel,                                   //文本样式的文本描述，仅在debug模式下有效           
  String fontFamily,
  List<String> fontFamilyFallback,
  String package,
})
```
接下来详解
#### color
flutter的color支持5种写法
```
style: TextStyle(
  color: Color(0xFF42A5F5),                                            //十六进制色号两个F的位置为透明度，取值范围00～FF
  color: Color.fromARGB(0xFF, 0x42, 0xA5, 0xF5),         //十六进制色号第一位为透明度，从00～FF
  color: Color.fromARGB(255, 66, 165, 245),                   //十进制色号第一位为透明度，0~255
  color: Color.fromRGBO(66, 165, 245, 1.0),                    //最后一位为透明度, 0.0~1.0
  color: Colors.red                                                   //material内置
)
```
#### backgroundColor
背景色和color的写法一样
#### fontSize
字体大小，double类型
```
style: TextStyle(
  fontSize: 30.0
)
```
不用写单位，flutter的单位是`dp`。
#### letterSpacing
字符间距
```
style: TextStyle(
  letterSpacing: 6.0
)
```
#### wordSpacing
```
style: TextStyle(
  wordSpacing: 10.0
)
```
#### textBaseline
对齐基线，类似css的基线，`alphabetic/ideographic`两个值
```
style: TextStyle(
  textBaseline: TextBaseline.alphabetic
)
```
alphabetic：简单理解为英文的对齐基线  
ideographic：简单理解为中文对齐基线
#### height
```
style: TextStyle(
  height: 1.5
)
```
和css一样1.5就是字体大小的1.5倍。
#### background
这个注意了，不是`Color`，是`Paint`。
```
style: TextStyle(
  background: Paint() ..color = Colors.blue
)
```
这个和`backgroundColor`一样，两者不能共存。  
..是`dart`语法糖，前一个函数的返回值的属性，说的有点绕，看代码。
```
Paint() ..color = Colors.blue;
//下边代码的简写
Paint pg = Paint();
pg.color = Colors.blue;
```
#### shadows
`List`类型
```
style = TextStyle(
  shadows: [Shadow(color: Colors.black,offset: Offset(5, 6),blurRadius: 3 )]
)
```
这里说明一下参数
`color`：阴影颜色，
`offset`：两个参数xy方向的偏移量，
`blurRadius`: 模糊程度
#### decoration
和css的`text-decoration`类似
```
style = TextStyle(
  decoration: TextDecoration.underline
)
```
有5个值  
`underline`：下划线   
`none`：无划线  
`overline`：上划线  
`lineThrough`：中划线  
`combine`：这个就厉害了，可以传入一个`List`，三线齐划
#### decorationColor
划线的颜色，默认和字体颜色相同。
```
style: TextStyle(
  decorationColor:  Colors.black
)
```
#### decorationStyle
默认为实线
```
style = TextStyle(
  decorationStyle: TextDecorationStyle.dashed
)
```
`dashed`：点划线  
`dotted`：虚线  
`double`：双划线  
`solid`：实线  
`wavy`：波浪线
#### decorationThickness
划线的粗细，默认为1
```
style = TextStyle(
  decorationThickness: 3.0
)
```
#### debugLabel
```
style = TextStyle(
  debugLabel: 'test
)
```
加上之后没找到怎么看这个提示。。。
### 2、strutStyle
看文档这个应该是`style`的简写，类似css里边的`background/font`这种，可以把样式写到一起，样式是有顺序的，这里不研究了，不推荐这种写法，可读性不高不好维护。
### 3、textAlign
对齐方式，和css的`text-align`基本上相同
```
textAlign: TextAlign.start
```
`start`：起始位置
`end`：结束位置
`center`：居中
`left`：左对齐
`right`：右对齐
`justify`：两端对齐
### 4、textDirection
```
textDirection: TextDirection.ltr
```
`ltr`：从左到右  
`rtl`：从右到左  
left to right，right to left
### 5、locale
```
locale: Locale('fr', 'CH')
```
这个不是添加了就会自动翻译，还要配置其他东西，以及第三方包，以后再详细说。
### 6、softWrap
```
softWrap: true
```
文本超出容器时是否自动换行，默认为`true`，为`false`时文本超出容器部分默认被剪切。
### 7、overflow
```
overflow: TextOverflow.clip
```
对文本溢出部分的处理，类似css中的`overflow`。  
`clip`：切断，超出部分不显示，默认值  
`ellipsis`：超出部分不显示，显示...  
`visible`：超出部分强制显示  
`fade`：超出部分淡出

### 8、textScaleFactor
```
textScaleFactor: 1.5
```
缩放的倍数
### 9、maxLines
```
maxLines: 2
```
文本的最大行数
####10、semanticsLabel
```
semanticsLabel: 'test'
```
这个应该是相当于html中`img`的`alt`。  
下面上完整代码，把上边demo中的`Center`换成`Container`（相当于html中的`div`，下篇笔记详细说），再加个`width`便于观察样式和属性对文本的改变。
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
          width: 400.0,
          child: new Text(
            'hello world hello world hello world hello world hello world hello world',
            style: TextStyle(
              color: Color.fromARGB(0xFF, 0x42, 0xA5, 0xF5),
              backgroundColor: Colors.red,
              fontSize: 30.0,
              letterSpacing: 6.0,
              wordSpacing: 15.0,
              height: 2.0,
//              background: Paint() ..color = Colors.blue,
              shadows: [Shadow(color: Colors.black,offset: Offset(5, 6),blurRadius: 3 )],
              decoration: TextDecoration.combine([
                TextDecoration.underline,
                TextDecoration.overline
              ]),
              decorationColor: Colors.black,
              decorationStyle: TextDecorationStyle.wavy,
              decorationThickness: 3.0,
              debugLabel: 'text'
            ),
            textAlign: TextAlign.justify,
            textDirection: TextDirection.rtl,
            locale: Locale('fr', 'CH'),
            softWrap: true,
            overflow: TextOverflow.visible,
            textScaleFactor: 1.5,
            maxLines: 2,
            semanticsLabel: 'test'
          )
        )
      )
    );
  }
}
```
学习的时候建议不要像这里一样加太多的样式和属性，不相关的属性或者样式先单独练习再组合，有的需要配合使用，比如溢出`softWrap `、`overflow `、`maxLines `这些。
## 二、TextSpan
html里有个`span`这里有个`TextSpan`，作用基本相同，文字放一行，下面看代码。
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
          child: new Text.rich(
            TextSpan(
              children: [
                new TextSpan(text: 'hello: '),
                new TextSpan(
                  text: 'world',
                  style: TextStyle(
                    color: Colors.red
                  )
                )
              ]
            )
          )
        )
      )
    );
  }
}
```
效果
![image.png](2.png)
`TextSpan`需要套一层`Text.rich`，可以有`children`，`children`同为`TextSpan`，可以分别加不同的样式，这里只能加样式，不可以加其他的属性。  
文本组件到这里就结束了，如有遗漏欢迎补充，如有错误请指正。
