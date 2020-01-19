---
title: flutter--单选框Radio、RadioListTile
date: 2020-01-05 19:49:07
tags:
---
之前学习了复选框和开关，还有个单选框，这里看一下。
## 一、Radio
还是先看constructor
<!--more-->
```
Radio<T>({
  Key key,
  @required T value,
  @required T groupValue,
  @required ValueChanged<T> onChanged,
  Color activeColor,
  MaterialTapTargetSize materialTapTargetSize
})
```
这个Widget的constructor很简单，这几个属性前边都介绍过，怎么用看demo。
```
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return new MaterialApp(
      home: Scaffold(
        body: RadioDemo()
      )
    );
  }
}

enum SingingCharacter { lafayette, jefferson }

class RadioDemo extends StatefulWidget {
  @override
  _RadioDemoState createState() => new _RadioDemoState();
}

class _RadioDemoState extends State<RadioDemo> {
  SingingCharacter _character = SingingCharacter.lafayette;

  @override
  Widget build(BuildContext context) {
    return Container(
      padding: EdgeInsets.only(top: 20.0),
      child: Column(
        children: <Widget>[
          Radio(
            value: SingingCharacter.lafayette,
            groupValue: _character,
            activeColor: Colors.red,
            onChanged: (SingingCharacter value) {
              print(value);
              setState(() {
                _character = value;
              });
            },
          ),
          Radio(
            value: SingingCharacter.jefferson,
            groupValue: _character,
            onChanged: (SingingCharacter value) {
              print(value);
              setState(() {
                _character = value;
              });
            },
          )
        ]
      )
    );
  }
}
```
这里有点陌生的东西，接下来看一下。
### enum
枚举类型。  
官方文档挺详细的[http://dart.goodev.org/guides/language/language-tour](http://dart.goodev.org/guides/language/language-tour)
这里就不详细说了
### Column
这里简单介绍一下，后边再详细说，这个Widget可以简单的理解成列表类似于html里边的`ul`，子节点是`children`一组Widget。
### T
看constructor的时候应该就发现了，多了个`<T>`，这个是泛型，`<>`里边是数据类型，例如想定义一个`List`里边只能放`String`,可以这样写
```
var str = new List<String>();
```
这里`T`是备用类型，实际上就是一个占位符。

## 二、RadioListTile
```
const RadioListTile<T>({
  Key key,
  @required T value,
  @required T groupValue,
  @required ValueChanged<T> onChanged,
  Color activeColor,
  Widget title,
  Widget subtitle,
  bool isThreeLine: false,
  bool dense,
  Widget secondary,
  bool selected: false,
  ListTitleControlAffinity controlAffinity: ListTileControlAffinity.platform
})
```
demo
```
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return new MaterialApp(
      home: Scaffold(
        body: RadioDemo()
      )
    );
  }
}

enum SingingCharacter { lafayette, jefferson }

class RadioDemo extends StatefulWidget {
  @override
  _RadioDemoState createState() => new _RadioDemoState();
}

class _RadioDemoState extends State<RadioDemo> {
  SingingCharacter _character = SingingCharacter.lafayette;

  @override
  Widget build(BuildContext context) {
    return Center(
      child: Column(
        children: <Widget>[
          RadioListTile<SingingCharacter>(
            value: SingingCharacter.lafayette,
            title: Text('lafayette'),
            groupValue: _character,
            activeColor: Colors.red,
            onChanged: (SingingCharacter value) {
              print(value);
              setState(() {
                _character = value;
              });
            },
          ),
          RadioListTile<SingingCharacter>(
            value: SingingCharacter.jefferson,
            title: Text('jefferson'),
            groupValue: _character,
            onChanged: (SingingCharacter value) {
              print(value);
              setState(() {
                _character = value;
              });
            },
          )
        ]
      )
    );
  }
}
```
和单独的`Radio`相比基本上没啥区别。  
相对于`Radio`多出来的属性看这里：[复选框CheckBox、CheckboxListTile](/post/flutterCheckWidget)
`CheckboxList`里边已经详细说过了。
