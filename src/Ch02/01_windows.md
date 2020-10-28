# windows
## 0x00 前言

最近入手了 SP6, 于是把 2015 年写的这篇文章修订为 2019 版

> 笔者已过了爱折腾的年纪，仅从提升工作效率方面来说。

背景：

0. Pythonista && Nodejs
1. 工作机 MBP 2017 款机器
2. 生活机 Surface Pro 6, 轻办公，有时也用来调试 Windows 上的程序。

本文目录

```
▼ 0x00 前言 : section
	0x01 文件整理 : section
▼ 0x02 自带功能 : section
		2.1 快捷键 : section
		2.2 触摸板 : section
		2.3 Win+R -- 运行 : section
▼ 0x03 必备软件 : section
		3.1 文件管理 : section
		3.2 资讯浏览 : section
		3.3 学习软件 : section
		3.4 播放器 : section
	0x04 Autohotkey : section
▼ 0x05 编程配置 : section
		5.1 Windows Subsystem Linux : section
		5.2 终端 : section
		5.3 Python : section
		5.4 Node : section
		5.5 其他 : section
	0x06 实用主义的工具论 : section
	0xEE 后续 : section
```

## 0x01 文件整理

文件整理一般从类型上进行划分子文件夹。

贴上我的几张图来看一下我的文件夹命名：

