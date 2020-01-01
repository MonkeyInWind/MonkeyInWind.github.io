---
title: 花瓣飘落效果
date:
tags:
---
先上效果：https://monkeyinwind.github.io/canvaspetal/index.html
github：https://github.com/MonkeyInWind/canvaspetal
<!--more-->
这个demo写了很久了，今天有时间简单写一下过程。  
用了react，这个不重要，随便用什么环境都可以。  
首先花瓣要有素材，随便搜了一下，切了几个出来。

![image.png](1.png)

# 1、在页面上添加一个canvas
整个页面只有一个canvas，我们需要这个canvas占满整个浏览器可视区，并且在浏览器窗口改变大小的时候依然和可视区大小相同，同时给canvas加个背景色。
![image.png](2.png)
这一步很简单没有什么需要说的。
# 2、在canvas上画一个花瓣
创建一个`createPetal`函数
```
createPetal() {
    let canvas = this.refs["canvas"];
    let ctx = canvas.getContext("2d");
    let img = new Image();
    img.src = require("./images/petal1.png");
    img.onload = () => {
      ctx.drawImage(img, 100, 100);
    }
  }
```
在`componentDidMount`调用
```
componentDidMount() {
    this.setCanvas();
    window.onresize = this.setCanvas;
    this.createPetal();
  }
```
这样就在`100， 100`这个位置画了个花瓣
![image.png](3.png)
# 3、让这个花瓣动起来
canvas动画是高频率刷新，清空上一帧，画下一帧，看起来是动画。
了解了动画的原理，接下来就可以开始写动画，首先将坐标放到`state`中。
```
this.state = {
      cw: 0,
      ch: 0,
      x: 0,
      y: 0
    }
```

创建一个`go`函数。
```
go(ctx, img) {
    ctx.clearRect(0, 0, this.state.cw, this.state.ch);//清空画布
    this.setState({
      x: this.state.x + 1,
      y: this.state.y + 1
    });//移动花瓣坐标
    ctx.drawImage(img, this.state.x, this.state.y);
    window.requestAnimationFrame(() => {
      this.go(ctx, img);
    });//重复清空画布，移动坐标重新画花瓣这个动作。
  }
```
在`createPetal`中调用。
```
createPetal() {
    let canvas = this.refs["canvas"];
    let ctx = canvas.getContext("2d");
    let img = new Image();
    img.src = require("./images/petal1.png");
    img.onload = () => {
      ctx.drawImage(img, this.state.x, this.state.y);
      this.go(ctx, img);
    }
  }
```
接下来可以看到效果。
![Feb-03-2019 10-16-19.gif](4.gif)
录的有点卡，实际上要比这个效果好很多。。。
有没有发现问题，花瓣位置超出浏览器之后去哪了打印一下坐标。
![image.png](5.png)
可以看到还在继续飘，这不是想要的，所以在坐标超出浏览器之后让它回到初始位置。
`go`这个函数修改如下：
```
go(ctx, img) {
    ctx.clearRect(0, 0, this.state.cw, this.state.ch);//清空画布
    let x = this.state.x + 1;
    let y = this.state.y + 1;
    if (x > this.state.cw || y > this.state.ch) {
      x = 0;
      y = 0;
    }
    this.setState({
      x,
      y
    });//移动花瓣坐标
    ctx.drawImage(img, this.state.x, this.state.y);
    window.requestAnimationFrame(() => {
      this.go(ctx, img);
    });//重复清空画布，移动坐标重新画花瓣这个动作。
  }
```
看一下效果
![Feb-03-2019 10-24-14.gif](6.gif)

