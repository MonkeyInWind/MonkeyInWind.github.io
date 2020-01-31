---
title: flutter--Scaffold以及功能组件介绍
date: 2020-01-31 18:45:53
tags:
---
`Scaffold`这个组件在之前的笔记都出现过，但是没有详细的说明，这一篇笔记就来介绍一下。  
先看`constructor`。
<!--more-->
```
const Scaffold({
    Key key,
    PreferredSizeWidget appBar,  // 页面顶部标题栏
    Widget body,        // 页面的主体
    Widget floatingActionBotton,  // 悬浮按钮
    FloatingActionButtonLocation floatingActionButtonLocation,  //悬浮按钮的位置
    FloatingActionButtonAnimator floatingActionButtonAnimator,  //悬浮按钮的动画
    List<Widget> persistentFooterButtons,   //底部按钮
    Widget drawer,      //左侧抽屉
    Widget endDrawer,   //右侧抽屉
    Widget bottomNavigationBar,     //底部导航
    Widget bottomSheet,     //底部滑出
    Color backgroundColor,      //背景色
    bool resizeToAvoidBottomPadding,    //已弃用，键盘弹出时是否重新绘制，以避免输入框被遮挡
    bool resizeToAvoidBottomInset,      //键盘弹出时是否重新绘制，以避免输入框被遮挡
    bool primary: true,     //是否计算手机顶部状态栏的高度
    DragStartBehavior drawerDragStartBehavior: DragStartBehavior.start,  //拖动的处理
    bool extendBody: false,     //是否延伸body至底部。
    bool extendBodyBehindAppBar: false,     //是否延伸body至顶部。
    Color drawerScrimColor,     //抽屉遮罩层背景色
    double drawerEdgeDragWidth  //滑动拉出抽屉的生效距离
})
```
## appBar
```
AppBar({
    Key key,
    Widget leading,
    bool automaticallyImplyLeading: true,
    Widget title,
    List<Widget>actions,
    Widget flexibleSpace,
    PreferredSizeWidget bottom,
    double elevation,
    ShapeBorder shape,
    Color backgroundColor,
    Brightness brightness,
    IconThemeData iconTheme,
    IconThemeData actionsIconTheme,
    TextTheme textTheme,
    bool primary: true,
    bool centerTitle,
    double titleSpacing: NavigationToolbar.kMiddleSpacing,
    double toolbarOpacity: 1.0,
    double bottomOpacity: 1.0
})
```
先看一张官方文档`AppBar`的图

