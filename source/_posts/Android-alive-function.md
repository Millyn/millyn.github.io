---
title: Android生命周期的理解方式
date: 2017-06-23 00:15:47
categories: Android
tags:
- Android
- live
- 生命周期
---

最近一直都在苦学 Android, 不得不说都是我啃了一遍 Java 基础之后现在反过头来看 Android 感觉能自己解决一些错误了,至少我明白错在哪里了.这是一件好事.

今天主要记录一下自己对 Android 生命周期管理的看法.

我相信网上写生命周期的一定很多人, 毕竟是Android 开发很重要的理念.

还是上图, Google 官方对生命周期的解释.

<!-- more -->

![image](https://developer.android.com/images/activity_lifecycle.png)

首先我们知道生命周期一共有
7种不同的生命周期.

- onCreate
- onStart
- onResume
- onPause
- onStop
- onRestart
- onDestroy

这 **7种** 生命周期代表了一个 Activity 的生命方式, 但在了解生命周期之前我们要知道 Android 的 Activity 是栈管理方式 .

栈管理在我现在的理解就一层一层的把 Activity 堆起来, 所以也有名词是”堆栈”.
我们启动2个 Activity 的时候, A 先显示 B 后显示,secondActivity 就到了栈顶(被显示)而 firstActivity 就到了 secondActivity栈的下面.

| Activity 名字 |栈级|视图|
|:--|:--|:--|
| secondActivity|栈顶|正在显示|
|firstActivity|栈内|隐藏|

这时候我们打开了thirdActivity, 那么栈内是这样的.

| Activity 名字 |栈级|视图|
|:--|:--|:--|
|thirdActivity|栈顶|正在显示|
|secondActivity|栈内|隐藏|
|firstActivity|栈内|隐藏|

新打开的栈会到栈的最顶上, 而上一次所显示的活动会被压下去一级.

如果说这时候我们点击返回键回到. secondActivity, 那么栈内层级又会出现这样改变, 因为我们从 **thirdActivity** 进行了返回按键( Android ) 那么根据生命周期的原理,
如果一个 Activity 不在被用户所见 那么他的生命周期会从 onResume( 正在显示 )–>onPause ( 暂停 )–>onStop( 不被显示 )–>onDestroy ( 销毁 )

___

和栈有关系的管理方式还有四种 Android 的活动启动模式。

- standard (默认)
- singleTop
- singleTask
- singInstance

### standard

在栈管理中,每一次有新的活动启动就会对活动创建一个新的实例, 例如我在 firstActivity 中在启动一次 firstActivity , 栈内就会出现两个 firstActivity, 而不是一个. 也就是说明你需要按2次返回键, 才会退出这个程序.

### singleTOP

standard 的启动模式明显是没有那么符合逻辑的, 因为我们日常的 app 里并没有说到要对某一个 Activity 疯狂的输出重复的实例 那么 singleTop 和 standard 的区别在于, 当属于 singleTop 的 firstActivity 在栈顶时, 启动 firstActivity 就不会像 standard 那样创建一个新的 firstActivity 的实例, 但 firstActivity 不在栈顶时, singleTop 还是会创建一个新的 firstActivity.
也就是说 singleTop 和 standard 的区别在于, singleTop 当 Activity 在栈顶时启动自己不会再创建一个新的实例,而 standard 是无论何时都会创建新的实例.

### singleTask

singleTask 就是解决以上两个启动模式里面重复创建实例的启动模式, 他会再你启动 Activity 时在栈内进行查找, 如果站内有即将要被启动的 Activity 时就会把这个 Activity 调到栈顶. 如果站内没有这个 Activity 才会重新创建, 减少了 重复创建 Activity实例.

### singleinstance

该启动模式和之前的三个启动模式概念完全不一样, 只要是被设置过 singleinstance 的启动模式的 Activity会启动一个新的返回栈来管理这个活动, 而新的返回栈的作用大多都在于程序之间互相调用时的栈管理问题, 我们不可能因为对方程序调用我们的某一活动时, 返回是需要一层一层的返回到退出程序, 而是有一个新的返回栈管理来针对这些程序之间的调用, 使返回时的层级更简单. singleinstance 我不是太了解, 可能说的太过含糊, 有兴趣的同学可以自己去查阅一下.

___

我在说 Activity 时先说明的是栈和启动模式上,因为我自己在理解生命周期时是先看的生命周期再看的栈和启动模式, 我觉得这个顺序不太好, 因为生命周期这个词的意思分为生命 // 周期 我们需要知道什么是生命, 什么是周期之后再来看生命周期的过程会更容易理解一点.

回到上面我们再看这个图, 就可以很明显的理解图中的意思了.

创建一个新的 Activity 时生命周期会经历
onCreate->onStart->onResume
展现到用户的手机界面上, 和用户进行交互过程.
如果用户在这个过程中, 通过交互打开了其他的 Activity, 那么之前的 Activity 就会进入到
onPause->onStop->onDestroy 这三步中, 至于会进入到哪一步, 跟用户的操作有绝大的关系 也跟系统管理回收机制有很大的关系.
在这个里面唯一没有提到的就是 onRestart, onRestart 是属于在 onStop的生命体时, 我们重新启动了这个 Activity, 我们就会通过 onRestart 进入到 onStart->onResume.

说了这么多生命周期, 那到底生命周期有什么用.
简单来说每个 Activity 的生命周期这七种, 系统都有回调方法, 也就是说我们在程序开发中可能会根据每种不同的生命周期做不同的事情, 可以再生命周期时进行交互工作, 比如我们知道用户要退出程序了, 可以提示他是否是误操作, 连续按2次 Back 我们才会退出.

也有可能是用户在未关闭程序时, 挂载到后台, 当从后台再次启动程序时 我们需要刷新程序里的数据 和网络进行通信工作, 为了做到更好的交互, 我们就可以再 onRestart 里做这些工作.

生命周期是每个 Android工程师必须理解的地方, 我也是一样, 我也在学习的过程中逐渐完善.
所以相当于写一篇自己的认知, 无论对错总要总结!