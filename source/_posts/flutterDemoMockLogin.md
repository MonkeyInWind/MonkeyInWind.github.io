---
title: 模拟登录
date: 2020-01-05 20:13:33
tags:
---
`dart`官方文档：[https://www.dartcn.com/](https://www.dartcn.com/)

先放上demo的github地址：[https://github.com/MonkeyInWind/flutter_demo](https://github.com/MonkeyInWind/flutter_demo)这篇笔记中的demo都在这个项目里。
<!--more-->
前边学了一些简单的组件下面从0开始来写个demo。  
动手之前先看demo效果
![Dec-12-2019 22-43-30.gif](1.gif)
# 一、创建一个项目
在之前的笔记里（[官方示例&代码解读](/post/flutterOfficialExample)）已经说过怎么创建项目，以及官方demo的解读，这里就不详细说了。
```
flutter create flutter_demo
```
看到控制台打印以下信息，说明创建成功了。
![image.png](2.png)
打开`Android Studio`
![image.png](3.png)
选择一个模拟器打开，打开之后就可以选择模拟器跑项目了。
![image.png](4.png)
选择刚打开的模拟器，点绿色的三角启动，不想用模拟器选web也可以。
![image.png](5.png)
跑起来就可以进行下一步了。
# 二、写Demo
### 1、一段文本
![image.png](6.png)
这里选成`Project`，打开`lib`目录下的`main.dart`，删除大部分代码，只保留以下部分：
```
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(

    );
  }
}
```
接下来在`MaterialApp`中添加文本。
```
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Text('我是一段文本')
    );
  }
}
```
![image.png](7.png)
文字显示出来了，但是这也太难看了，接下来进行装饰。
```
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(    //翻译成中文就是脚手架，提供了一个布局框架，里边有很多常用的api，比如顶部标题、底部菜单、左右抽屉等。
        appBar: AppBar(
          title: Text('文本')
        ),
        body: Text('我是一段文本')
      )
    );
  }
}
```
![image.png](8.png)
变成了这样，比前边好多了，我们再改一下，让这段文字在页面内居中显示，并且换个颜色。
```
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(
          title: Text('文本')
        ),
        body: Center(
          child: Text(
            '我是一段文本',
            style: TextStyle(
              color: Color.fromARGB(0xFF, 0xFF, 0x11, 0xF5)
            )
          )
        )
      )
    );
  }
}
```
只需要加上`Center`这个widget，就实现了水平垂直居中。  
关于`Text`的详细介绍看这里[hello world和文本组件Text、TextSpan](/post/flutterTextWidget)

到这可能有人会提出问题，一个app不可能所有代码都放在一个class里，那根本没法看，这就是接下来要干的事。
### 2、组件封装
定义一个新的class叫`Page1`，并把`scaffold`放在里边。
```
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Page1()
    );
  }
}

class Page1 extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('page1')
      ),
      body: Center(
        child: Text(
          '我是一段文本',
          style: TextStyle(
            color: Color.fromARGB(0xFF, 0xFF, 0x11, 0xF5)
          )
        )
      )
    );
  }
}
```
是不是感觉像是在写react（不考虑他这蛋疼的写法）。  
写到这又会有人提出问题，这是没写到同一个class里，但是是在同一个文件里啊，接下来咱们就拆成两个文件。  
在`lib`目录下新建一个文件叫`page1.dart`。
![image.png](9.png)
内容如下
```
import 'package:flutter/material.dart';

class Page1 extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('page1')
      ),
      body: Center(
        child: Text(
          '我是一段文本',
          style: TextStyle(
            color: Color.fromARGB(0xFF, 0xFF, 0x11, 0xF5)
          )
        )
      )
  );
  }
}
```
就是把`material.dart`import进来之后再把刚刚`main.dart`里`Page1`粘过来。  
下面改写`main.dart`，将刚新建的`page1.dart`import进来。
```
import 'package:flutter/material.dart';
import 'package:flutter_demo/page1.dart';   //项目目录名/文件名

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Page1(),
    );
  }
}
```
这样就做到了组件拆分。  
开篇的demo地址切到`demo_1`分支，就是上边的完整代码。
### 3、一个方块
html有个最常用的标签`div`，曾有一段时间把页面布局叫做div布局，flutter里有个类似的widget叫`Container`（[容器组件Container](/post/flutterContainerWidget)）。  
接下来我们在`lib`目录下新建一个`page2.dart`文件。  
整体框架和`page1.dart`相同。
```
import 'package:flutter/material.dart';

class Page2 extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('page2')
      ),
      body: Center(

      )
    );
  }
}
```
我们在`main.dart`里把`page2.dart`import进来，然后把home改成`Page2`。
```
import 'package:flutter/material.dart';
//import 'package:flutter_demo/page1.dart';
import 'package:flutter_demo/page2.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Page2(),
    );
  }
}
```
![image.png](10.png)
可以看见一个空白的页面，title是`page2`，接下来在`Center`里边写一个`Container`。
```
import 'package:flutter/material.dart';

class Page2 extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('page2')
      ),
      body: Center(
        child: Container(
          width: 200,
          height: 200,
          color: Colors.red
        )
      )
    );
  }
}
```
可以看见页面上出现了一个红色的方块。  
有page1和page2了，接下来看以下页面怎么跳转。  
### 4、路由
我们来改造一下前边写的demo，`main.dart`还是import`page1.dart`。
```
import 'package:flutter/material.dart';
import 'package:flutter_demo/page1.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Page1(),
    );
  }
}
```
在`page1.dart`中，我们在右下角加一个悬浮按钮。
```
import 'package:flutter/material.dart';

class Page1 extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('page1')
      ),
      body: Center(
        child: Text(
          '我是一段文本',
          style: TextStyle(
            color: Color.fromARGB(0xFF, 0xFF, 0x11, 0xF5)
          )
        )
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: () {
          print('pressed next page');
        }
      )
    );
  }
}
```
这个时候右下角出现了一个蓝色的悬浮按钮，点击之后可以看见控制台输出打印的信息。
![image.png](11.png)
为了好看一点，我们在按钮里加一个图标（[图标Icon](/post/flutterIconWidget)）。
```
floatingActionButton: FloatingActionButton(
  onPressed: () {
    print('pressed next page');
  },
  child: new Icon(Icons.arrow_forward),
)
```
效果如下：
![image.png](12.png)
接下来是跳转到下一页  
先在`page1.dart`中将`page2.dart`import一下，然后写路由跳转，`page1.dart`完整的代码如下：
```
import 'package:flutter/material.dart';
import 'package:flutter_demo/page2.dart';

class Page1 extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('page1')
      ),
      body: Center(
        child: Text(
          '我是一段文本',
          style: TextStyle(
            color: Color.fromARGB(0xFF, 0xFF, 0x11, 0xF5)
          )
        )
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: () {
          print('pressed next page');
          Navigator.push(
            context,
            new MaterialPageRoute(builder: (context) => Page2()),
          );
        },
        child: new Icon(Icons.arrow_forward),
      )
    );
  }
}
```
效果如下
![Dec-08-2019 13-21-39.gif](13.gif)
如果不想点标题栏的返回按钮，也可以自定义。  
将`page2.dart`中的`Container`删除，换成一个按钮`MaterialButton`（[按钮 各种Button](/post/flutterButtonWidget)
）。
代码如下
```
import 'package:flutter/material.dart';

class Page2 extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('page2')
      ),
      body: Center(
        child: MaterialButton(
          child: Text('back'),
          color: Colors.blue,
          onPressed: () {
            print('back');
            Navigator.pop(context);
          }
        )
      )
    );
  }
}
```
![image.png](14.png)
只需要调用`Navigator.pop(context)`方法就可以返回上一页。  
分支切换到`demo_2`为以上demo的代码。
### 5、插播一条调试
##### 布局
写web都知道开发者工具可以定位到页面上任意一个元素，flutter也可以。  
`Android Studio`在菜单栏`View -> Tool Windows -> Flutter Inspector`。  
打开之后在编辑区右侧出现了调试工具。
![image.png](15.png)
`Widgets`可以看见整个页面的结构，点左上角的准星，可以去模拟器中选中某一个`Widget`。
![image.png](16.png)
并且可以看见这个`Widget`上的所有属性和样式同时模拟器左下角还会出现一个放大镜，点击放大镜后可以再选中其他`Widget`。
![image.png](17.png)
另一个工具可以查看页面的整体布局。
##### 打断点
需要在debug模式下运行才可以打断点。
![image.png](18.png)
用这只虫子启动项目，在某一行代码前点击，出现红色的圆点。
![image.png](19.png)
然后点击`back`这个按钮。
![image.png](20.png)
和web一样。
##### 利用浏览器调试
![image.png](21.png)
点击这个按钮会在浏览器打开`Dart DevTools`，和在`Android Studio`调试基本相同，就不重复了，放一张图。
![image.png](22.png)
### 6、图片
首先新建一个`page3.dart`的文件，在`page2.dart`中添加`floatingActionButton`，并将`page3.dart`import进来，`page3.dart` 中搭好页面。  
`page2.dart`
```
import 'package:flutter/material.dart';
import 'package:flutter_demo/page3.dart';

class Page2 extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('page2')
      ),
      body: Center(
        child: MaterialButton(
          child: Text('back'),
          color: Colors.blue,
          onPressed: () {
            print('back');
            Navigator.pop(context);
          }
        )
      ),
      floatingActionButton: FloatingActionButton(
        child: Icon(Icons.arrow_forward),
        onPressed: () {
          Navigator.push(
            context,
            new MaterialPageRoute(builder: (context) => Page3()),
          );
        }
      ),
    );
  }
}
```
`page3.dart`
```
import 'package:flutter/material.dart';

class Page3 extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('page3')
      ),
      body: Center(

      ),
    );
  }
}
```
在第二页点右下角的按钮会进入第三页， 一个空页面。  
接下来在根目录下新建一个`images`文件夹，里边放一张图片。  
把刚才图片的路径添加到`pubspec.yaml`。
![image.png](23.png)

接下来就可以使用这张图片了。
```
import 'package:flutter/material.dart';

class Page3 extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('page3')
      ),
      body: Center(
        child: Image.asset('./images/logo.png')
      ),
    );
  }
}
```
保存之后page3中会居中显示一个flutter的logo。  
有时候我们不需要把图片打包进来，需要用到网络图片，这个时候需要把`Image.asset`换成`Image.network`。
```
import 'package:flutter/material.dart';

class Page3 extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('page3')
      ),
      body: Center(
        child: Image.network('https://www.baidu.com/img/bd_logo1.png')
      ),
    );
  }
}
```
去网上复制一张图片的链接，如果图片不显示，有可能是图片加了防盗链，这里用了百度的logo。  
网络图片都有个加载时间，我们在放一个loading占位。  
把`Center`换成`Stack`，`Stack`里放两个`Center`，再把loading和图片放在`Center`里。
```
import 'package:flutter/material.dart';

class Page3 extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
        appBar: AppBar(
            title: Text('page3')
        ),
        body: Stack(
          children: <Widget>[
            Center(
              child: CircularProgressIndicator()
            ),
            Center(
              child: Image.network('https://www.baidu.com/img/bd_logo1.png')
            ),
          ],
        ),
    );
  }
}
```
![Dec-09-2019 21-19-47.gif](24.gif)

这里简单介绍一下`Stack`，类似于`position: relative`，子`Widget`会重叠显示，上边的demo实际上是图片加载之后把loading盖住了。  
上边虽然实现了loading占位，但是图片显示太过生硬，我们用`FadeInImage`给他加个淡入效果。  
`pubspec.yaml`里添加一个`transparent_image`（[https://github.com/brianegan/transparent_image](https://github.com/brianegan/transparent_image)
）在图片加载之前占位用，这里其实体现不出来他的作用，但是`placeholder`不能为空。

这里用了第三方的包[https://pub.dev/](https://pub.dev/)，类似于npm的仓库。

![image.png](25.png)

命令行执行`flutter pub get`如果是在`Android Studio`中添加之后会有提示，安装好之后改写一下`page3.dart`。
```
import 'package:flutter/material.dart';
import 'package:transparent_image/transparent_image.dart';

class Page3 extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
        appBar: AppBar(
            title: Text('page3')
        ),
        body: Stack(
          children: <Widget>[
            Center(
              child: CircularProgressIndicator()
            ),
            Center(
              child: FadeInImage.memoryNetwork(
                placeholder: kTransparentImage,
                image: 'https://www.baidu.com/img/bd_logo1.png'
              )
            ),
          ],
        ),
    );
  }
}
```
效果如下
![Dec-09-2019 21-17-09.gif](26.gif)

这就比直接显示图片要好很多。  
以上代码在demo_3分支。
### 7、滚动列表 & 网格布局
还是在`page3.dart`中添加`floatingActionButton`，然后新建一个文件`page4.dart`，在`page3.dart`中import。  
先看一个最简单的列表
```
import 'package:flutter/material.dart';

class Page4 extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('page4')
      ),
      body: ListView(
        children: <Widget>[
          ListTile(
            leading: Icon(Icons.phone),
            title: Text('Title1')
          ),
          ListTile(
              leading: Icon(Icons.cached),
              title: Text('Title2')
          ),
          ListTile(
              leading: Icon(Icons.adb),
              title: Text('Title3')
          ),
          ListTile(
              leading: Icon(Icons.adjust),
              title: Text('Title4')
          ),
        ],
      )
    );
  }
}
```
![image.png](27.png)

这样就实现了一个滚动列表，可以多复制一些`ListTitle`尝试一下上下滑动，这里就不写了。  
纵向滚动实现里，下面看一下横向滚动，直接在`ListView`里加一个`Container`。
```
import 'package:flutter/material.dart';

class Page4 extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('page4')
      ),
      body: ListView(
        children: <Widget>[
          ListTile(
            leading: Icon(Icons.phone),
            title: Text('Title1')
          ),
          ListTile(
              leading: Icon(Icons.cached),
              title: Text('Title2')
          ),
          Container(
            height: 200,
            child: ListView(
              scrollDirection: Axis.horizontal,
              children: [
                Container(
                  color: Colors.red,
                  width: 150,
                ),
                Container(
                  color: Colors.blue,
                  width: 150,
                ),
                Container(
                  color: Colors.green,
                  width: 150,
                ),
                Container(
                  color: Colors.yellow,
                  width: 150,
                ),
              ]
            )
          )
        ],
      )
    );
  }
}
```
![image.png](28.png)
这就在纵向列表里加了一个横向的列表。  
这还不满足的话继续往下看，我们还可以在列表里同时展示两列。
```
import 'package:flutter/material.dart';

class Page4 extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('page4')
      ),
      body: ListView(
        children: <Widget>[
          ListTile(
            leading: Icon(Icons.phone),
            title: Text('Title1')
          ),
          ListTile(
              leading: Icon(Icons.cached),
              title: Text('Title2')
          ),
          Container(
            height: 200,
            child: ListView(
              scrollDirection: Axis.horizontal,
              children: [
                Container(
                  color: Colors.red,
                  width: 150,
                ),
                Container(
                  color: Colors.blue,
                  width: 150,
                ),
                Container(
                  color: Colors.green,
                  width: 150,
                ),
                Container(
                  color: Colors.yellow,
                  width: 150,
                ),
              ]
            )
          ),
          Container(
            height: 200,
            decoration: BoxDecoration(
              border: Border.all(width: 1, color: Colors.black)
            ),
            child: GridView.count(
              crossAxisCount: 2,
              children: List.generate(100, (index) {
                return Center(
                  child: Text('Item $index')
                );
              })
            )
          )
        ],
      )
    );
  }
}
```
![Dec-11-2019 21-09-18.gif](29.gif)

这里用了`GridView`和`List.generate`。  
`GridView`就是网格布局，可以指定一行有几个`Widget`，每个`Widget`之间的距离等。  
`List.generate`值是一个函数，返回一个`Widget`，用来生成一组`Widget`。  
以上代码在`demo_4`分支。
### 8、手势
前边都是一些常见`Widget`的简单用法，接下来说一下手势。  
先来介绍一下`Pointers`，用户与屏幕交互的原始数据，包括`PointerDownEvent`、`PointerMoveEvent`、`PointerUpEvent`、`PointerCancelEvent`。类似于移动端`web`的`touch`事件。  
再说一下手势，一个或几个`Pointer`的封装，先来一个按钮的demo看一下。
```
import 'package:flutter/material.dart';

class Page5 extends StatelessWidget{
  final items = new List<String>.generate(5, (i) => 'Item ${i + 1}');

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('page5'),
      ),
      body: InkWell(
        child: Center(
          child: Container(
            child: Text('this is a button'),
            padding: EdgeInsets.only(
              top: 10,
              bottom: 10
            )
          )
        ),
        onTap: () {
          print('on tap');
        },
        onTapDown: (tapDownDetail) {
          print('on tap down');
        },
        onTapCancel: () {
          print('on tap cancel');
        },
        onDoubleTap: () {
          print('on dubble tap');
        },
        onLongPress: () {
          print('on long press');
        }
      )
    );
  }
}
```
这样一个全屏的大按钮就完成了，点击还有水波纹效果。。。  
监听了五种手势，可以看一下打印。  
接下来对demo进行改造，写一个可以滑动删除的列表。
```
import 'package:flutter/material.dart';

class Page5 extends StatelessWidget{
  final items = new List<String>.generate(5, (i) => 'Item ${i + 1}');

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('page5'),
      ),
      body: ListView(
        children: <Widget> [
          InkWell(
            child: Center(
              child: Container(
                child: Text('this is a button'),
                padding: EdgeInsets.only(
                  top: 10,
                  bottom: 10
                )
              )
            ),
            onTap: () {
              print('on tap');
            },
            onTapDown: (tapDownDetail) {
              print('on tap down');
            },
            onTapCancel: () {
              print('on tap cancel');
            },
            onDoubleTap: () {
              print('on dubble tap');
            },
            onLongPress: () {
              print('on long press');
            }
          ),
          Container(
            height: 400,
            child: ListView.builder(
              itemCount: items.length,
              itemBuilder: (context, index) {
                final item = items[index];
                return Dismissible(
                  key: Key('key_$index'),
                  onDismissed: (direction) {
                    items.removeAt(index);
                    Scaffold.of(context).showSnackBar(
                      SnackBar(
                        content: Text('$item dismissed')
                      )
                    );
                  },
                  background: Container(
                    color: Colors.red
                  ),
                  child: ListTile(
                    title: Text('$item')
                  )
                );
              }
            )
          )
        ]
      )
    );
  }
}
```
看一下效果
![Dec-11-2019 21-27-49.gif](30.gif)
这里用了`List.generate`方法生成了一个`List` ，然后用了`ListView.builder`生成了一个`ListView`。`Dismissible`是flutter提供的一个可以滑动删除的`Widget`，`SnackBar`就是底部的提示。  
关于手势的中文文档看这里[https://flutterchina.club/gestures/](https://flutterchina.club/gestures/)。  
以上demo在`demo_5`分支。
### 9、有状态组件
前边的`Widget`都是继承`StatelessWidget`也就是无状态组件，接下来看一下`StatefulWidget`有状态组件。  
先来布个局
![image.png](31.png)
这个结构很简单，上边一个红色的`Container`，下边一行放三个按钮`MaterialButton`，最终实现的效果就是点击按钮改变上边`Container`的颜色。  
先看布局代码
```
import 'package:flutter/material.dart';

class Page6 extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('page6'),
      ),
      body: Column(
        crossAxisAlignment: CrossAxisAlignment.center,
        children: <Widget>[
          Container(
            width: 200,
            height: 200,
            color: Colors.red,
            margin: EdgeInsets.only(
              top: 20,
              bottom: 20
            ),
          ),
          Row (
            mainAxisAlignment: MainAxisAlignment.center,
            children: <Widget>[
              Container (
                padding: EdgeInsets.only(
                  left: 10,
                  right: 10
                ),
                child: MaterialButton (
                  color: Colors.red,
                  child: Text('red'),
                  onPressed: () {
                    print('red');
                  },
                ),
              ),
              Container (
                padding: EdgeInsets.only(
                    left: 10,
                    right: 10
                ),
                child: MaterialButton (
                  color: Colors.blue,
                  child: Text('blue'),
                  onPressed: () {
                    print('blue');
                  }
                ),
              ),
              Container (
                padding: EdgeInsets.only(
                  left: 10,
                  right: 10
                ),
                child: MaterialButton (
                  color: Colors.green,
                  child: Text('green'),
                  onPressed: () {
                    print('green');
                  }
                )
              )
            ],
          )
        ],
      )
    );
  }
}
```
里边用了`Column`和`Row`这两个新的`Widget`，一个是子`Widget`纵向排列，另一个是横向排列，另外还用了`MainAxisAlignment `主轴上的对齐方式和`CrossAxisAlignment `交叉轴上的对齐方式，这里都用了居中，至于`margin`为什么不加在`MaterialButton`上，对不起，没有。  
接下来对demo进行改写，`StatelessWidget`肯定是不行的，要换成`StatefulWidget`。  
先看代码
```
import 'package:flutter/material.dart';

class Page6 extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('page6'),
      ),
      body: BoxChangeColor()
    );
  }
}

class BoxChangeColor extends StatefulWidget {
  @override
  _BoxChangeColorState createState() => new _BoxChangeColorState();
}

class _BoxChangeColorState extends State<BoxChangeColor> {
  @override
  Widget build(BuildContext context) {
    return Column(
      crossAxisAlignment: CrossAxisAlignment.center,
      children: <Widget>[
        Container(
          width: 200,
          height: 200,
          color: Colors.red,
          margin: EdgeInsets.only(
            top: 20,
            bottom: 20
          ),
        ),
        Row (
          mainAxisAlignment: MainAxisAlignment.center,
          children: <Widget>[
            Container (
              padding: EdgeInsets.only(
                left: 10,
                right: 10
              ),
              child: MaterialButton (
                color: Colors.red,
                child: Text('red'),
                onPressed: () {
                  print('red');
                },
              ),
            ),
            Container (
              padding: EdgeInsets.only(
                left: 10,
                right: 10
              ),
              child: MaterialButton (
                color: Colors.blue,
                child: Text('blue'),
                onPressed: () {
                  print('blue');
                }
              ),
            ),
            Container (
              padding: EdgeInsets.only(
                left: 10,
                right: 10
              ),
              child: MaterialButton (
                color: Colors.green,
                child: Text('green'),
                onPressed: () {
                  print('green');
                }
              )
            )
          ],
        )
      ],
    );
  }
}
```
这里新建了一个`BoxChangeColor `继承了`StatefulWidget`，并重写了`createState`方法，再创建一个`_BoxChangeColorState `类继承`State`，在`_BoxChangeColorState `返回上边的`Column`，这样就完成了一个缺少状态的有状态组件。  
以`_`开头表示私有。  
下面把缺少的状态添加进去。  
在`_BoxChangeColorState`中声明一个变量，这个变量就是`state`，类型为`Color`并把这个`state`写成`Container`的 `color`属性值，在`MaterialButton`的`onPressed`事件中调用`setState`方法来改变`state`，这个时候会重新`build`，实现了切换颜色。  
这个demo的完整带么如下：
```
import 'package:flutter/material.dart';

class Page6 extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('page6'),
      ),
      body: BoxChangeColor()
    );
  }
}

class BoxChangeColor extends StatefulWidget {
  @override
  _BoxChangeColorState createState() => new _BoxChangeColorState();
}

class _BoxChangeColorState extends State<BoxChangeColor> {
  Color color = Colors.red;

  @override
  Widget build(BuildContext context) {
    return Column(
      crossAxisAlignment: CrossAxisAlignment.center,
      children: <Widget>[
        Container(
          width: 200,
          height: 200,
          color: color,
          margin: EdgeInsets.only(
            top: 20,
            bottom: 20
          ),
        ),
        Row (
          mainAxisAlignment: MainAxisAlignment.center,
          children: <Widget>[
            Container (
              padding: EdgeInsets.only(
                left: 10,
                right: 10
              ),
              child: MaterialButton (
                color: Colors.red,
                child: Text('red'),
                onPressed: () {
                  setState(() {
                    color = Colors.red;
                  });
                },
              ),
            ),
            Container (
              padding: EdgeInsets.only(
                left: 10,
                right: 10
              ),
              child: MaterialButton (
                color: Colors.blue,
                child: Text('blue'),
                onPressed: () {
                  setState(() {
                    color = Colors.blue;
                  });
                }
              ),
            ),
            Container (
              padding: EdgeInsets.only(
                left: 10,
                right: 10
              ),
              child: MaterialButton (
                color: Colors.green,
                child: Text('green'),
                onPressed: () {
                  setState(() {
                    color = Colors.green;
                  });
                }
              )
            )
          ],
        )
      ],
    );
  }
}
```
再来看一下效果
![Dec-11-2019 22-43-58.gif](32.gif)
以上代码在`demo_6`分支。
### 10、模拟登录
前边都是基本用法的介绍，而且demo都很零碎，接下来做一个模拟登录的demo。  
点击`Login`按钮跳转至登录页面，点击`Cancel`按钮返回登录页面，输入`username`和`password`后点击登录页面的`Login`按钮模拟登录，跳转回前一页面，这个时候隐藏登录页面的`Login`按钮并显示一张图片。  
首先新建一个`login.dart`文件用作登录页面，终于不是page了，在里边创建一个名为`Login`的`StatefulWidget`，然后在`main.dart`中把这个文件import进来，然后在`main.dart`创建一个名为`HomePage`的`StatefulWidget`作为主页，中间放一个按钮，点击按钮跳转至登录页。  
先看效果
![Dec-12-2019 14-51-49.gif](33.gif)
再看代码
`main.dart`
```
import 'package:flutter/material.dart';
import 'package:flutter_demo/login.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: HomePage(),
    );
  }
}

class HomePage extends StatefulWidget {
  @override
  _HomePageState createState() => new _HomePageState();
}

class _HomePageState extends State<HomePage> {
  @override
  Widget build(BuildContext context) {
    return Scaffold (
      appBar: AppBar(
        title: Text('Login')
      ),
      body: Center (
        child: RaisedButton(
          child: Text('Login'),
          color: Colors.blue,
          onPressed: () {
            Navigator.push(
              context,
              MaterialPageRoute(
                fullscreenDialog: true,
                builder: (context) => Login()
              )
            );
          }
        )
      )
    );

  }
}

```
这里用了个`fullscreenDialog `属性，全屏弹窗，下一个页面从底部弹出。  
`login.dart`
```
import 'package:flutter/material.dart';

class Login extends StatefulWidget {
  @override
  _LoginState createState() => new _LoginState();
}

class _LoginState extends State<Login> {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Column(
        crossAxisAlignment: CrossAxisAlignment.center,
        children: <Widget>[
          Container (
            padding: EdgeInsets.only(
              top:50,
              left: 20,
              right: 20
            ),
            child: TextField (
              decoration: InputDecoration(
                prefixIcon: Icon(Icons.person),
                labelText: ('username')
              ),
            )
          ),
          Container (
            padding: EdgeInsets.all(20),
            child: TextField (
              decoration: InputDecoration(
                prefixIcon: Icon(Icons.lock),
                labelText: ('password')
              ),
              obscureText: true
            )
          ),
          Row (
            mainAxisAlignment: MainAxisAlignment.center,
            children: <Widget>[
              Container (
                margin: EdgeInsets.only(
                  left: 10,
                  right: 10
                ),
                child: RaisedButton(
                  child: Text('Login'),
                  color: Colors.blue,
                  onPressed: () {

                  }
                )
              ),
              Container (
                margin: EdgeInsets.only(
                  left: 10,
                  right: 10
                ),
                child: RaisedButton(
                  child: Text('Cancel'),
                  onPressed: () {
                    Navigator.pop(context);
                  }
                )
              )
            ]
          )
        ],
      )
    );
  }
}
```
这里用了文本输入框`TextField`，一个花里胡哨的`Widget`属性一大堆，和`html`里的`input`但是要比`input`强大很多，想要的效果这个`Widget`基本上都有，只要加属性就行了。这里用了`decoration `的`prefixIcon`输入框前的图标，`labelText `类似于`input`的`placeholder`当输入框获得焦点时虽小到左上角，还有`obscureText `是否密文。

写到这布局基本上就算完成了，接下来加状态，`HomePage`加一个登录状态，`Login`加上`username`和`password`，并在点`Login`的时候做空校验，通过之后跳转回`HomePage`并带上登录状态。

首先在`Login`里添加两个`state`，`username`和`password`，并在两个`TextField`里监听`onChanged`事件，给`username`和`password`赋值，并在点击`Login`的时候做校验。

`TextField`
```
TextField (
  decoration: InputDecoration(
    prefixIcon: Icon(Icons.person),
    labelText: ('username')
  ),
  onChanged: (value) {
    setState(() {
      username = value;
    });
  },
)
```
`username`和`password`一样，就不重复了。  

`Cancel`
```
RaisedButton(
  child: Text('Cancel'),
  onPressed: () {
    setState(() {
      username = '';
      password = '';  
    });
    Navigator.pop(context);
  }
)
```
取消的时候将`username`和`password`置空。  

`Login`
```
RaisedButton(
  child: Text('Login'),
  color: Colors.blue,
  onPressed: () {
    String tip = '';
    if (username == '') {
      tip = 'username empty';
    } else if (password == '') {
      tip = 'password empty';
    } else {
      setState(() {
        username = '';
        password = '';
      });
    tip = 'success';
    }
    print(tip);
  }
)
```
在这里对`username`和`password`做校验，得到不同的`tip`。  
三种不同状态，可以打印出来。
![image.png](35.png)
接下来做提示，就用之前用过的`SnackBar`。
```
import 'package:flutter/material.dart';
import 'dart:async';

class Login extends StatefulWidget {
  @override
  _LoginState createState() => new _LoginState();
}

class _LoginState extends State<Login> {
  String username = '';
  String password = '';

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Builder (builder: (BuildContext context) {
        return Column(
          crossAxisAlignment: CrossAxisAlignment.center,
          children: <Widget>[
            Container (
              padding: EdgeInsets.only(
                top:50,
                left: 20,
                right: 20
              ),
              child: TextField (
                decoration: InputDecoration(
                  prefixIcon: Icon(Icons.person),
                  labelText: ('username')
                ),
                onChanged: (value) {
                  setState(() {
                    username = value;
                  });
                },
              )
            ),
            Container (
              padding: EdgeInsets.all(20),
              child: TextField (
                decoration: InputDecoration(
                  prefixIcon: Icon(Icons.lock),
                  labelText: ('password')
                ),
                obscureText: true,
                onChanged: (value) {
                  setState(() {
                    password = value;
                  });
                }
              )
            ),
            Row (
              mainAxisAlignment: MainAxisAlignment.center,
              children: <Widget>[
                Container (
                  margin: EdgeInsets.only(
                    left: 10,
                    right: 10
                  ),
                  child: RaisedButton(
                    child: Text('Login'),
                    color: Colors.blue,
                    onPressed: () {
                      String tip = '';
                      if (username == '') {
                        tip = 'username empty';
                      } else if (password == '') {
                        tip = 'password empty';
                      } else {
                        setState(() {
                          username = '';
                          password = '';
                        });
                        Timer(
                          Duration(seconds: 1),
                          () {
                            Navigator.pop(context, true);
                          }
                        );
                        tip = 'success';
                      }
                      Scaffold.of(context).showSnackBar(
                        SnackBar(
                          content: Text(tip),
                          duration: Duration(seconds: 1),
                        )
                      );
                    }
                  )
                ),
                Container (
                  margin: EdgeInsets.only(
                    left: 10,
                    right: 10
                  ),
                  child: RaisedButton(
                    child: Text('Cancel'),
                    onPressed: () {
                      setState(() {
                        username = '';
                        password = '';
                      });
                      Navigator.pop(context, false);
                    }
                  )
                )
              ]
            )
          ],
        );
      })
    );
  }
}
```
这里有几处处改动，`Column`套了个`Builder`方法，否则在用`Scaffold.of(context)`的时候会报错，查了一下是`context`的问题。另一个是`Login`的`onChanged`，这里加了`SnackBar`，`duration`是提示的时间，如果登录成功开了个一秒的定时器，在一秒之后返回上一页，这里需要注意的一点就是，`Timer`是在`dart/sync`这个包里，需要import进来，另外在登录成功和取消按钮跳转回前一页时传回一个登录状态的参数。  
接下来把`main.dart`稍作改动
```
import 'package:flutter/material.dart';
import 'package:flutter_demo/login.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: HomePage(),
    );
  }
}

class HomePage extends StatefulWidget {
  @override
  _HomePageState createState() => new _HomePageState();
}

class _HomePageState extends State<HomePage> {
  bool logged = false;

  @override
  Widget build(BuildContext context) {
    return Scaffold (
      appBar: AppBar(
        title: Text('Login')
      ),
      body: Center (
        child: logged ? Image.asset('./images/logo.png')
            :
          RaisedButton(
            child: Text('Login'),
            color: Colors.blue,
            onPressed: () async {
              var result = await Navigator.push(
                context,
                MaterialPageRoute(
                  fullscreenDialog: true,
                  builder: (context) => Login()
                )
              );
              setState(() {
                logged = result;
              });
            }
          )
      )
    );
  }
}
```
在这同样加一个`logged`表示登录状态，`Center`的`child`根据`logged`判断显示图片还是按钮，`Login`按钮的`onPressed`事件这需要注意一下，加了`sync`表示这是一个异步方法，`Navigator`这返回一个`Feature`，需要`await`一下，看着和js的`async`、`await`、`promise`用法一样，`result`就是返回的参数，把他set给`logged`，就实现了我们的需求。  
再来看一下效果
![Dec-12-2019 22-43-30.gif](35.gif)
### 11、网络请求
前边的登录是在app里模拟，但是写应用是离不开网络请求的，接下来我们添加个网络请求，调个登录接口。  
这里用一个封装好的第三方网络请求[https://github.com/flutterchina/dio](https://github.com/flutterchina/dio)  
还是在`pubspec.yaml`里添加`dio`。
```
dependencies:
  dio: 3.0.7
  flutter:
    sdk: flutter
```
添加好之后`android studio`会提示需要下载这个包，或者在命令行
```
flutter pub get
```
下载好了之后在`login.dart`中`import`进来。
```
import 'package:dio/dio.dart';
```
接下来就是改写代码，首先在`state`里声明一个`dio`
![image.png](36.png)
接下来在登录按钮的`onpressed`中添加网络请求
```
RaisedButton(
  child: Text('Login'),
  color: Colors.blue,
  onPressed: () async {
    String tip = '';
    if (username == '') {
      tip = 'username empty';
    } else if (password == '') {
      tip = 'password empty';
    } else {
      try {
        Response res = await dio.post(
            'http://118.25.7.84:10086/login',    //这个是我自己的服务器提供的接口，可以直接用
            data: {
              'username': username,
              'password': password
            }
        );
        print(res.data);
        tip = res.data;
      } catch (e) {
        print(e);
        tip = 'failed';
      }
    }
    Scaffold.of(context).showSnackBar(
      SnackBar(
        content: Text(tip),
        duration: Duration(seconds: 1),
      )
    );
  }
)
```
具体这就实现了网络请求，接口返回`登录成功`，另外用了`try catch`捕获异常。  
贴一下完整代码
```
import 'package:flutter/material.dart';
import 'dart:async';
import 'package:dio/dio.dart';

class Login extends StatefulWidget {
  @override
  _LoginState createState() => new _LoginState();
}

class _LoginState extends State<Login> {
  Dio dio = new Dio();
  String username = '';
  String password = '';

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Builder (builder: (BuildContext context) {
        return Column(
          crossAxisAlignment: CrossAxisAlignment.center,
          children: <Widget>[
            Container (
              padding: EdgeInsets.only(
                top:50,
                left: 20,
                right: 20
              ),
              child: TextField (
                decoration: InputDecoration(
                  prefixIcon: Icon(Icons.person),
                  labelText: ('username')
                ),
                onChanged: (value) {
                  setState(() {
                    username = value;
                  });
                },
              )
            ),
            Container (
              padding: EdgeInsets.all(20),
              child: TextField (
                decoration: InputDecoration(
                  prefixIcon: Icon(Icons.lock),
                  labelText: ('password')
                ),
                obscureText: true,
                onChanged: (value) {
                  setState(() {
                    password = value;
                  });
                }
              )
            ),
            Row (
              mainAxisAlignment: MainAxisAlignment.center,
              children: <Widget>[
                Container (
                  margin: EdgeInsets.only(
                    left: 10,
                    right: 10
                  ),
                  child: RaisedButton(
                    child: Text('Login'),
                    color: Colors.blue,
                    onPressed: () async {
                      String tip = '';
                      if (username == '') {
                        tip = 'username empty';
                      } else if (password == '') {
                        tip = 'password empty';
                      } else {
                        try {
                          Response res = await dio.post(
                              'http://118.25.7.84:10086/login',
                              data: {
                                'username': username,
                                'password': password
                              }
                          );
                          print(res.data);
                          tip = res.data;
                          Timer(
                            Duration(seconds: 1),
                            () {
                              Navigator.pop(context, true);
                            }
                          );
                        } catch (e) {
                          print(e);
                          tip = 'failed';
                        }
                      }
                      Scaffold.of(context).showSnackBar(
                        SnackBar(
                          content: Text(tip),
                          duration: Duration(seconds: 1),
                        )
                      );
                    }
                  )
                ),
                Container (
                  margin: EdgeInsets.only(
                    left: 10,
                    right: 10
                  ),
                  child: RaisedButton(
                    child: Text('Cancel'),
                    onPressed: () {
                      setState(() {
                        username = '';
                        password = '';
                      });
                      Navigator.pop(context, false);
                    }
                  )
                )
              ]
            )
          ],
        );
      })
    );
  }
}
```
目前flutter已经支持`MacOs`，可以在吗命令行执行以下命令
```
flutter config --enable-macos-desktop
```
在项目目录下执行
```
flutter create .
```
即可体验  
在`android studio`的设备下啦列表了会多一个`macOs(desktop)`，选中后就可以跑起来了。  
这里需要注意的是，跑起来之后调接口是会失败的，需要在`macos/Runner/DebugProfile.entitlements`文件中添加`com.apple.security.network.client`。
```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
	<key>com.apple.security.app-sandbox</key>
	<true/>
	<key>com.apple.security.cs.allow-jit</key>
	<true/>
	<key>com.apple.security.network.server</key>
	<true/>
	<key>com.apple.security.network.client</key>
	<true/>
</dict>
</plist>
```
以上代码在`http_server`分支。

最后还是要吐槽一下`flutter`的地狱嵌套，当然和我没有拆分组件有关系。。。看着脑袋疼。