![](1.png)
可以看见，之前只用到`AppBar`的`title`，其实这个`Widget`是很强大的，接下来详细看一下。
### leading
这个之前用过，但是是默认的，就是路由跳转之后，在`AppBar`左上角有一个返回按钮，就是它。  
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
        appBar: AppBar(
          leading: MaterialButton(
            child: Text('L'),
            onPressed: () {
              print('pressed leading');
            },
          )
        ),
      )
    );
  }
}
```
左上角的按钮，在这里用了一个`MaterialButton`，点击的时候会打印出`pressed leading`。
效果如图
![](2.png)
就是左上角的那个`L`。  
### automaticallyImplyLeading
这个就牛逼了，如果`leading`为空,`automaticallyImplyLeading`值为`true`的话，就自动推断出`leading`是什么并创建，比如之前说的返回按钮，再比如加了个`Drawer`，`leading`会是一个操作`Drawer`的按钮，这个就不在这写demo了，看后边。
### title
这个就不细说了，之前的笔记里用过很多次了。
### actions
`leading`在左侧，这个`action`在右侧，与`leading`不同的是，`action`是一个`List`类型，可以有多个子`Widget`。
```
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return new MaterialApp(
      home: Scaffold(
        appBar: AppBar(
          actions: <Widget>[
            MaterialButton(
              child: Text('BTN1')
            ),
            MaterialButton(
              child: Text('BTN2')
            )
          ],
        ),
      )
    );
  }
}
```

![](3.png)
### flexibleSpace
一个高度可以自适应的`Widget`，被标题和工具栏覆盖，看一下demo。
```
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return new MaterialApp(
      home: Scaffold(
        appBar: AppBar(
          title: Text(
            'title',
            style: TextStyle(
              color: Colors.red
            )
          ),
          flexibleSpace: FlexibleSpaceBar(
            title:  Text('flexibleSpace title')
          ),
        ),
      )
    );
  }
}
```
![](4.png)
可以看见两个标题重叠了，并且`title`覆盖在了`flexibleSpace title`之上。
`flexibleSpace`一般用在`SliverAppBar`里，`SliverAppBar`是一个可以随着内容一起滚动的头部，大多数都是在滚动页面改变头部高度时用。
### bottom
布局上和`title`一行的`toolbar`同级，但是是底对齐，和`toolbar`同时出现时会把`toolbar`顶上去。
```
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return new MaterialApp(
      home: Scaffold(
        appBar: AppBar(
          leading: MaterialButton(
            child: Text('H'),
            onPressed: () {

            },
          ),
          title: Text(
            'title',
            style: TextStyle(
              color: Colors.red
            )
          ),
          bottom: PreferredSize(
            child: Text('bottom')
          )
        ),
      )
    );
  }
}
```

![](5.png)
可以看到`title`和`leading`所在的`toolbar`被顶上去了。
### elevation
`appBar`下方的阴影，默认值是4。
```
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return new MaterialApp(
      home: Scaffold(
        appBar: AppBar(
          title: Text(
            'title'
          ),
          elevation: 10
        ),
      )
    );
  }
}
```

![](6.png)
### shape
设置`appBar`的形状，常规来说应用场景不会太多。。。
```
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return new MaterialApp(
      home: Scaffold(
        appBar: AppBar(
          title: Text(
            'title'
          ),
          shape: StadiumBorder(),
        ),
      )
    );
  }
}
```

![](7.png)
```
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return new MaterialApp(
      home: Scaffold(
        appBar: AppBar(
          title: Text(
            'title'
          ),
          shape: CircleBorder(),
        ),
      )
    );
  }
}
```

![](8.png)

### backgroundColor
背景色，没啥说的。
```
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return new MaterialApp(
      home: Scaffold(
        appBar: AppBar(
          title: Text(
            'title'
          ),
          backgroundColor: Colors.red,
        ),
      )
    );
  }
}
```

![](9.png)
### brightness
主题色调，两个值`Brightness.dark`和`Brightness.light`，没看出什么效果，关于`brightness`，这里有介绍[按钮 各种Button](/post/flutterButtonWidget/)
### iconTheme
[图标Icon](/post/flutterIconWidget/)这里有介绍，不重复了。
### actionsIconTheme
同上。
### textTheme
与`iconTheme`类似。
### primary
`appBar`是否计算手机顶部状态栏的高度，默认为`true`，为`false`时如图

![](10.png)
### centerTitle
`title`是否居中，默认为`true`，为`false`是居左。
### titleSpacing
`title`和其他元素的距离。
```
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return new MaterialApp(
      home: Scaffold(
        appBar: AppBar(
          leading: MaterialButton(
            child: Text('L'),
            onPressed: () {

            },
          ),
          title: Text(
            'title',
          ),
          actions: <Widget>[
            MaterialButton(
              child: Text('A'),
              onPressed: () {

              },
            ),
            MaterialButton(
              child: Text('B'),
              onPressed: () {

              },
            )
          ],
          bottom: PreferredSize(
            child: Text('bottom')
          ),
          titleSpacing: 70,
        ),
      )
    );
  }
}
```

![](11.png)
设置过大自己内容显示不全，出现了...
### toolbarOpacity
`toolbar`透明度，默认是`1.0`。
```
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return new MaterialApp(
      home: Scaffold(
        appBar: AppBar(
          leading: MaterialButton(
            child: Text('L'),
            onPressed: () {

            },
          ),
          title: Text(
            'title',
          ),
          actions: <Widget>[
            MaterialButton(
              child: Text('A'),
              onPressed: () {

              },
            ),
            MaterialButton(
              child: Text('B'),
              onPressed: () {

              },
            )
          ],
          bottom: PreferredSize(
            child: Text('bottom')
          ),
          toolbarOpacity: 0.5
        ),
      )
    );
  }
}
```

![](12.png)
貌似只对`title`生效。
### bottomOpacity
`bottom`的透明度。
```
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return new MaterialApp(
      home: Scaffold(
        appBar: AppBar(
          leading: MaterialButton(
            child: Text('L'),
            onPressed: () {

            },
          ),
          title: Text(
            'title',
          ),
          actions: <Widget>[
            MaterialButton(
              child: Text('A'),
              onPressed: () {

              },
            ),
            MaterialButton(
              child: Text('B'),
              onPressed: () {

              },
            )
          ],
          bottom: PreferredSize(
            child: Text('bottom')
          ),
          bottomOpacity: 0.5
        ),
      )
    );
  }
}
```

![](13.png)
## body
值是一个`Widget`，在顶部工具栏和底部菜单栏之间，可以是`Container`，可以是`Text`，也可以是`Center`或者其他。。。
## floatingActionBotton
对于这个`Widget`在[按钮 各种Button](/post/flutterButtonWidget/)中有介绍，这里就不详细说了。
## floatingActionButtonLocation
悬浮按钮的位置  
`FloatingActionButtonLocation.endTop`：右上角  
`FloatingActionButtonLocation.centerFloat`：下居中，不贴边  
`FloatingActionButtonLocation.centerDocked`：下居中贴边  
`FloatingActionButtonLocation.endDocked`：右下角贴底边  
`FloatingActionButtonLocation.endFloat`：默认值，右下角不贴边  
`FloatingActionButtonLocation.startTop`：左上角  
`FloatingActionButtonLocation.miniStartTop`：左上角，离左侧边框更近一点。  
这里需要注意，不管左上角还是右上角，全都是悬浮按钮的中心线在顶部边框的位置，也就是有一半按钮在屏外
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
          child: Text('BTN'),
        ),
        floatingActionButtonLocation: FloatingActionButtonLocation.startTop
      )
    );
  }
}
```

