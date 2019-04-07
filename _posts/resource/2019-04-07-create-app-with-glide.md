---
layout: post
title: 五分钟创建一个手机App——Glide
category: 资源
tags: App Glide
keywords: Glide
description: 
---

## 前言

以往听到移动应用开发，我更多想到的是利用较为复杂的IDE(例如XCode)，使用高级语言（Objective-C或Swift）、各种库（JSONKit、AFNetworking）以及多种第三方框架（SDWebImage、MBProgressHUD），并且需要使用多种工具（git、wireshark等）以及申请开发者账号，才能制作一个基本的移动应用。最近发现的 Glide 服务真让我感到惊喜，它利用 Google Sheet 作为数据来源，只需要 5 分钟就能创建一个漂亮的、可以自动更新的手机应用。

## 什么是Glide服务

Glide 的服务是这样的：它能够将 Google 表格的内容按照设置的版面展示出来，只需要将 Glide 屏幕上需要显示的标题、图片、内容和 Google 表格中一一对应起来，就可以了：

![1](http://ww1.sinaimg.cn/large/007ozevdgy1g1udm6jzudj30my0fe7be.jpg)

## 制作及体验

让我们忘掉前言里提到的令人感到生涩的单词，甚至不用考虑屏幕适配的问题。直接打开[Glide官网](https://www.glideapps.com/)即可~

这部分我不会教你怎么制作，因为过程太简单，你需要做的只是用登录Google账号，然后将 Glide 屏幕上需要显示的标题、图片、内容等和 Google Sheet 表格中一一对应起来就行。

然后，一个简单的 App 就建好了，当然如果你愿意的话完全可以通过编辑Google Sheet、添加按钮、链接、视频等来使你的App看起来更丰富。通过访问 Glide 提供的网址，用手机浏览器可以创建快捷方式到桌面，然后就可以像普通的手机App一样使用了。甚至你也可以将该应用发布到应用市场。

![2](http://ww1.sinaimg.cn/large/007ozevdgy1g1udl7ia1qj312j0lon04.jpg)

Glide 官网提供了不少示例，比如有HR面试、员工信息、城市向导、运动队成员信息、播客App等：

![3](http://ww1.sinaimg.cn/large/007ozevdgy1g1udmgl9otj30on0lijwl.jpg)

我也随手创建了一个：[http://pesowen.glideapp.io](http://pesowen.glideapp.io)

![4](http://ww1.sinaimg.cn/large/007ozevdgy1g1ue63qtk8j30vz0izwgw.jpg)

这是多年前我收藏的一个日语学习的Excel表格，做成App之后看起来舒服多啦~

当然，Glide使用的是Google以及AWS的服务，所以在国内无法直接访问，聪明的朋友们你们可以想到办法的，对吧。毕竟比起前言提到的工具和语言，通过Glide创建App几乎是零成本的~

