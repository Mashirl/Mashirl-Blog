---
title: 如何在Linux系统下搭建一个Minecraft Java版服务器？
date: 2019-11-30 12:36:10
tags:
---
Minecraft服务器相关的教程有很多，可是基本都是Windows下的教程，而Linux的开服教程，多数又较为老旧，有部分内容已经不再适用于今天的Minecraft服务器。正好没事，就来自己写一个教程吧。

>本文使用[CentOS](https://www.centos.org/)操作系统为例 版本为CentOS 7，
>其余环境请自行参考本文内容进行测试。
>本教程受众面为对Linux系统不是很懂的萌新，请各位julao不要指指点点。

<!-- more -->

## 安装jre

第一步当然就是安装jre了，输入如下命令↓

```
yum install jre -y
```

然后再输入

```
java -version
```

如果返回如下信息

```
openjdk version "1.8.0_242"
OpenJDK Runtime Environment (build 1.8.0_242-b08)
OpenJDK 64-Bit Server VM (build 25.242-b08, mixed mode)
```

则代表jre安装成功，接下来进入下一步。

## 配置服务器

说到Minecraft服务器，就要有一个服务端核心。

下面以[PaperMC](https://papermc.io)核心为例。

首先在[PaperMC官网](https://papermc.io)下载最新版的核心（~~看不懂的请使用Chrome自带的翻译或者使用各路机翻~~）。

下载好核心之后，文件名一般为 paper-版本号.jar，这里使用最新的230构建为例。

创建一个文件夹并且把核心文件扔在文件夹中。

在终端执行如下命令，开启服务器。

```
java -jar paper-230.jar
```
为了方便，我们创建一个sh脚本，就和Windows的bat一样。

```
vi start.sh
```
输入如上命令进入vim编辑器，按I开始编辑，输入上面我们执行的开服命令，然后按ESC退出编辑模式，输入:wq保存并退出就好了。

那么，如何运行这个脚本呢？

输入如下命令即可

```
./start.sh
```

它会自己下载一些东西，下完之后，你会发现服务器直接关闭了，属于正常现象。

在刚才创建的文件夹里面多出了几个文件。

我们需要用vim编辑 eula.txt

将

```
eula=false
```

改为

```
eula=true
```

再次运行服务器

当终端显示

```
Done (26.479s)! For help, type "help" or "?"
```

或类似内容，即代表服务器已经开启。

现在我们要关闭服务器修改一下服务器的基本配置文件，相当于服务器的设置，它就是server.properties文件。

它大概长这样↓

```
#Minecraft server properties
#Sat Nov 30 12:59:07 CST 2019
spawn-protection=16
server-name=Unknown Server
generator-settings=
force-gamemode=false
allow-nether=true
gamemode=2
broadcast-console-to-ops=true
enable-query=false
player-idle-timeout=0
difficulty=1
spawn-monsters=true
op-permission-level=4
resource-pack-hash=
announce-player-achievements=true
pvp=true
snooper-enabled=true
level-type=DEFAULT
hardcore=false
enable-command-block=false
max-players=20
network-compression-threshold=256
max-world-size=29999984
server-port=25565
debug=false
server-ip=
spawn-npcs=true
allow-flight=false
level-name=world
view-distance=10
resource-pack=
spawn-animals=false
white-list=false
generate-structures=true
online-mode=true
max-build-height=256
level-seed=
enable-rcon=false
motd=A Minecraft Server
```

上面的配置文件只是一个参考，这里就不一一讲解什么意思了。

如果你连初中英语水平都不到建议不要开服。

那我们继续往下走。

## 配置防火墙

配置一下防火墙才能进入服务器，当然具体怎么弄建议去各大搜索引擎搜索，这里就不附教程了。

当然如果你不想去的话可以直接手动关闭防火墙，简单粗暴，但是可能会出现安全问题。

```
systemctl stop firewalld
```



## 在关掉终端之后继续运行

这时候的服务器在关掉终端之后就会关掉，那么如何让它自己运作呢？

这就需要Screen的了。

输入如下命令安装Screen
```
yum install screen -y
```
安装完成后，输入如下命令查看使用方法
```
screen -h
```
不过这里就告诉大家怎么用吧，毕竟我们只需要让服务器自己运行下去

我们要用的命令大概只有下面几个
```
screen -S yourname -> 新建一个叫yourname的session
screen -ls（或者screen -list） -> 列出当前所有的session
screen -r yourname -> 回到yourname这个session
screen -d yourname -> 远程detach某个session
screen -d -r yourname -> 结束当前session并回到yourname这个session
```
输入如下命令新建一个session
```
screen -S 123
```
这个123是session的名字，可以随便起。

然后我们到存放服务端的文件夹，开启服务器。

再然后呢，你就可以把终端关掉，打开客户端看看能不能进去。

~~不能进去就让隔壁小孩玩一下电闸~~

如果能成功连接，那么恭喜你，成功了。

如果想连接回控制台，就在终端输入

```
screen -r PID/session名称
```

如果你忘记了session名称，可以输入下面的命令查看。

```
screen -ls
```

可以列出所有的session，找到你的那个，前面的数字是PID，后面的名称就是你填的名称啦~

就是这样，祝各位服主的服务器开的越来越好~