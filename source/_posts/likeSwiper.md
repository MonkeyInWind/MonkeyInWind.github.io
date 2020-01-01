---
title: 一个移动端翻页的封装
date: 2019-09-21 11:06:25
tags:
---
想写一个移动端的h5页面  
还不想用swiper那些工具，主要是太大，我需要的仅仅只是一个翻页功能  
所以自己写了一个  
<!--more-->
地址：https://github.com/MonkeyInWind/switching  
效果预览：https://monkeyinwind.github.io/switching/

下面说一下用法：
```
git clone https://github.com/MonkeyInWind/switching.git
cd switching
npm install
```
安装好之后

开发模式
```
npm run dev
```
打包
```
npm run build
```
先说一下功能：  
可以根据手滑动的八个方向切换界面  
左上、上、右上 、右  这四个方向是下一页  
左下、下、右下、左   这四个方向是上一页  
手滑向哪个方向  被滑走的界面就飞向哪个方向  
做了手指滑出屏幕判断  
切换界面会触发slide事件  可以做动画  
配置了babel和postcss  

用法：  
html结构：
```
<div class="main_container">
    <section style="background-color: red">
        <div class="div1"></div>
        <div class="div2">
            <img src="./src/img/aa.png" alt="">
        </div>
    </section>
    <section style="background-color: blue"></section>
    <section style="background-color: yellow"></section>
    <section style="background-color: green"></section>
  </div>
```
js:
```
var Switch = new Switching({
    target: '.main_container'
});
Switch.on('slide', function (e) {
    console.log(e);  //返回一个对象 属性包括当前界面的index   当前界面DOM节点  所有界面的DOM   滑动方向
});
```
