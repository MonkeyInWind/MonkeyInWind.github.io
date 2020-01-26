---
title: electron 'unsafe-eval'
date: 2020-01-26 21:01:35
tags:
---
electron 出现如下警告
<!--more-->
```
Electron Security Warning (Insecure Content-Security-Policy) This renderer process has either no Content Security
    Policy set or a policy with "unsafe-eval" enabled. This exposes users of
    this app to unnecessary security risks.

For more information and help, consult
https://electronjs.org/docs/tutorial/security.
This warning will not show up
once the app is packaged.
```
在`main.js`（入口文件）添加一行代码
```
process.env['ELECTRON_DISABLE_SECURITY_WARNINGS'] = 'true';
```