这一步实现之后，有没有发现还有问题，要模拟自然飘落，这个花瓣不可能没有旋转，接下来再加上旋转。
这个旋转，需要的是画布旋转，旋转画好了之后再复位。
在`state`中加上旋转角度：
```
this.state = {
      cw: 0,
      ch: 0,
      x: 0,
      y: 0,
      r: 0   //旋转角度
    }
```
在`go`里面加上旋转，并且为了统一动作和计算方便，这里将图片位移改为画布位移，画图坐标相对画布始终在同一位置：
```
go(ctx, img) {
    ctx.clearRect(0, 0, this.state.cw, this.state.ch);//清空画布
    let x = this.state.x + 1;
    let y = this.state.y + 1;
    let r = this.state.r + 0.1;
    if (x > this.state.cw || y > this.state.ch) {
      x = 0;
      y = 0;
    }
    this.setState({
      x,
      y,
      r
    });//移动花瓣坐标
    ctx.save();//保存画布当前状态
    ctx.translate(this.state.x, this.state.y); //改为画布位移
    ctx.rotate(this.state.r);   //画布旋转
    ctx.drawImage(img, 0, 0);  //画图坐标始终在画布左上角
    ctx.restore();
    window.requestAnimationFrame(() => {
      this.go(ctx, img);
    });//重复清空画布，移动坐标重新画花瓣这个动作。
  }
```

