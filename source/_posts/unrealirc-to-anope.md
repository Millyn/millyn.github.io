---
layout: title
title: Unrealircd 和 Anope 配置记录
date: 2018-09-04 17:12:38
tags:
- IRCD
- UnrealIrc
- Anope
- IRC
- TRPG
- 跑团
- DND
---

Unrealircd 是一款基于 C++ 的开源 IRCD 服务端应用。

Anope 同样也是一款 C++ 的开源 IRCD Module 类型应用，Anope 相当于 IRC 的扩展包，他自带了很多常用的 Module，便于管理 IRC 。

两款服务端程序按照文档跑起来很简单（如果你无法使他们跑起来，我建议你寻找了解部署的工程师朋友帮助你，因为参考文档完全可以使其跑起来，哦对了， 不要 Git Clone， 直接下载最新的发布包就行），但是互通的过程是有点麻烦的。

Unrealircd 和 Anope 都是独立运行的，但 Anope 运行时必须能够链接上配置好的 Unrealircd 服务器，所以启动顺序应该是先启动 Unrealircd（以下简称IRCD） 再启动 Anope。

OK，开始正题，有那些配置是官方文档没有说，但需要调整的。

<!-- more -->

## IRCD 中文昵称

1. 在 `conf/unrealircd.conf` 找到 `set {` 的开始行，在 set 这个块级配置下，增加参数
2. `allowed-nickchars { gbk };`
3. 原文参考 [WIKI](https://www.unrealircd.org/docs/Nick_Character_Sets "WIKI")
4. 原文里推荐的是 `chinese-simp; chinese-trad;` 个人觉得这2个配置实际上都是 GBK ，所以直接配置 GBK 是最好的。

## Anope 配置 IRCD 链接

处理这个，我觉得第一件事情应该是先理解，IRCD 和 Anope 怎么样产生链接。

我对 IRCD 了解不深，这一次处理这个业务主要是为了果园的 IRCD 服务重构直接上手学习的，Cpp 方面的代码我也只能说作为浅看，具体实现和一些底层方面的，我没有做过多了解。

### 如何产生链接

IRCD 需要新增一个 Link 配置，这个 Link 配置后，处于等待服务连接至`自己`的状态，也就是说 IRCD 需要公开一个自己被他人管理的接口，那么这里就通过 Link 来实现。

例子：
```
link anope.test.org {
	incoming {
		mask *;
	};
	
	outgoing {
		hostname 127.0.0.1;
		port 6667;
		options { ssl; authoconnect; };
	};
	
	password "yourpassword";
	hub *;
	class servers;
}
```

我是这样写的 Link ， 有几个需要注意的地方是，因为 Anope 和 IRCD 在一台主机上，所以 Hostname 为 127.0.0.1 ，只通过本地链接。
Port options hub 这三个参数，建议自己查阅官方文档理解，我就不作过多叙述，这里需要记录下来的就是 `hostname port password` 三个参数在 Anope 去设置参数。

### Anope 的配置

打开 Anope 应用根目录下的 `conf/services.conf`  找到 `uplink` 这个配置块（默认是存在的，需要修改 Value 就行）

修改这个 uplink 块级下面的 `host / port / password` 三个参数的值，和配置 IRCD 的 Link 为一致。

保存文件，启动 Anope 你应该会遇到报错，这个报错是提示你 Password 错了，不用担心，这并不是你 Password 错了，而是参数上的问题。

还是 Anope 的配置文件，找到
```
module
{
	name = "inspircd20"
}

```

如果你的 Anope 版本是官网最新的，那么应该是在 `263` 行。
修改 name 的值为 `unreal4` ，这个 `4` 的意思是指 unrealircd 版本为 4 开头，如果你的 IRCD 版本过低，是 3.2.x 版本的，那么修改为 `unreal` 即可

保存文件，重新尝试启动你的 Anope ，一切正常。

## Anope NickName Server Chars 设置

在之前，我们已经提到了 IRCD 允许中文昵称，但 Anope 还未调整（就算调整了，也不支持中文名注册，但我们必须要先调整。

`conf/services.conf` -> `nick_chars` 参数的值修改为 GBK 

## Anope NS 注册中文名

首先我们了解 NS 是 NickServ 这个服务，也就是昵称服务。
在 IRCD 独立运行的条件下，是没有 NickServ 这种功能的，是因为和 Anope 产生了链接，Anope 为 IRCD 提供了这个服务。

但在 Anope 里，NickServ 是一个 Module（模块），简单来说是一个 Anope 插件，也就是说，Anope 有成千上万个插件， NickServ 是其中一个。

OK，先说怎么使用插件

### Anope 插件使用

在 Anope 的 `conf` 文件夹下，有一个文件名为 `module.example.conf` 的文件，把这个文件 cp 一个去掉 `.example` 后缀的在当前目录下。

module.conf 就是管理 Anope Module 的配置文件，现在既然已经有了配置文件，那么插件就会生效。

重启 Anope 服务器，尝试一下指令，你会发现 /help 是可以输入的 但是昵称注册的命令是什么？你都不知道，我们还是到官方 [WIKI](https://wiki.anope.org/index.php/2.0/Modules/ns_register "WIKI") 看一下。

`/msg NickServ register MyPassword fred@example.com`

注册昵称，需要输入 password  和 邮箱，前面的都是必填的指令而已。

WIKI 的下方有 `Default Configuration` 默认配置，我们把这段配置进行复制。

然后重新打开 `module.conf` 文件，快速跳到文件尾部 ，把复制下来的配置，黏贴到文件里，保存。

重启你的 Anope，尝试输入命令，你会发现如果你当前使用的是英文名，那么 Anope 会提示你注册成功，但你是中文名，就会注册失败。

### 解决中文名注册

中文名注册失败这个问题，我的解决思路很不好，但由于时间问题，我没有去过多的研究这个问题。
但面向需求的方面，先解决问题。

我描述下我整体的解决思路。

1. 当中文名注册失败时，会有一串错误提示。
2. 我把错误提示复制下来，在 Anope 的 Github 进行 This Repo 的搜索，查看 Repo 下有那些文件出现了这些错误提示的`调用`
3. 调用中有一个函数命名非常的符合我们的目标 `IsNickValid` 
4. 看这个函数的解释 [Github](https://github.com/anope/anope/blob/368300d31990eeafc0c7835a21bc0f0835fed71b/src/protocol.cpp#L351 "Github") 

在函数上已经表明，nickname 只允许 letter digit special 三个参数下的内容，那么 GBK 肯定是不在这个范围里的，所以当我们用中文名注册时， IsNikeVaild 触发判断后，都会返回 False ，拒绝注册。

```
	/**
	 * RFC: defination of a valid nick
	 * nickname =  ( letter / special ) ( letter / digit / special / "-" )
	 * letter   =  A-Z / a-z
	 * digit    =  0-9
	 * special  =  [, ], \, `, _, ^, {, |, }
	 **/
```

到这里，我们已经找到了问题所在，就是 IsNickName 产生的问题，但我没有去修改这个函数让其支持 GBK 的验证，而是取巧了一个方法。

考虑到 IRCD 也会对用户的 NickName 做一次是否规范的检查，所以暂时不考虑因为非法的 NickName 会导致服务器崩盘。

找到 `modules/commands/ns_register.cpp` ns 注册插件的源文件，找到 Class `CommandNSRegister` 在下方会发现有一处调用了 `IsNickName` 把这一段代码注释掉 [Github](https://github.com/anope/anope/blob/368300d31990eeafc0c7835a21bc0f0835fed71b/modules/commands/ns_register.cpp#L169 "Github") ，然后重新编译一次 Anope ，让修改过的 Module 生效，然后再重启 Anope 服务器。

进入服务器，尝试中文 NickName 的注册。

一切大功告成。

## 结语

IRCD 是上古时期的聊天工具，现在还在被使用，证明 IRC 必然有他自己的魅力，可能你一辈子都不会去使用 IRC，但可以思考下我作为一个不懂 C++ 和不了解 IRC 的伪工程师，是如何解决 Nickname 这个问题的，虽然解决方法不太对，但定位 Bug 的思路我认为是没错的。

