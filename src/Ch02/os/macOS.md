# 如何优雅的使用 macOS

## 0x01 macOS 系统操作

### 快捷键

```bash
- cmd 为 command 按键，通常情况下为所有桌面程序通用性的快捷键。
- ctrl ，通常情况下是针对程序的功能进行加强，并且此功能往往是非 cmd 类（窗口操作，选择，复制粘贴等等）操作。
- shift 按键通常用于加强操作。一般会让操作更进一步 or 相反操作。

- cmd+tab =~ alt+tab 程序之间的切换
- cmd+\` 应用内窗口切换

- cmd+h 窗口 hide
- cmd+m 窗口 minimize
- cmd+n 新建窗口
- cmd+o 打开
- cmd+s 保存
- cmd+shift+s 另存为
- cmd+p 打印 print
- cmd+w 关闭
- cmd+q quit

- cmd+a select all
- cmd+i show info
- cmd+n create a new folder
- cmd+f search
- cmd+c copy
- cmd+v paste
- cmd+delete  删除选中文件
- cmd+shift+delete 清空回收站

- cmd+= 放大
- cmd+- 缩小
- cmd+t 新建选项卡
- cmd+r 刷新

- cmd+shift+3 截取整个屏幕
- cmd+shift+4 截取选择区域
- cmd+shift+4+SPACE 截取选择窗口
- cmd+ 鼠标点击 -> 选中不连续文件
- control+ 鼠标点击 -> 相当于 win 中右键点击

- fn+left home
- fn+right end
- fn+up pageup
- fn+down pagedown
```

### 触摸板

```bash
- 单指点击 - 单击
- 单指滑动 - 滑动鼠标光标
- 双指点击 - 相当于 Windows 的鼠标右键
- 三指点击 - 划词查找

- 双指上下滑动 - 滚动
- 双指缩放 - 与 Android 上图片缩放一致
- 双指双击 - 只能缩放
- 双指旋转 - 旋转
- 双指左右滑动 - 应用内切换网页
- 双指头从右往左
- 三指头左右滑动 - 全屏幕 App 切换
- 大拇指和食中无名缩放 - launchpad
```

## 0x02 日常软件

- iTunes
- iPhoto
- SpotLight -> Alfred 3

- Google Chrome
- Safari

- QQ
- WeXin

- Adobe PhotoShop CC
- Adobe PhotoShop LightRoom
- Adobe After Effect
- Final Cut Pro X
- Pollar Photo Editor
- Sketch
- ScreenFlow
- QuickTime
- iQiyi
- Snip Pro
- NeteaseMusic
- IINA
- Axure
- HandBrake

- 欧陆词典
- Calibre
- Margin Note 3
- PDF Expect
- Microsoft Office
- iWork 套件：包括 pages, numbers, keynote
- XMind
- Spark
- TeamViewer 远程管理
- OmniFocus

- AppCleaner
- CleanMyMac
- VMWare
- BetterZip
- Amphetamine
- PopClip
- Paste

### 0x03 开发者工具

### 图形界面

```bash
- iTerm2

- PyCharm
- Intellij IDEA
- WebStorm
- Datagrip
- VSCode
- Vim/NeoVim

- SS QT
- Charles
- Wireshark
- Github Desktop
- QGIS

- Dash
```

### 命令行

    #!/bin/bash

    if test ! $(which brew); then
        echo "Installing homebrew..."
        ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
    fi

    # Shell Support
    brew install bash
    brew install bash-completion2
    echo "Adding the newly installed shell to the list of allowed shells"
    sudo bash -c 'echo /usr/local/bin/bash >> /etc/shells'
    # chsh -s /usr/local/bin/bash
    brew install zsh
    sudo bash -c 'echo /usr/local/bin/zsh >> /etc/shells'
    chsh -s /usr/local/bin/zsh
    brew install fish
    sudo bash -c 'echo /usr/local/bin/fish >> /etc/shells'
    # chsh -s /usr/local/bin/fish

> 注意：MAC 中存在一些独占的命令

```
open
pbcopy
pbpaste
screencapture
launchctl
mdfind（还是 linux 的 find 好用）
sip （还是比较推荐 imagemagic)
```

> 注意：MAC 使用的大多命令来自于 FreeBSD , 并不是来自 GNU , 所以很多命令会与常规的 linux 命令不太一样。 所以，Shell 命令请在安装完 Gnu 的工具集之后，可以到我的文章 Shell CheatSheat 查看语法。

```bash
brew install diffutils
brew install binutils
brew install moreutils
brew install findutils
brew install gnu-sed
brew install ed
brew install findutils
brew install gnu-indent
brew install gnu-sed
brew install gnu-tar
brew install gnu-which
brew install grep

# aerial 屏保
# https://github.com/JohnCoates/Aerial
brew cask install aerial
# https://github.com/sindresorhus/quick-look-plugins
brew cask install qlcolorcode qlstephen qlmarkdown quicklook-json qlprettypatch quicklook-csv betterzipql qlimagesize webpquicklook suspicious-package quicklookase qlvideo
brew cask install keycastr
```