一级文件夹如下：
![](http://upload-images.jianshu.io/upload_images/52890-b6a5322beca48d2c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

二级文件夹如下：
![](http://upload-images.jianshu.io/upload_images/52890-656d90e16abaff66.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

三级或者三级以上文件夹

![](http://upload-images.jianshu.io/upload_images/52890-bdf7349fccedda52.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

文件命名规范（因为涉及到后面使用 Everything，所以我们的命名尽量追求便于搜索）

举个例子，对于读书笔记 ：读书笔记、_设计模式、_20150303\_v2.1.xmind

对于照片这种文件比较多的，优先命名文件夹，其次按照地址人物日期命名，比如：大明湖胖、_夏雨荷、_20150101

> 无需刻意追求命名，方便搜索，方便管理就好。

不妨参考下面文章：

[电脑上的文件夹该如何命名（整理）才能做到很久都不用重新整理的那种？ - 文件整理](http://www.zhihu.com/question/21537488)

由于 Windows 的文件没有标签系统。则可以从命名上强行打上标签，比如加上年份。

嗯这样你在搜索笔记的时候在 Everything 里面只需要键入 2015 笔记就可以查看 2015 笔记文件。

是不是很方便？

![](http://upload-images.jianshu.io/upload_images/52890-f4a815755900d627.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![](http://upload-images.jianshu.io/upload_images/52890-1fb3ac3766e59044.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

当然桌面上尽量少放或者不放文件夹，我的桌面上仅仅有一个链接到 OneDrive 里面的 TEMP 文件夹的快捷方式，用于存放临时没有整理的文件。

![](http://upload-images.jianshu.io/upload_images/52890-be22e9252b149d1f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 0x02 自带功能

### 2.1 快捷键

能看到本文的读者应该都可以搜索到这些东西。就不赘述了。

仅仅说一些重点快捷键。Win+ 数字键 把常用的软件附在任务栏上。

建议四个以内，方便单手操作。Win+X Alt+tab 切换窗口 Win+R 运行

### 2.2 触摸板

嗯，没有苹果的触摸板好用。但 Windows 的快捷键好用多了。

### 2.3 Win+R -- 运行

主要用于启动一些程序或者一些 DOS 小命令。

![](http://upload-images.jianshu.io/upload_images/52890-d39ea3f078f1d800.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我将所有的便携与非便携软件的快捷方式放在这里并且配置环境变量。

比如，我需要启动为知笔记，我就仅仅需要闭上眼睛输入 Win+R + wz +<Enter>

PS: 景观

其他软件同理。

```python
# sublime text 打开需要输入 
Win+R + st +<Enter>
# 欧路词典 打开需要输入 
Win+R + ol +<Enter>
# 这里省去若干软件打开方式。
```

关于 Win+R 你可以参考善用佳软 [最绿色最高效，用 win+r 启动常用程序和文档](http://xbeta.info/win-run.htm) 的介绍。

## 0x03 必备软件

### 3.1 文件管理

- 图片方面，如果不是 RAW 格式，可以考虑使用 Eagle 这个神器
- 文字方面，如果是简单的图文素材，可以直接丢到印象笔记 / 有道云笔记里。
- 文档管理，如果是比较 Geek 一些的话，可以考虑用 TotalCMD 管理文档，Everything 快速搜索

- PS: 没有足够的需求，不要搞 TC.
- PS2: 无坚不摧，为快不破。 everything 是搜索效率最快的软件。没有之一。

### 3.2 资讯浏览

- 浏览器 Chrome / 新版 Edge 浏览器
- 欧陆词典：可以外挂其他的开源词库，查词速度超级快。

### 3.3 学习软件

- Xmind
- OneNote
- Office

### 3.4 播放器

- PotPlayer
- foobar2000 -- 逼格提升必备

![](http://upload-images.jianshu.io/upload_images/52890-1d8bc57d69948d60.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- 网易云音乐 -- 这货真的不错。

## 0x04 Autohotkey

早年刚接触这个软件的时候，是为了在魔兽争霸里面快速输入文本....

但这个软件可以帮你少装很多软件，

比如：

1. 可以针对快捷键进行编程，
2. 节省大量的时间，比如一大段文字中有一个网址，你需要访问它，我的解决方法就是选中那些文字，然后一个 Win+b，直接打开 chrome 进行搜索，如果文字中没有网址，那么对选中文字进行百度。同理可以推淘宝，京东 github 等等。
3. 你需要大量的文本编辑，但是上下左右离自己的工作区比较远，你可以小拇指按住大写锁定按键，然后使用 HJKL 进行移动。

而且，作为曾经的 Geek , 不写点代码定制一下使用流程，你都不好意思说优雅。

教程参考 [AutoHotkey 之美 - 知乎专栏](http://zhuanlan.zhihu.com/autohotkey)

新手可以先拿我搜刮整理的 AHK 代码看看。[twocucao/ChortHotKey · GitHub](https://github.com/twocucao/ChortHotKey)

PS：AHK/TotalCMD 神级软件，都是入门容易精通难的深坑，想调教好也不是想象中呢么简单的，但，书到用时方恨少，你可以先挑一些使用。如果你以后有不少的文件需要管理，在未来，你一定会用到。

> PS: 站在 2019 年回头看，其实 AHK 的语法还是蛮糟糕的。相比于顺手打出很多快捷键。倒不如先优化自己的工作流程。甚至，很多 AHK 的场景完全可以用 Python 之类的语言来写。当我在 Windows 上的时候一味追求 APM, 即高效的操作，但其实最高效的还是自己的思路清晰，想好了再动手。

毕竟，AHK 作为脚本语言，确实语法不美，数据结构也少，没有主流编程语言的社区支持。

嗯，其实还是推荐学 Python

## 0x05 编程配置

### 5.1 Windows Subsystem Linux

如何配置 WSL （我死了？哈哈哈哈好逗的缩写）

https://zhuanlan.zhihu.com/p/49227132

https://zhuanlan.zhihu.com/p/47733615

### 5.2 终端

cmder 相当于 windows 上的 iterm

https://github.com/cmderdev/cmder

### 5.3 Python

https://www.anaconda.com/distribution/#download-section

由于笔者要调试 Win32 程序，而大部分这些 dll 文件都是在 32 位的情况下编译出来的。

### 5.4 Node

https://nodejs.org/en/download/

### 5.5 其他

- beyond compare 代码比对
- jetbrain
- vscode

## 0x06 实用主义的工具论

当你选择用一个工具的时候，务必把精力放在解决问题上。

能满足你的要求，就是好工具，须知『梅须逊雪三分白，雪却输梅一段香』

孰高孰下取决于使用者。

> 你是张三丰，你就能徒手夺来灭绝师太的倚天剑。

并不是说用了一个操作系统，用了某个软件，就会显得自己多么高明，如果不能给日常工作生活提高效率，让自己节省时间，再好的工具，那又有什么意义呢？

> 抓到老鼠的猫才是好猫呀！

## 0xEE 后续

> 2016-01-04 已换 Macbook Pro, 依然挂念 Windows.
> 2017-05-01 已换 2016 年 Macbook Pro With Multi-Touchbar.
> 2019-04-13 新增 SP6 作为生活机。

ChangeLog:
- 2017-03-08 09:32:15 整理知乎回答，搬运到博客上。
- 2017-05-01 09:32:15 补充现在使用的电脑信息
- 2017-06-10 09:32:15 重新排版，增加后续章节。
- 2019-04-13 13:57:02 重新排版
