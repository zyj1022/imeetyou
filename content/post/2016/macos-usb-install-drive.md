---
title: 简单制作 macOS Sierra 正式版U盘USB启动安装盘方法教程
date: 2016-11-12 13:38:23
categories:
- life
tags:
- mac
---

# 概述

最近家里的Macmin重现折腾了一遍，换了一块ssd，系统要重现安装一次，需要制作U盘启动盘，
在此做个记录教程，方便以后查看。

# 使用命令行创建制作 macOS Sierra 正式版 USB 安装盘

苹果官方系统内置的命令，优点是稳妥而且没有兼容性问题，只是需要通过命令行操作，对新手来说可能看似有点复杂，但其实步骤还是非常简单的

- 首先，准备一个 8GB 或更大容量的 U盘，并备份好里面的所有资料。
- 下载好 macOS Sierra 正式版的安装程序
- 打开 “应用程序 → 实用工具 → 磁盘工具”，将U盘「抹掉」(格式化) 成「Mac OS X 扩展（日志式）」格式、GUID 分区图，并将U盘命名为「Sierra」。(注意：这个盘符名称将会与后面的命令一一对应，如果你改了这盘符的名字，必须保证后面的命令里的名称也要一致。)

<!-- more -->

![disk_ulitily_2x](http://og8z552x2.bkt.clouddn.com/disk_ulitily_2x.jpg)



- 打开 “应用程序→实用工具→终端”，将下面的一段命令复制并粘贴进去：

```
sudo /Applications/Install\ macOS\ Sierra.app/Contents/Resources/createinstallmedia --volume /Volumes/Sierra --applicationpath /Applications/Install\ macOS\ Sierra.app --nointeraction
```

- 回车并执行该命令，这时会提示让你输入管理员密码，便会开始制作过程了：

![terminal_2x](http://og8z552x2.bkt.clouddn.com/terminal_2x.jpg)

- 如上图，这时系统已经在制作中了，请耐心等待直到屏幕最后出现 Done. 字样即表示大功告成了！然后，就带着U盘出去浪吧……


# 通过 U 盘安装 macOS Sierra / 格式化重装 (抹盘全新安装系统) 方法

当你制作好 macOS Sierra 的安装盘 U 盘之后，你就可以利用它来给 Mac 电脑格式化重装 (抹盘安装)了。操作的方法非常简单：

- 当然还是要想办法备份好 Mac 里所有的重要数据了。
- 插上制作好的安装U盘，如果系统能识别出来即可，这时我们先关机了。
- 按下电源键开机，当听到“噹”的一声时，按住 Option 键不放，直到出现启动菜单选项：

![mac_option_boot](http://og8z552x2.bkt.clouddn.com/mac_option_boot_2x.jpg)

- 这时选择安装U盘 (黄色图标) 并回车，就可以开始安装了，在过程中你可以通过“磁盘工具”对 Mac 的磁盘式化或者重新分区等操作。

- 之后就是一步一步的安装直到完成了。


更详细的或可以查看这里 [异次元](http://www.iplaysoft.com/macos-usb-install-drive.html)