![Feb-03-2019 10-48-19.gif](7.gif)
和预想的不太一样，这是因为画布默认的旋转中心为左上角，
我们需要将旋转中心移到图片的中心。
```
go(ctx, img) {
    ctx.clearRect(0, 0, this.state.cw, this.state.ch);//清空画布
    let w = img.width;
    let h = img.height;
    let x = this.state.x + 1;
    let y = this.state.y + 1;
    let r = this.state.r + 0.1;
    if (x > this.state.cw || y > this.state.ch) {
      x = 0;
      y = 0;
    }
    this.setState({
      x,
      y,
      r
    });//移动花瓣坐标
    ctx.save();//保存画布当前状态
    ctx.translate(this.state.x + w / 2, this.state.y + h / 2); //改为画布位移
    ctx.rotate(this.state.r);
    ctx.drawImage(img, -w / 2, - h / 2);  //画图坐标始终在画布左上角
    ctx.restore();
    window.requestAnimationFrame(() => {
      this.go(ctx, img);
    });//重复清空画布，移动坐标重新画花瓣这个动作。
  }
```
看一下效果
![Feb-03-2019 10-55-18.gif](8.gif)
旋转是有了，
但是好像不太对，只绕Z轴旋转，要让它变成3D旋转，这里要用到缩放`scale`，缩放这里不可能一直放大或者缩小，所以还要加一个变量控制。
```
this.state = {
      cw: 0,
      ch: 0,
      x: 0,
      y: 0,
      r: 0,
      scale: 1,
      toLarge: true
    }
```
接下来将`go`改一下，加上`scale`并且旋转速度调整一下：
```
go(ctx, img) {
    ctx.clearRect(0, 0, this.state.cw, this.state.ch);//清空画布
    let w = img.width;
    let h = img.height;
    let x = this.state.x + 1;
    let y = this.state.y + 1;
    let r = this.state.r + 0.05;
    let scale = this.state.scale;
    let toLarge = this.state.toLarge;

    if (scale >= 1) {
      toLarge = false;
    } else if (scale <= 0) {
      toLarge = true;
    }//这里根据scale大小设置toLarge

    if (toLarge) {
      scale += 0.01;
    } else {
      scale -= 0.01;
    }//这里根据toLarge更改scale值
    if (x > this.state.cw || y > this.state.ch) {
      x = 0;
      y = 0;
    }
    this.setState({
      x,
      y,
      r,
      scale,
      toLarge
    });//移动花瓣坐标
    ctx.save();//保存画布当前状态
    ctx.translate(this.state.x + w / 2, this.state.y + h / 2); //改为画布位移
    ctx.rotate(this.state.r);
    ctx.scale(1, this.state.scale);
    ctx.drawImage(img, -w / 2, - h / 2);  //画图坐标始终在画布左上角
    ctx.restore();
    window.requestAnimationFrame(() => {
      this.go(ctx, img);
    });//重复清空画布，移动坐标重新画花瓣这个动作。
  }
```
看一下效果
![Feb-03-2019 11-13-14.gif](9.gif)
至此一个花瓣就写完了。
但是我们想要的是很多个花瓣同时飘。
这就需要一个花瓣的类。
# 4、创建一个花瓣的class
新建一个`petal.js`
```
export default class Petal {
  constructor(w, h) {
    this.canvasW = w;  //canvas宽
    this.canvasH = h;  //canvas高
    this.w = 0;        //花瓣宽
    this.h = 0;        //花瓣高
    this.x = 0;        //初始x坐标
    this.y = 0;        //初始y坐标
    this.r = 0;        //初始旋转角度
    this.scale = 1;    //初始缩放
    this.toLarge = false;   //默认放大为false
    this.speedX = 1;   //x方向速度
    this.speedY = 1;   //y方向速度
    this.speedScale= 0.01  //缩放速度
    this.speedR = 0.05    //旋转速度
  }
  //数据初始化，用于当花瓣超出浏览器可视区时重置位置
  init() {
    this.x = 0;
    this.y = 0;
    this.r = 0;
    this.scale = 1;
    this.speedX = 1;
    this.speedY = 1;
    this.speedScale = 0.01;
    this.speedR = 0.05;
  }
  //画布位移、画图、画布复位
  draw(ctx, img) {
    this.w = img.width;
    this.h = img.height;
    ctx.save();     //保存当前画布状态
    ctx.translate(this.x + this.w / 2,  this.y + this.h / 2);  //画布位移
    ctx.rotate(this.r);   //画布旋转
    ctx.scale(1, this.scale);  //画布缩放
    ctx.drawImage(img, -this.w / 2, -this.h / 2);   //画图
    ctx.restore();    //画布复位
  }
  //计算坐标
  move() {
    this.x += this.speedX;
    this.y += this.speedY;
    this.r += this.speedR;
    if (this.scale >= 1) {
      this.toLarge = false;
    } else if (this.scale <= 0) {
      this.toLarge = true;
    }

    if (this.toLarge) {
      this.scale += this.speedScale;
    } else {
      this.scale -= this.speedScale;
    }

    if (this.x >= this.canvasW || this.y >= this.canvasH) {
      this.init();
    }
  }
}

```
在`App.js`内引入并new一个花瓣，打印一下；
```
import React, { Component } from 'react';
import './App.css';
import Petal from './petal';

class App extends Component {
  constructor(props) {
    super(props);

    this.state = {
      cw: 0,
      ch: 0,
      x: 0,
      y: 0,
      r: 0,
      scale: 1,
      toLarge: true
    }

    this.setCanvas = this.setCanvas.bind(this);
    this.componentDidMount = this.componentDidMount.bind(this);
    this.createPetal = this.createPetal.bind(this);
    this.go = this.go.bind(this);
  }

  setCanvas() {
    let W = document.documentElement.clientWidth;
    let H = document.documentElement.clientHeight;
    this.setState({
      cw: W,
      ch: H
    });
  }

  createPetal() {
    let canvas = this.refs["canvas"];
    let ctx = canvas.getContext("2d");
    let img = new Image();
    img.src = require("./images/petal1.png");
    img.onload = () => {
      //ctx.drawImage(img, this.state.x, this.state.y);
      // this.go(ctx, img);
      let petal = new Petal(this.state.cw, this.state.ch);
      console.log(petal);
    }
  }

  go(ctx, img) {
    ctx.clearRect(0, 0, this.state.cw, this.state.ch);//清空画布
    let w = img.width;
    let h = img.height;
    let x = this.state.x + 1;
    let y = this.state.y + 1;
    let r = this.state.r + 0.05;
    let scale = this.state.scale;
    let toLarge = this.state.toLarge;

    if (scale >= 1) {
      toLarge = false;
    } else if (scale <= 0) {
      toLarge = true;
    }

    if (toLarge) {
      scale += 0.01;
    } else {
      scale -= 0.01;
    }
    if (x > this.state.cw || y > this.state.ch) {
      x = 0;
      y = 0;
    }
    this.setState({
      x,
      y,
      r,
      scale,
      toLarge
    });//移动花瓣坐标
    ctx.save();//保存画布当前状态
    ctx.translate(this.state.x + w / 2, this.state.y + h / 2); //改为画布位移
    ctx.rotate(this.state.r);
    ctx.scale(1, this.state.scale);
    ctx.drawImage(img, -w / 2, - h / 2);  //画图坐标始终在画布左上角
    ctx.restore();
    window.requestAnimationFrame(() => {
      this.go(ctx, img);
    });//重复清空画布，移动坐标重新画花瓣这个动作。
  }

  componentDidMount() {
    this.setCanvas();
    window.onresize = this.setCanvas;
    this.createPetal();
  }

  render() {
    return (
      <div className="App">
      <canvas id="canvas" ref="canvas" width={this.state.cw} height={this.state.ch}></canvas>
      </div>
    );
  }
}

export default App;
```
![image.png](10.png)
可以看见已经创建了一个初始的花瓣，暂时还没有画图片。
接下来就是把之前的go改一下，画上花瓣并动起来。
`App.js`更改后如下：
```
import React, { Component } from 'react';
import './App.css';
import Petal from './petal';

class App extends Component {
  constructor(props) {
    super(props);

    this.state = {
      cw: 0,
      ch: 0
    }

    this.setCanvas = this.setCanvas.bind(this);
    this.componentDidMount = this.componentDidMount.bind(this);
    this.createPetal = this.createPetal.bind(this);
    this.go = this.go.bind(this);
  }

  setCanvas() {
    let W = document.documentElement.clientWidth;
    let H = document.documentElement.clientHeight;
    this.setState({
      cw: W,
      ch: H
    });
  }

  createPetal() {
    let canvas = this.refs["canvas"];
    let ctx = canvas.getContext("2d");
    let img = new Image();
    img.src = require("./images/petal1.png");
    img.onload = () => {
      let petal = new Petal(this.state.cw, this.state.ch);
      this.go(ctx, petal, img);
    }
  }

  go(ctx, petal, img) {
    let W = this.state.cw;
    let H = this.state.ch;
    //浏览器窗口改变大小时同步更新petal的cnavas宽高值，与花瓣坐标对比判断是否在可视区内
    petal.canvasW = W;
    petal.canvasH = H;
    ctx.clearRect(0, 0, this.state.cw, this.state.ch);//清空画布
    petal.move();
    petal.draw(ctx, img);

    window.requestAnimationFrame(() => {
      this.go(ctx, petal, img);
    });//重复清空画布，移动坐标重新画花瓣这个动作。
  }

  componentDidMount() {
    this.setCanvas();
    window.onresize = this.setCanvas;
    this.createPetal();
  }

  render() {
    return (
      <div className="App">
      <canvas id="canvas" ref="canvas" width={this.state.cw} height={this.state.ch}></canvas>
      </div>
    );
  }
}

export default App;
```
# 5、很多花瓣
一个花瓣已经完成了，接下来就是很多个花瓣。
这里涉及到几个点：
1、img的src不能用变量，所以要用字符串拼接变量的形式。
2、一个花瓣用了onload，很多花瓣很明显一个onload已经不能满足了，这里用`promise.all`。
3、创建很多花瓣，并不是每次`drawImage`都需要`clearRect`，需要在第0个画之前清空canvas。
4、关于初始坐标和初始速度，很多个花瓣就需要随机坐标和随机速度，而且初始化所在的区域需要计算，否则会出现花瓣位移过程中不经过浏览器可视区或者分布不均。
##### img的src
在`state`里加上花瓣数组，这里不能带后缀。
```
this.state = {
      cw: 0,
      ch: 0,
      n: 60,   //所要创建的花瓣数量
      imgnames: [
        "petal1",
        "petal2",
        "petal3",
        "petal4",
        "petal5",
        "petal6",
        "petal7",
        "petal8"
      ]
    }
```
`createPetal`函数改一下，创建多个img：
```
createPetal() {
    let canvas = this.refs["canvas"];
    let ctx = canvas.getContext("2d");
    // let img = new Image();
    // img.src = require("./images/petal1.png");
    let totalNum = this.state.imgnames.length; //图片的总数量
    for (let i = 0; i < this.state.n; i++) {
      let imgname = this.state.imgnames[i % totalNum];
      let img = new Image();
      img.src = require(`./images/${imgname}.png`);
      console.log(img)
    }

    // img.onload = () => {
      // let petal = new Petal(this.state.cw, this.state.ch);
      // this.go(ctx, petal, img);
    // }
  }
```
打印出60个img，src为base64；
##### 所有图片onload
这里把单个img的load封装为`promise`，添加到一个数组里，然后用`promise.all`
新建一个`imgLoad`函数，返回一个load的`promise`;
新建一个allImgLoad函数，用于返回一个`promise.all`。
```
imgLoad(imgname) {
    return new Promise((resolve, reject) => {
      try {
        let img = new Image();
        img.src = require(`./images/${imgname}.png`);
        img.onload = () => {
          resolve(img);
        }
      } catch(e) {
        reject(e);
      }
    });
  }

  allImgLoad(imgnames) {
    let p = [];
    for(let i = 0; i < imgnames.length; i++) {
      p.push(this.imgLoad(imgnames[i]));
    }
    return Promise.all(p).then(res => {
      return res;
    }).catch((e) => {
      console.log(e);
    });
  }

  async createPetal() {
    let canvas = this.refs["canvas"];
    let ctx = canvas.getContext("2d");
    // let img = new Image();
    // img.src = require("./images/petal1.png");
    let imgnames = [];
    let totalNum = this.state.imgnames.length; //图片的总数量
    for (let i = 0; i < this.state.n; i++) {
      let imgname = this.state.imgnames[i % totalNum];
      imgnames.push(imgname);
    }
    let imgs = await this.allImgLoad(imgnames);
    console.log(imgs)
  }
```
可以看到打印出了60个img
![image.png](11.png)
##### 很多花瓣
每一帧画60个花瓣，并且在第0个画之前清空画布，如果每画一个都清空一次，会把前59个都清空，画布上只有最后一个。
在`Petal`类里边的`move`和`init`用异步，加个`async`，否则会出现有的花瓣跳帧或者init的时候花瓣突然出现在屏幕上。
```
export default class Petal {
  constructor(w, h) {
    this.canvasW = w;  //canvas宽
    this.canvasH = h;  //canvas高
    this.w = 0;        //花瓣宽
    this.h = 0;        //花瓣高
    this.x = 0;        //初始x坐标
    this.y = 0;        //初始y坐标
    this.r = 0;        //初始旋转角度
    this.scale = 1;    //初始缩放
    this.toLarge = false;   //默认放大为false
    this.speedX = 1;   //x方向速度
    this.speedY = 1;   //y方向速度
    this.speedScale= 0.01  //缩放速度
    this.speedR = 0.05    //旋转速度
  }
  //数据初始化，用于当花瓣超出浏览器可视区时重置位置
  async init() {
    this.x = 0;
    this.y = 0;
    this.r = 0;
    this.scale = 1;
    this.speedX = 1;
    this.speedY = 1;
    this.speedScale = 0.01;
    this.speedR = 0.05;
  }
  //画布位移、画图、画布复位
  draw(ctx, img) {
    this.w = img.width;
    this.h = img.height;
    ctx.save();     //保存当前画布状态
    ctx.translate(this.x + this.w / 2,  this.y + this.h / 2);  //画布位移
    ctx.rotate(this.r);   //画布旋转
    ctx.scale(1, this.scale);  //画布缩放
    ctx.drawImage(img, -this.w / 2, -this.h / 2);   //画图
    ctx.restore();    //画布复位
  }
  //计算坐标
  async move() {
    this.x += this.speedX;
    this.y += this.speedY;
    this.r += this.speedR;
    if (this.scale >= 1) {
      this.toLarge = false;
    } else if (this.scale <= 0) {
      this.toLarge = true;
    }

    if (this.toLarge) {
      this.scale += this.speedScale;
    } else {
      this.scale -= this.speedScale;
    }

    if (this.x >= this.canvasW || this.y >= this.canvasH) {
      await this.init();
    }
  }
}

```

