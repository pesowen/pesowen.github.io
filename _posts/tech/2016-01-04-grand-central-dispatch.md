---
layout: post
title: Grand Central Dispatch
category: 技术
tags: iOS Developer

---

## Grand Central Dispatch (GCD)是Apple开发的一个多核编程的解决方法

### 两个核心概念

- 队列和任务
- 同步函数和异步函数
    
## GCD基本使用

|类型|特点|
|:--:|:--:|
|异步函数+并发队列|开启多条线程，并发执行任务|
|异步函数+串行队列|开启一条线程，串行执行任务|
|同步函数+并发队列|不开线程，串行执行任务|
|同步函数+串行队列|不开线程，串行执行任务|
|异步函数+主队列|不开线程，在主线程中串行执行任务|
|同步函数+主队列|不开线程，串行执行任务（注意死锁发生）(主线程会死锁, 子线程不会死锁)|

* 注意同步函数和异步函数在执行顺序上面的差异
* 异步函数具备开多条线程的能力, 具体开几条要看队列的类型
    
## GCD线程间通信

```objc
//0.获取一个全局的队列
    dispatch_queue_t queue = dispatch_get_global_queue(0, 0);

    //1.先开启一个线程，把下载图片的操作放在子线程中处理
    dispatch_async(queue, ^{

       //2.下载图片
        NSURL *url = [NSURL URLWithString:@"http://7xrwnk.com1.z0.glb.clouddn.com/IMG_5628.jpg"];
        NSData *data = [NSData dataWithContentsOfURL:url];
        UIImage *image = [UIImage imageWithData:data];

        NSLog(@"下载操作所在的线程--%@",[NSThread currentThread]);

        //3.回到主线程刷新UI
        dispatch_async(dispatch_get_main_queue(), ^{
           self.imageView.image = image;
           //打印查看当前线程
            NSLog(@"刷新UI---%@",[NSThread currentThread]);
        });

    });
```

## GCD其它常用函数

```objc
    01 栅栏函数（控制任务的执行顺序）
    dispatch_barrier_async(queue, ^{
        NSLog(@"--dispatch_barrier_async-");
    });

    02 延迟执行（延迟·控制在哪个线程执行）
      dispatch_after(dispatch_time(DISPATCH_TIME_NOW, (int64_t)(2.0 * NSEC_PER_SEC)), dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0), ^{
        NSLog(@"---%@",[NSThread currentThread]);
    });

    03 一次性代码（注意不能放到懒加载）
    -(void)once
    {
        //整个程序运行过程中只会执行一次
        //onceToken用来记录该部分的代码是否被执行过
        static dispatch_once_t onceToken;
        dispatch_once(&onceToken, ^{

            NSLog(@"-----");
        });
    }

    04 快速迭代（开多个线程并发完成迭代操作）
       dispatch_apply(subpaths.count, queue, ^(size_t index) {
    });

    05 队列组（同栅栏函数）
    //创建队列组
    dispatch_group_t group = dispatch_group_create();
    //队列组中的任务执行完毕之后，执行该函数
    dispatch_group_notify(dispatch_group_t group,
    dispatch_queue_t queue,
    dispatch_block_t block);
```

---

## 一些函数的使用

### 创建队列

```objc
//创建队列
//第一个参数: 标签, 队列的名称, C语言<"io.github.pesowen.download">
//第二个参数: 队列的类型<DISPATCH_QUEUE_CONCURRENT|DISPATCH_QUEUE_SERIAL>
dispatch_queue_t queue = dispatch_queue_create(<#const char *label#>, <#dispatch_queue_attr_t attr#>)
```

### 获取全局并发队列

```objc
//第一个参数: 优先级<>
//第二个参数: 留给未来使用的0
dispatch_queue_t queue = dispatch_get_global_queue(<#long identifier#>, <#unsigned long flags#>)
```

### 获得主队列

```objc
dispatch_queue_t queue = dispatch_get_main_queue();
```

### 显式的指明一个任务执行块进入了这个组

```objc
dispatch_group_enter();
```

### 显式表明执行块执行完毕，移除出组

```objc
dispatch_group_leave();
```

### 对于同步任务，这两个方法结合起来的效果等同于

```objc
dispatch_group_async();
```


