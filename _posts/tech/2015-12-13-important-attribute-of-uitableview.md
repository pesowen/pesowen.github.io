---
layout: post
title: UITableView的重要属性
category: 技术
tags: iOS Developer

---

> UITableView是iOS开发中最常见的控件之一, 搞清楚它的各种属性对我们日常开发非常有帮助. 

# tableView的内容包括以下三个部分

- cell
- tableHeaderView/tableFooterView
- sectionHeader/sectionFooter

---

# tableView的几个重要属性


## contentSize.height

- 内容的高度


## contentOffset.y

- 内容的偏移量
- 也就是(frame的顶部 - content的顶部)

## contentInset

- 内边距
- 也就是内容周围的间距


## frame

- frame.size.height :  矩形框的高度
- frame: 以父控件内容的左上角为坐标原点 

---


# 下面分为没有额外子控件和有额外子控件两种不同的情况来研究这些属性具体代表的意义

## 1. 没有额外子控件的情况

### 情况1


![image](http://wx2.sinaimg.cn/large/007ozevdgy1fwx2vgzun2j32801e0tew.jpg)

### 情况2

![image](http://ws4.sinaimg.cn/large/007ozevdgy1fwx2vskmjcj32801e0gq7.jpg)


### 情况3

![image](http://ws1.sinaimg.cn/large/007ozevdgy1fwx2vytbchj32801e00y0.jpg)


### 情况4

![image](http://wx1.sinaimg.cn/large/007ozevdgy1fwx2w4kwegj32801e079n.jpg)


### 情况5

![image](http://wx4.sinaimg.cn/large/007ozevdgy1fwx2wa6xlbj32801e0dld.jpg)


### 情况6

![image](http://wx3.sinaimg.cn/large/007ozevdgy1fwx2wgd0wwj32801e07aa.jpg)


---


## 2. 有额外子控件的情况

### 情况1

![image](http://ws4.sinaimg.cn/large/007ozevdgy1fwx2wox5v5j32801e0agm.jpg)


### 情况2

![image](http://wx3.sinaimg.cn/large/007ozevdgy1fwx2wvg2ugj32801e0jxf.jpg)


### 情况3

![image](http://ws1.sinaimg.cn/large/007ozevdgy1fwx2x2j1raj32801e0jxj.jpg)


### 情况4


![image](http://wx2.sinaimg.cn/large/007ozevdgy1fwx2x87cu5j32801e0agi.jpg)