```
import React, { Component } from 'react';
import './App.css';
import Petal from './petal';

class App extends Component {
  constructor(props) {
    super(props);

    this.state = {
      cw: 0,
      ch: 0,
      n: 60,
      imgnames: [
        "petal1",
        "petal2",
        "petal3",
        "petal4",
        "petal5",
        "petal6",
        "petal7",
        "petal8"
      ]
    }

    this.setCanvas = this.setCanvas.bind(this);
    this.componentDidMount = this.componentDidMount.bind(this);
    this.createPetal = this.createPetal.bind(this);
    this.go = this.go.bind(this);
    this.imgLoad = this.imgLoad.bind(this);
    this.allImgLoad = this.allImgLoad.bind(this);
  }

  setCanvas() {
    let W = document.documentElement.clientWidth;
    let H = document.documentElement.clientHeight;
    this.setState({
      cw: W,
      ch: H
    });
  }

  imgLoad(imgname) {
    return new Promise((resolve, reject) => {
      try {
        let img = new Image();
        img.src = require(`./images/${imgname}.png`);
        img.onload = () => {
          resolve(img);
        }
      } catch(e) {
        reject(e);
      }
    });
  }

  allImgLoad(imgnames) {
    let p = [];
    for(let i = 0; i < imgnames.length; i++) {
      p.push(this.imgLoad(imgnames[i]));
    }
    return Promise.all(p).then(res => {
      return res;
    }).catch((e) => {
      console.log(e);
    });
  }

  async createPetal() {
    let canvas = this.refs["canvas"];
    let ctx = canvas.getContext("2d");
    let imgnames = [];
    let totalNum = this.state.imgnames.length; //图片的总数量
    for (let i = 0; i < this.state.n; i++) {
      let imgname = this.state.imgnames[i % totalNum];
      imgnames.push(imgname);
    }
    let imgs = await this.allImgLoad(imgnames);
    if(!imgs) return;
    for(let i = 0; i < imgs.length; i++) {
      let petal = new Petal(canvas.width, canvas.height);
      this.go(ctx, petal, imgs[i], i);
    }
  }

  async go(ctx, petal, img, index) {
    let W = this.state.cw;
    let H = this.state.ch;
    //浏览器窗口改变大小时同步更新petal的cnavas宽高值，与花瓣坐标对比判断是否在可视区内
    petal.canvasW = W;
    petal.canvasH = H;
    if( index === 0) {
      ctx.clearRect(0, 0, W, H);//清空画布
    }
    await petal.move();
    petal.draw(ctx, img);

    window.requestAnimationFrame(() => {
      this.go(ctx, petal, img, index);
    });//重复清空画布，移动坐标重新画花瓣这个动作。
  }

  componentDidMount() {
    this.setCanvas();
    window.onresize = this.setCanvas;
    this.createPetal();
  }

  render() {
    return (
      <div className="App">
      <canvas id="canvas" ref="canvas" width={this.state.cw} height={this.state.ch}></canvas>
      </div>
    );
  }
}

export default App;
```
这个时候60个花瓣叠在一起，看一下效果
![Feb-03-2019 14-34-22.gif](12.gif)

