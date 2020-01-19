---
title: flutter--官方示例 & 代码解读
date: 2020-01-05
tags:
---
语法的东西就不详细说了，可以看官方文档了解一下[https://www.dartcn.com/](https://www.dartcn.com/)，学习过程中也会介绍一些。
<!--more-->
环境搭好，怎么搭看官网，很详细，并且为中国用户提供了解决方案
```
flutter help
```
可以看见官方提供的命令。接下来创建个项目：
```
flutter create mypp
```
创建好之后进入`myapp`先看一下模拟器（android stidio里边自己下）
```
➜  myapp flutter emulators    //查看可用的模拟器
```
![image.png](1.png)
我这里有两个，运行android模拟器
```
➜ flutter emulators --launch Nexus_5X_API_28
```
模拟器跑起来之后，查看一下可用设备
```
flutter devices
```
![image.png](2.png)
接下来把项目跑起来
```
//只有一个可用设备
flutter run
//如果有多个设备需要指定id
flutter run -d emulator-5554
```
![image.png](3.png)

![image.png](4.png)
可以看到项目已经跑起来了
命令直接按`r`热重载，`R`重启项目。
按`h`可以查看更多命令。
![image.png](5.png)
我们来看一下代码。
```
import 'package:flutter/material.dart';  //引入material风格的ui库

//每个dart项目都要有一个main函数
//void表示空类型，写到函数前边代表这个函数没有返回值
//这里用了dart语法的箭头函数
void main() => runApp(MyApp());

//main函数跑的MyApp在这里定义  继承了无状态Widget（相当于react的组件）
class MyApp extends StatelessWidget {
  @override    //@元数据  为代码添加额外的信息  @override代表下边这个是一个覆写超类的函数
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Demo',
      //app的主题
      theme: ThemeData(
        //这里给了蓝色，可以改成red、green、yellow等其他颜色然后在命令行按r看一下效果
        primarySwatch: Colors.blue,
      ),
      //路由主页
      home: MyHomePage(title: 'Flutter Demo Home Page'),
    );
  }
}

//创建主页  这是一个有状态组件
class MyHomePage extends StatefulWidget {
  //接收一个参数title
  MyHomePage({Key key, this.title}) : super(key: key);
  final String title;
  //有状态组件还需要一个State类
  @override
  _MyHomePageState createState() => _MyHomePageState();
}

//有状态组件的State类
class _MyHomePageState extends State<MyHomePage> {
  //状态
  int _counter = 0;
  //声明一个改变状态的函数
  void _incrementCounter() {
    //和react的setState作用一样
    setState(() {
      _counter++;
    });
  }

  @override
  Widget build(BuildContext context) {
    //这里有个Scaffold组件，先不用管干啥的，介绍组件的时候会详细介绍
    return Scaffold(
      //主页的标题栏
      appBar: AppBar(
        //传入的title
        title: Text(widget.title),
      ),
      //主页内容，Center组件里边的内容水平垂直居中，以后再详细介绍
      body: Center(
        //Column的子组件垂直排列
        child: Column(
          //主轴上的对其方式，这里是居中
          mainAxisAlignment: MainAxisAlignment.center,
          //多个子组件用children，单个子组件用child（有child的组件，不一定有children，有的组件只可以有一个子组件）
          //这里是一个泛型数组里只能放Widget
          children: <Widget>[
            //文本组件，可以设置文字样式
            Text(
              'You have pushed the button this many times:',
            ),
            Text(
              //相当于js里边的`${_counter}`
              '$_counter',
              //文本样式
              style: Theme.of(context).textTheme.display1,
            ),
          ],
        ),
      ),
      //ui库提供的浮动按钮
      floatingActionButton: FloatingActionButton(
        onPressed: _incrementCounter,
        //长按提示
        tooltip: 'Increment',
        child: Icon(Icons.add),
      ),
    );
  }
}
```
差不多就这些东西吧，详细的东西放到后边，这里先介绍一下结构。