![](14.png)
再看一下在底部贴边的效果
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
          child: Text('BTN'),
        ),
        floatingActionButtonLocation: FloatingActionButtonLocation.centerDocked
      )
    );
  }
}
```

![](15.png)
## floatingActionButtonAnimator
操作`floatingActionButton`的动画效果，例如旋转、缩放、偏移。  
具体怎么用还不知道。。。
## persistentFooterButtons
固定在页面下方的一组按钮(不止可以放按钮也可以放其他`Widget`，按钮比较常见)，不随页面滚动，右对齐，新增的`Widget`会在最右侧，这个不是底部导航。
水平方向空间足够时，按钮会水平排列，空间不足时会竖直排列。
```
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return new MaterialApp(
      home: Scaffold(
        persistentFooterButtons: <Widget>[
          MaterialButton(
            child: Text('BTN1'),
            onPressed: () {
              print('pressed btn1');
            },
          ),
          MaterialButton(
            child: Text('BTN2'),
            onPressed: () {
              print('pressed btn2');
            },
          ),
          MaterialButton(
            child: Text('BTN3'),
            onPressed: () {
              print('pressed btn3');
            },
          ),
          MaterialButton(
            child: Text('BTN4'),
            onPressed: () {
              print('pressed btn4');
            },
          ),
          Container(
            width: 50,
            height: 50,
            color: Colors.red
          )
        ],
      )
    );
  }
}
```

![](16.gif)
## drawer
最常见的左抽屉，自带半透明遮罩层，值可以是`Container`也可以是其他，如`Button`、`Text`。  
这里加上`appBar`会自动生成一个`leading`，同时向右滑动也可以拉出抽屉。
```
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return new MaterialApp(
      home: Scaffold(
        appBar: AppBar(
          title: Text('title')
        ),
        drawer: Container(
          width: 150,
          color: Colors.blue
        )
      )
    );
  }
}
```

![](17.gif)
如果不想用`appBar`自带的`leading`，也可以自定义，flutter提供了`ScaffoldState.openDrawer`函数。
## endDrawer
右侧抽屉，和左侧一样，两个抽屉可同时存在。
## drawerDragStartBehavior
触发拖拽的时机，有两个值，默认为`DragStartBehavior.start`，还有`DragStartBehavior.down`，文档推荐用默认值，试了一下，没感觉两个有明显差别。
## drawerScrimColor
抽屉遮罩层背景色，默认为`Colors.black54`。
## drawerEdgeDragWidth
滑动拉出抽屉的生效距离，设置为0时不能滑动拉出。
## bottomNavigationBar
这里需要注意的是，值不只可以是`BottomNavigationBar`，其他`Widget`也可以，试了一下`ListView`，不会显示在底部，居上显示，失去了底部导航的意义。
底部导航，先看一下`constructor`。
```
BottomNavigationBar({
    Key key,
    @required List<BottomNavigationBarItem> items,
    ValueChanged<int> onTap,
    int currentIndex: 0,
    double elevation: 8.0,
    BottomNavigationBarType type,
    Color fixedColor,
    Color backgroundColor,
    double iconSize: 24.0,
    Color selectedItemColor,
    Color unselectedItemColor,
    IconThemeData selectedIconTheme: const IconThemeData(),
    IconThemeData unselectedIconTheme: const IconThemeData(),
    double selectedFontSize: 14.0,
    double unselectedFontSize: 12.0,
    TextStyle selectedLabelStyle,
    TextStyle unselectedLabelStyle,
    bool showSelectedLabels: true,
    bool showUnselectedLabels: true
})
```
### items
菜单的按钮
```
BottomNavigationBarItem({
    @required Widget icon,  //菜单按钮的图标
    Widget title,   //菜单按钮的标题
    Widget activeIcon,  //active状态下的图标
    Color backgroundColor   //背景色，需要type为shifting，否则没有效果
})
```
```
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return new MaterialApp(
      home: Scaffold(
        bottomNavigationBar: BottomNavigationBar(
          items: <BottomNavigationBarItem>[
            BottomNavigationBarItem(
              icon: Icon(
                Icons.home
              ),
              title: Text('HOME'),
            ),
            BottomNavigationBarItem(
              icon: Icon(
                Icons.android
              ),
              title: Text('ANDROID')
            )
          ],
        )
      )
    );
  }
}
```

![](18.png)
接下来给`HOME`加一个`activeIcon`
```
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return new MaterialApp(
      home: Scaffold(
        bottomNavigationBar: BottomNavigationBar(
          items: <BottomNavigationBarItem>[
            BottomNavigationBarItem(
              icon: Icon(
                Icons.home
              ),
              title: Text('HOME'),
              activeIcon: Icon(
                Icons.done
              )
            ),
            BottomNavigationBarItem(
              icon: Icon(
                Icons.android
              ),
              title: Text('ANDROID'),
            )
          ],
        )
      )
    );
  }
}
```

![](19.png)
### onTap
点击回调，有个`int`类型的参数，为点击的索引。
```
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return new MaterialApp(
      home: Scaffold(
        bottomNavigationBar: BottomNavigationBar(
          items: <BottomNavigationBarItem>[
            BottomNavigationBarItem(
              icon: Icon(
                Icons.home
              ),
              title: Text('HOME'),
            ),
            BottomNavigationBarItem(
              icon: Icon(
                Icons.android
              ),
              title: Text('ANDROID'),
            )
          ],
          onTap: (index){
            print(index);
          }
        )
      )
    );
  }
}
```

![](20.png)
### elevation
导航栏的阴影，关于阴影前边有介绍，这里就不重复了。
### type
flutter的这个底部的导航栏，不只提供了一种形式，默认为`fixed`，就是前边的那种，还有一种`shifting`，下面来看一下。
```
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return new MaterialApp(
      home: Scaffold(
        bottomNavigationBar: BottomNavigationBar(
          items: <BottomNavigationBarItem>[
            BottomNavigationBarItem(
              icon: Icon(
                Icons.home
              ),
              title: Text('HOME'),
              backgroundColor: Colors.red
            ),
            BottomNavigationBarItem(
              icon: Icon(
                Icons.android
              ),
              title: Text('ANDROID'),
            )
          ],
          onTap: (index){
            print(index);
          },
          type: BottomNavigationBarType.shifting,
        )
      )
    );
  }
}
```
可以看到`BottomNavigationBarItem`的背景色起作用了，只需要在一个设置了，整个导航栏都加上了，尝试了一下分别设置不同的颜色，只有第一个起作用。  
图标和标题默认为白色，并且不显示标题，`active`状态下图标会放大，同时标题也会显示出来。
### fixedColor
`icon`和`title`的颜色。
### backgroundColor
标题栏的背景色，只在`type`为`fixed`时起作用。  
感觉这有点乱，`fixed`和`shifting`背景色设置不统一。
### iconSize
`icon`的大小
### selectedItemColor & unselectedItemColor
这两个放一起，选中和未选中的颜色。
```
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return new MaterialApp(
      home: Scaffold(
        bottomNavigationBar: BottomNavigationBar(
          items: <BottomNavigationBarItem>[
            BottomNavigationBarItem(
              icon: Icon(
                Icons.home
              ),
              title: Text('HOME'),
            ),
            BottomNavigationBarItem(
              icon: Icon(
                Icons.android
              ),
              title: Text('ANDROID'),
            )
          ],
          onTap: (index){
            print(index);
          },
          selectedItemColor: Colors.red,
          unselectedItemColor: Colors.green
        )
      )
    );
  }
}
```

![](21.png)
红配绿
### selectedIconTheme & unselectedIconTheme
这个和前边那两个颜色类似，是主题。
### selectedFontSize & unselectedFontSize
选中和未选中的字体大小
### selectedLabelStyle & unselectedLabelStyle
两种状态下的文字样式，关于文字的样式看这里[hello world和文本组件Text、TextSpan](/post/flutterTextWidget/)
### showSelectedLabels & showUnselectedLabels
是否显示选中 / 为选中的`title`默认都是`true`，`false`时不显示。
### currentIndex
选中的索引
```
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return new MaterialApp(
      home: Scaffold(
        bottomNavigationBar: BottomNavigationBar(
          items: <BottomNavigationBarItem>[
            BottomNavigationBarItem(
              icon: Icon(
                Icons.home
              ),
              title: Text('HOME'),
            ),
            BottomNavigationBarItem(
              icon: Icon(
                Icons.android
              ),
              title: Text('ANDROID'),
            )
          ],
          currentIndex: 1
        )
      )
    );
  }
}
```

![](22.png)
## bottomSheet
底部滑出的组件。
先看一个最简单的`demo`。
```
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return new MaterialApp(
      home: Scaffold(
        bottomSheet:Container(
          height: 100,
          color: Colors.red,
          child: Center(
            child: Text('bottomSheet')
          )
        ),
      )
    );
  }
}
```

![](23.png)
这就完成了，这种东西用在哪里？微信聊天界面底部，是不是有个输入框和一个按钮，就可以用这个实现。  
有人会问，这是固定在底部的，也不是滑出来的啊，往下看。  
想要弹出的话需要用`showBottomSheet`或者`showModalBottomSheet`方法，区别就是一个没有半透明遮罩层，一个有半透明遮罩层并且点击遮罩层会关闭。
```
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return new MaterialApp(
      home: Scaffold(
        body: Builder(
          builder: (BuildContext context){
            return Center(
              child: ListView(
                children: <Widget>[
                  MaterialButton(
                    child: Text('showBottomSheet'),
                    onPressed: () {
                      showBottomSheet(
                        context: context,
                        builder: (context) {
                          return Container(
                            height: 100,
                            color: Colors.red,
                            child: Center(
                              child: Text('bottomSheet')
                            )
                          );
                        }
                      );
                    }
                  ),
                  MaterialButton(
                    child: Text('showModalBottomSheet'),
                    onPressed: () {
                      showModalBottomSheet(
                        context: context,
                        builder: (context) {
                          return Container(
                            height: 100,
                            color: Colors.blue,
                            child: Center(
                                child: Text('bottomSheet')
                            )
                          );
                        }
                      );
                    },
                  )
                ],
              )
            );
          }
        )
      )
    );
  }
}
```
![](24.gif)
这里需要注意一点就是`body`需要用`Builder`包一下，否则会找不到`Scaffold`。
## backgroundColor
背景色。
## resizeToAvoidBottomPadding & resizeToAvoidBottomInset
当键盘弹出时是否重新绘制以避免输入框被遮挡，默认为`true`。`resizeToAvoidBottomPadding`已弃用，目前还可以用。
```
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return new MaterialApp(
      home: Scaffold(
        resizeToAvoidBottomPadding: true,
        body: ListView(
          children: <Widget>[
            Container(
              height: 400,
              color: Colors.blue
            ),
            TextField(),
            Container(
              height: 100,
              color: Colors.red
            )
          ],
        )
      )
    );
  }
}
```

![](25.gif)
## primary
和`appBar`的`primary`类似。
## extendBody
如果设为`true`，当有`bottomNavigationBar`或者`bottomSheet`时`body`会延伸到下方，也就是说有一部分内容可能会被遮挡。
## extendBodyBehindAppBar
与`extendBody`类似，是否延伸到`appBar`下方。



