---
hot: "yes"
layout: post
title:  "如何写出不可维护的服务端程序"
date:   2014-03-03
category: work
---

<center>
<img src="http://7viirv.com1.z0.glb.clouddn.com/fragile.png" class="photo"></img>
</center>

『配置文件篇』

### 1. 配置文件一定要写不只一个

比如 `1.conf,2.conf,3.conf,...`
而且这n个配置文件一定要分散在不同的目录下。才能让别人部署移植你这个项目的时候永远也修改不完配置文件。

### 2. 配置文件的载入一定不要在项目初始化的时候载入

比如我们这个项目是一个服务，一定要在每次socket请求来临的时候，我们再去读取一遍配置文件，首先这样我们能显著降低本服务的运行效率（磁盘IO的速度你懂的）。

最关键的是能让别人部署完这个项目的时候，明明配置文件写错了，但是部署运行仍然没有问题，
直到外部请求进来的时候，这个程序才华丽的崩溃掉。让人类知道程序的崩溃是如此的防不胜防。

### 3. 配置文件的格式一定要惜墨如金，只写value不写key

比如在如下配置文件

```
192.168.0.1
10011
192.168.0.2
10012
192.168.0.3
10013
```

让别人去猜，这到底是个什么东西，依次到底是哪些调用。
你懂的，预测和猜测都是程序员的必备技能之一，都大数据时代了，没点算命的天赋你以后还怎么搞大数据分析和预测？！

### 4. 你写的服务要监听的端口一定不要写在配置文件里面，一定要写死在代码里面

这样，当别人部署你的项目的时候，改完了配置文件，很开心的启动之后发现报错退出。
哦，原来是端口已经被占用。那我修改个端口呗，怎么修改，少年，去慢慢看源代码吧。哈哈哈。


『日志篇』

### 5. 一定不要打日志

打个毛日志？哥在eclipse，vs里面都是直接单步调试，舒畅无比。
什么刚启动加载配置啊，配置文件找不到啊，配置所需要的端口被占用啊之类的错误，都一定不要打出日志。

什么？每次请求进来到处理完成，都要打一条INFO日志？烦不烦？

一定要让别人启动整个项目之后，可以看到明明在运行。但是让外界死活调用不了。
打开xxx.log 文件一看，空荡荡，只有一句淡淡的“service started.” 仿佛在诉说着什么。

### 6. 打日志一定不要暴露时间，文件名等关键信息

当然要写的模糊一点，要知道如果日志写的太清晰，程序一出错，别人就知道错在哪里。
别人就可以根据错误时间和文件名定位到你的错误代码，这样让你的代码多没面子。

最好是在出错的地方打出一行"here is wrong."，深藏功与名。
让接管或者部署你项目代码的人看得泪流满面。


『外部依赖』

### 7. 外部依赖一定不要包括进deps/之类的目录下

别人肯定以为拷贝了你整个目录的代码就可以运行起来，但是你显然不能让他们得逞。
要让他们一运行就报错，让他们知道你依赖了各种牛逼的库，这种库分布在linux各个匪夷所思的目录。

甚至你的账号家目录是`/home/zhangsan/`，你硬生生的依赖了`/home/lisi/`下的`xxx.jar`或者`yyy.hpp`文件，让他们慢慢找吧，幸福就在不远处。


### 8. 一定不要使用`git/svn`之类的版本控制软件

写代码多简单啊，不就是一个`x.cpp y.java`。更新代码就更简单了啊，
参照着x.cpp写呗，再来个x2.cpp，2太难听？那来个x_new.cpp。或者来个x_20130101.cpp更加夺目。

当整个目录下面全是各数字后缀，`y3.java, y4.java, y4s.java`之后，
什么？别人觉得这样丑爆了？你要反驳他们说：吵什么吵，iphone也是这么版本命名的好吗。
 
### 9. 一定不要在README.md里面写明项目的启动方式和条件

这样才能给你的代码加上一层防盗标志，保密程度直逼iphone5s的指纹识别。
没有你的启动命令，休想使用你的代码。


『总结』

看上去很荒谬，但是其实以上这些都是实实在在作者亲身遇到的。
累觉不爱，以此为鉴。
