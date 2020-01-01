---
title: flutter for web
date: 2019-09-21 07:31:19
tags:
---
谷歌开发者大会上宣布flutter1.9正式发布，并且flutter_web已经合到dev合master。  
我们来体验一下。  
<!--more-->
首先切换到master并升级flutter到最新版本
```
flutter channel master
flutter upgrade
```
安装`webdev`
```
flutter pub global activate webdev
```
这里需要注意一下，安装之后看提示还要添加环境变量
```
Installed executable webdev.
Warning: Pub installs executables into $HOME/.pub-cache/bin, which is not on your path.
You can fix that by adding this to your shell's config file (.bashrc, .bash_profile, etc.):

  export PATH="$PATH":"$HOME/.pub-cache/bin"

Activated webdev 2.5.1.
```
打开`~ > .bash_profile`把`export PATH="$PATH":"$HOME/.pub-cache/bin"`添加进去，然后更新环境变量。
```
source ~/.bash_profile
```
到这`webdev`就完事了，命令行敲`webdev`测试一下
```
webdev
/Users/xxx/.pub-cache/bin/webdev: line 7: dart: command not found
```
惊不惊喜，意不意外，这是因为`dart`没有添加环境变量。
在`.bash_profile`中添加dart环境变量
```
export DART_HOME=/Users/xxx/sdk/flutter/bin/cache/dart-sdk/bin
export PATH=${DART_HOME}:${PATH}
```
刷新环境变量
```
source ~/.bash_profile
```
重新试一下`webdev`如果显示如下，说明没有问题
```
A tool to develop Dart web projects.

Usage: webdev <command> [arguments]

Global options:
-h, --help       Print this usage information.
    --version    Prints the version of webdev.

Available commands:
  build   Run builders to build a package.
  help    Display help information for webdev.
  serve   Run a local web development server and a file system watcher that rebuilds on changes.

Run "webdev help <command>" for more information about a command.
```
这里需要注意一下，如果没有用`flutter`自带的`dart-sdk`而是单独安装，这里可能会因为dart版本与flutter版本不匹配而出现如下提示
```
Can't load Kernel binary: Invalid kernel binary format version.
No active package webdev.
```
出现这种情况把dart卸载
```
brew uninstall dart
```
然后如前边所述将flutter内置的dart-sdk添加到环境变量就可以了。
启用`flutter_web`
```
flutter config --enable-web
```
出现如下提示
```
Setting "enable-web" value to "true".
```
接下来创建一个flutter项目
```
flutter create myapp
cd myapp
```
可以看见目录下多了一个`web`文件夹里边是一个`index.html`，内容如下
```
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <title>myapp</title>
</head>
<body>
  <script src="main.dart.js" type="application/javascript"></script>
</body>
</html>
```
从这里可以看见，最终也是编译成js文件。
先看一下已连接的设备
```
➜  myapp flutter devices
3 connected devices:

macOS  • macOS  • darwin-x64     • Mac OS X 10.14.5 18F132
Chrome • chrome • web-javascript • Google Chrome 76.0.3809.132
Server • web    • web-javascript • Flutter Tools
```
这里合之前相比多了`Chrome`和`Server`

在chrome里跑一下项目
```
flutter run -d chrome
```
效果如下，一个熟悉的页面。
![image.png](1.png)
可以看见大多数都是自定义标签，当然也不全是自定义，比如中间那一行文本和数字就是`p`标签，关于渲染`flutter`用了[CSS Paint](https://developers.google.com/web/updates/2018/01/paintapi)（能不能打开看缘分），就是用css画图，挺有意思的api。
点`sources`，然后`command+o`
![image.png](2.png)
可以看见这里并不是js文件，而是dart文件。
选择`main.dart`，看到的就是`main.dart`的源码。
接下来说一下调试，直接用chrome的开发者工具查找DOM是比较困难的，这个时候需要`android Studio`，在`android Studio`中打开`myapp`，设备选择`chrome(web)`，点绿色的三角跑起来
![image.png](3.png)
在`View > Tool Windows`下选择`Flutter Inspector`
![image.png](4.png)
打开之后是这个样子
![image.png](5.png)
可以看到不知道多少个层级
![image.png](6.png)
这个按钮可以在页面上显示`widget`的边界
![image.png](7.png)
这个准星一样的按钮相当于浏览器的审查元素，点击之后页面左下角会出现一个放大镜，想要重新在页面上选择元素需要点击放大镜，也可以在`Inspector`之中直接选择，页面上对应的元素会高亮。
这里再说一下另一种方式，chrome内置了`Dart DevTools`
项目跑起来之后点下边这个按钮
![image.png](8.png)
chrome会弹出个新窗口
![image.png](9.png)
和`Flutter Insector`类似，但是更好用一点。

关于`dart`文件的调试，和js一样可以打断点
![image.png](10.png)

接下来是打包
```
➜  myapp flutter build web
Compiling lib/main.dart for the Web...                             26.4s
```
build结束后看一下`build > web`目录下
```
➜  web ls
assets           index.html       main.dart.js     main.dart.js.map
```
`dart`被编译成了js
