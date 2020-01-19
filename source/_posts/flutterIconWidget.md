---
title: flutter--图标Icon
date: 2020-01-05 19:15:59
tags:
---
```
const Icon(
  IconData icon,                          //具体展示的图标
 {                      
    Key key,
    double size,                          //字体大小
    Color color,                          //颜色
    String semanticLabel,                   //语义标签
    TextDirection textDirection            //icon里可以添加文本，文本的书写方向
  }
)
```
<!--more-->
官方给出了4种icon  
`Icons`：基础的图标。  
`IconButton`：交互式图标，就是图标按钮。  
`IconTheme`：为图标提供环境配置。  
`ImageIcon`：自定义图片图标。
## 一、Icons
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
          child: Icon(
            Icons.add,
            size: 50.0,
            color: Colors.blue
          )
        )
      )
    );
  }
}
```
比较简单，设置字体大小颜色。  
flutter内置了material的icon，所有icon看这里  [design.google.com/icons/](https://design.google.com/icons/)直接用就可以。（打不开？自己想办法）

## 二、自定义字体图标
不想用material提供的图标，想自定义？可以，看下面  
### 1、首先生成一个字体：
可以去这里[https://www.iconfont.cn/](https://www.iconfont.cn/)  
也可以去这里[https://icomoon.io/app/#/select](https://icomoon.io/app/#/select)  
下载一个字体图标包，解压。  
### 2、在项目根目录下新建一个`fonts`文件夹，把`tff`格式的字体放到文件夹下。
### 3、在`pubspec.yaml`中添加字体
```
flutter:
  uses-material-design: true
  fonts:
    - family: myIcon  #指定一个字体名
      fonts:
        - asset: fonts/icomoon.ttf
```
### 4、使用字体图标
```
body: new Container(
  alignment: Alignment.center,
  child: Icon(
    IconData(
      0xe90c,                    //去下载的字体包里找到css文件，把字体的`content`粘过来，别忘了加`0x`表示十六进制
      fontFamily: 'myIcon'      //自定义的字体名
    )
  )
)
```

## 三、IconButton
`Icon`里有这货，`Button`里也有这货，不纯洁。  
写在`Icon`里吧，下一篇写`Button`就不写他了。  
```
const IconButton({
  Key key,
  double iconSize: 24.0,                                        //字体大小
  EdgeInsetsGeometry padding: const EdgeInsets.all(8.0),        //按钮的内边距
  AlignmentGeometry alignment: Alignment.center,                //图标的对齐方式
  @required Widget icon,                                         //图标
  Color color,                                                  //图标字体颜色
  Color focusColor,                                             //获得焦点时的颜色?
  Color hoverColor,                                              //鼠标悬停时的颜色？
  Color highlightColor,                                         //按钮按下时的背景颜色
  Color splashColor,                                           //点击时，水波动画中水波的颜色
  Color disabledColor,                                          //禁用时的颜色
  @require VoidCallback onPressed,                              //点击按钮的回调函数
  FocusNode focusNode,                                          //
  String tooltip                                                 //点击按钮的提示语
})
```
上代码，看demo
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
          child: IconButton(
            icon: new Icon(Icons.add),
            iconSize: 24.0,
            padding: const EdgeInsets.all(4.0),
            alignment: Alignment.centerLeft,
            color: Colors.red,
            highlightColor: Colors.blue,
            splashColor: Colors.green,
            tooltip: 'test',
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
这是个圆形的按钮。  
`focusNode`和`focusColor`推测是配合使用，和可以获得焦点的节点联动，获得焦点改变图标颜色，还未实验，欢迎补充。  
`tooltip`是要长按才出现提示，点击不会出现，`highlightColor`也是需要长按。  
`hoverColor`推测是桌面端和web端用的，毕竟移动端没发hover。  
调整`iconSize/alignment/padding`会发现`IconButton`是有一个固定的宽高尺寸的，不可设置。
## 四、IconTheme
这货只有`child`和`data`，在`data`里配置样式。
```
const IconThemeData({
  Color color,
  double opacity,
  double size
})
```
样式也不多，下面上代码，看用法。
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
          child: IconTheme(
            data:  IconThemeData(color: Colors.red, opacity: 0.5, size: 30.0),
            child: Container(
              child: new Icon(Icons.add)
            )
          )
        )
      )
    );
  }
}
```
可以看到这里`IconTheme `的子节点，不一定必须是`Icon`，这里是个`Container`。  
`IconTheme`相当于一个装饰器，对其下面的所有图标样式统一设置。
## 五、ImageIcon
```
const ImageIcon(
  ImageProvider image,
  {
    Key key,
    double size,
    Color color,
    String semanticlabel
  }
)
```
用一张图片做icon，和`Image`加`color `效果基本相同，不管什么花里胡哨的图片，都渲染成纯色，这里一定要用png这种背景透明的图片。
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
          child: ImageIcon(
            AssetImage('images/logo.png'),
            color: Colors.red,
            size: 50.0
          )
        )
      )
    );
  }
}
```
Icon到这里基本上就结束了。