##### 随机初始化
首先要确定一下花瓣初始化的随机区域，有以下几点要求。
1、除了打开页面或者刷新页面，可以出现在浏览器可视区，其他情况下要出现在可视区外，从可视区边缘飘进可视区。
2、花瓣移动的路径要经过可视区，并且不会出现在左下角或者右上角只有半个花瓣划过的情况，没有意义。
3、分布均匀

接下来就是具体实施，先画个图，便于理解。
![image.png](13.png)

把浏览器45度向左上方平移，我们需要花瓣出现在两条红线之间的区域，并且当花瓣移出浏览器可视区之后，只能出现在蓝色斜线区域。
这里花瓣首先随机出现在整个大矩形里，如果出现在想要的区域外，我们做如下处理：
![image.png](14.png)
这样可以保证所有花瓣都会经过浏览器可视区，左下角和右上角不会出现半个花瓣的情况，并且均匀分布整个浏览器可视区。
移动端同理这里就不画图了下面上代码：
```
const randNum = (min, max) => {
  return Math.random() * (max - min) + min;
}

const calculateXY = (w, h) => {
  return new Promise((resolve, reject) => {
    let x = randNum(-h + 100, w - 100);
    let y = randNum(-h + 100, h - 100);
    let b = 60;   //这里是加一个偏移量，防止移出可视区后初始化位置时突然在可视区上边缘和做边缘出现。
    if (w >= h) {
      let a = w - h;
      //坐标在canvas区域，移到左上方同canvas大小区域
      if (x > -b && y > -b) {
        x = randNum(-h + b, a - b);
        y = randNum(-h + b, -b);
      } else if (x > a - b && y < -(h - (x - a) + b)) {
        //坐标在canvas右上方三角形区域，飘落不经过canvas，移到正上方三角形区域
        y = randNum(-(h - (x - a) + b), -b);
      } else if (x < -b && y > h + x - b) {
        //坐标在canvas左下方三角形区域，飘落不经过canvas，移到正左方三角形区域
        y = randNum(0, h + x - b);
      }
    } else {
      let a = h - w;
      if (x > -b && y > -b) {
        x = randNum(-w + b, -b);
        y = randNum(-w + b, a - b);
      } else if (x > -b && y < -(w - x) + b) {
        y = randNum(-(w - x) + b, -b);
      } else if (x < -b && y > h - x - b) {
        y = randNum(a, h - x - b);
      }
    }
    resolve({x, y});
  });
}

export default class Petal {
  constructor(w, h) {
    this.canvasW = w;
    this.canvasH = h;
    this.w = 0;
    this.h = 0;
    this.y = randNum(-h + 100, h - 100); //这里两个100是防止直接出现在可视区边缘半个直接飘出去了
    this.x = randNum(-h + 100, w - 100);
    this.r = Math.random();
    this.scale = -Math.random();
    this.toLarge = false;
    this.speedX = Math.random() * 0.5 + 0.5;
    this.speedY = this.speedX;
    this.speedScale = Math.random() * 0.007;
    this.speedR = Math.random() * 0.03;
  }

  draw(ctx, img) {
    this.w = img.width;
    this.h = img.height;
    ctx.save();
    ctx.translate(this.x + this.w / 2, this.y + this.h / 2);
    ctx.rotate(this.r);
    ctx.scale(1, this.scale);
    ctx.drawImage(img, -this.w / 2, -this.h / 2);
    ctx.restore();
  }

  async init() {
    let xy = await calculateXY(this.canvasW, this.canvasH);
    this.x = xy.x;
    this.y = xy.y;
    this.r = Math.random();
    this.scale = -Math.random();
    this.speedX = Math.random() * 0.5 + 0.3;
    this.speedY = this.speedX;
    this.speedScale = Math.random() * 0.004;
    this.speedR = Math.random() * 0.03;
  }

  async move() {
    this.x += this.speedX;
    this.y += this.speedY;
    this.r += this.speedR;
    if (this.scale >= 1) {
      this.toLarge = false;
    } else if (this.scale <= 0) {
      this.toLarge = true;
    }

    if (this.toLarge) {
      this.scale += this.speedScale;
    } else {
      this.scale -= this.speedScale;
    }

    if (this.x >= this.canvasW || this.y >= this.canvasH) {
      await this.init();
    }
  }
}
```
到这里就完成了，看一下帧数。
打开chrome开发者模式
![image.png](15.png)
选`rendering`，勾选`FPS meter`
![image.png](16.png)
可以看到在60左右，是比较理想的
![image.png](17.png)