另起终端

有时候 /usr/local 的可能会存在权限问题，建议如果可能出现问题，则需要执行下面的命令修复权限。

```bash
sudo chown -R $(whoami):admin /usr/local/
```

## 0x03 编程环境

---

### Python

    brew install python@2 python

### NodeJS

```bash
# ~/.npmrc
chromedriver_cdnurl=http://cdn.npm.taobao.org/dist/chromedriver
operadriver_cdnurl=http://cdn.npm.taobao.org/dist/operadriver
phantomjs_cdnurl=http://cdn.npm.taobao.org/dist/phantomjs
fse_binary_host_mirror=https://npm.taobao.org/mirrors/fsevents
sass_binary_site=http://cdn.npm.taobao.org/dist/node-sass
electron_mirror=http://cdn.npm.taobao.org/dist/electron/

registry=https://registry.npm.taobao.org
disturl=https://npm.taobao.org/dist
chromedriver-cdnurl=https://npm.taobao.org/mirrors/chromedriver
couchbase-binary-host-mirror=https://npm.taobao.org/mirrors/couchbase/v{version}
debug-binary-host-mirror=https://npm.taobao.org/mirrors/node-inspector
electron-mirror=https://npm.taobao.org/mirrors/electron/
flow-bin-binary-host-mirror=https://npm.taobao.org/mirrors/flow/v
fse-binary-host-mirror=https://npm.taobao.org/mirrors/fsevents
fuse-bindings-binary-host-mirror=https://npm.taobao.org/mirrors/fuse-bindings/v{version}
git4win-mirror=https://npm.taobao.org/mirrors/git-for-windows
gl-binary-host-mirror=https://npm.taobao.org/mirrors/gl/v{version}
grpc-node-binary-host-mirror=https://npm.taobao.org/mirrors
hackrf-binary-host-mirror=https://npm.taobao.org/mirrors/hackrf/v{version}
leveldown-binary-host-mirror=https://npm.taobao.org/mirrors/leveldown/v{version}
leveldown-hyper-binary-host-mirror=https://npm.taobao.org/mirrors/leveldown-hyper/v{version}
mknod-binary-host-mirror=https://npm.taobao.org/mirrors/mknod/v{version}
node-sqlite3-binary-host-mirror=https://npm.taobao.org/mirrors
node-tk5-binary-host-mirror=https://npm.taobao.org/mirrors/node-tk5/v{version}
nodegit-binary-host-mirror=https://npm.taobao.org/mirrors/nodegit/v{version}/
operadriver-cdnurl=https://npm.taobao.org/mirrors/operadriver
phantomjs-cdnurl=https://npm.taobao.org/mirrors/phantomjs
profiler-binary-host-mirror=https://npm.taobao.org/mirrors/node-inspector/
puppeteer-download-host=https://npm.taobao.org/mirrors
python-mirror=https://npm.taobao.org/mirrors/python
rabin-binary-host-mirror=https://npm.taobao.org/mirrors/rabin/v{version}
sass-binary-site=https://npm.taobao.org/mirrors/node-sass
sodium-prebuilt-binary-host-mirror=https://npm.taobao.org/mirrors/sodium-prebuilt/v{version}
sqlite3-binary-site=https://npm.taobao.org/mirrors/sqlite3
utf-8-validate-binary-host-mirror=https://npm.taobao.org/mirrors/utf-8-validate/v{version}
utp-native-binary-host-mirror=https://npm.taobao.org/mirrors/utp-native/v{version}
zmq-prebuilt-binary-host-mirror=https://npm.taobao.org/mirrors/zmq-prebuilt/v{version}
cypress_download_mirror=https://npm.taobao.org/mirrors/cypress{version}

```

## 0x04 剪辑环境

---

## 0xDD mac 系统踩坑集

---

这里记录 mac homebrew 之类的工具的踩坑集合

### 本地 Socket 异常

在 Python 中执行下面的代码的时候总是报错：

```python
ip = socket.gethostbyname(socket.gethostname())
# socket.gaierror:
# [Errno 8] nodename nor servname provided, or not known

```

最后发现是因为设置主机名没有设置好

```bash
sudo scutil --set ComputerName "macOS"
sudo scutil --set LocalHostName "macOS"
sudo scutil --set HostName "macOS"
dscacheutil -flushcache
```

### 常见编译问题解决思路

```
# 确保 xcode command line 是最新
softwareupdate --all --install --force

# 重新安装 xcode command line
sudo rm -rf /Library/Developer/CommandLineTools
sudo xcode-select --install

# 如果还不能安装，建议到下面地址下载
https://developer.apple.com/download/more/?=command%20line%20tools
```

## 0xEE. 扩展阅读

---

- [关于 Mac 我的回答](https://www.zhihu.com/question/30816866/answer/59415036)
- [关于 Ubuntu 我的回答](https://www.zhihu.com/question/30816866/answer/59415036)
- [关于 Win10 我的回答](https://www.zhihu.com/question/32129337/answer/59379401)

---
ChangeLog:
 - **2020-10-31** 重修文字
