---
title: deepin-ubuntu-linux
layout: post
date:   2017-12-12 16:55:11 +0800
categories: learn linux
---

# 高级命令

- `set -- a b c`  设置当前函数、命令行参数`$1 $2 $3`

# 启动脚本

## 系统级

1. /etc/environment
1. /etc/xprofile Shell script executed while starting X Window System session. 
1. /etc/profile and /etc/profile.d/*
1. `/etc/<shell>.<shell>rc` 作用单独Shell script. This is a poor choice because it is single shell specific.

## 用户级

~/.pam_environment ~/.xprofile ~/.profile `~/.<shell>rc`

# oh-my-zsh

via curl

```bash
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

via wget

```bash
sh -c "$(wget https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O -)"
```

# autojump

`yum install autojump-zsh`

# root启动

* For a console program use: `sudo <program name>`
* If it is a GUI application use: `gksudo <program name>`
* 可修改`/usr/share/applications`和`/usr/local/share/applications`的快捷方式

# 软件仓库改为阿里云

```bash
sudo sed -i 's/packages.deepin.com\/deepin/mirrors.aliyun.com\/deepin\//p' /etc/apt/sources.list
```

# grub

1. [启动 菜单无法显示问题]( https://community.linuxmint.com/tutorial/view/842)

# sshpass - ssh密码

```bash
export SSHPASS=$DEPLOY_PASS
sshpass -e scp package.tgz $DEPLOY_USER@$DEPLOY_HOST:$DEPLOY_PATH
sshpass -e ssh $DEPLOY_USER@$DEPLOY_HOST $DEPLOY_PATH/deploy.sh
```

# ssh保持连接配置

- 客户端`/etc/ssh/ssh_config`

```bash
ServerAliveInterval 60
ServerAliveCountMax 3
```

- 服务端`/etc/ssh/sshd_config`

```bash
ClientAliveInterval 60
ClientAliveCountMax 3
```

# FlashPlayer Linux Debug问题

- 错误信息

```java
java.lang.NullPointerException
	at flash.tools.debugger.concrete.PlayerSession.pullUpActivationObjectVariables(PlayerSession.java:1007)
	at flash.tools.debugger.concrete.PlayerSession.requestFrame(PlayerSession.java:984)
	at flash.tools.debugger.concrete.DStackContext.populate(DStackContext.java:156)
	at flash.tools.debugger.concrete.DStackContext.getArguments(DStackContext.java:74)
	at flex.tools.debugger.cli.DebugCLI.appendFrameInfo(DebugCLI.java:1202)
	at flex.tools.debugger.cli.DebugCLI.doInfoStack(DebugCLI.java:1167)
	at flex.tools.debugger.cli.DebugCLI.processLine(DebugCLI.java:6471)
	at flex.tools.debugger.cli.DebugCLI.process(DebugCLI.java:727)
	at flex.tools.debugger.cli.DebugCLI.execute(DebugCLI.java:569)
	at flex.tools.debugger.cli.DebugCLI.main(DebugCLI.java:374)
java.lang.NullPointerException
	at flash.tools.debugger.concrete.PlayerSession.pullUpActivationObjectVariables(PlayerSession.java:1007)
	at flash.tools.debugger.concrete.PlayerSession.requestFrame(PlayerSession.java:984)
	at flash.tools.debugger.concrete.DStackContext.populate(DStackContext.java:156)
	at flash.tools.debugger.concrete.DStackContext.getArguments(DStackContext.java:74)
	at flex.tools.debugger.cli.DebugCLI.appendFrameInfo(DebugCLI.java:1202)
	at flex.tools.debugger.cli.DebugCLI.doInfoStack(DebugCLI.java:1167)
	at flex.tools.debugger.cli.DebugCLI.processLine(DebugCLI.java:6471)
	at flex.tools.debugger.cli.DebugCLI.process(DebugCLI.java:727)
	at flex.tools.debugger.cli.DebugCLI.execute(DebugCLI.java:569)
	at flex.tools.debugger.cli.DebugCLI.main(DebugCLI.java:374)
```

- 原因分析：出错时选中的debug sdk是工程sdk，是3.6，可能老的sdk和新的linux flashplayer不兼容
- 解决办法：在idea选中新版本的sdk做debug，这个sdk可以跟编译版本的sdk不同
- [讨论网址](https://bugs.interactive-pioneers.de/browse/FDT-2999)
    >The Flex SDK Debugger Adapter of the Flex SDK 3.6 has some bugs especially with 64-bit platforms 
    >(also Ubuntu 64-bit, times back when Adobe was also supporting Linux)
    >This was the reason for us to allow the user to choose the Flex SDK to debug with.