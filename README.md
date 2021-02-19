# 论如何开好一个 Minecraft 模组服

大家好，这里是晓夜。一个普通的模组服服主。本帖将介绍如何开好一个Minecraft 模组服。同时也会与各位分享一些开服经验。希望本帖能够帮助到新人腐竹。

本文重写计划始于 2021/02/17。全文使用 Markdown 格式撰写。

协助者：[冷枫小乐](https://www.mcbbs.net/home.php?mod=space&uid=1838457)

## 一、开服前的准备

开 Minecraft 服务器，相当于在操作系统上运行一个 Java 程序。由于对高稳定性和高性能的需要，我们通常不会将生产服务器置于自己的私人电脑上。这时候，我们需要租用或购买专业的物理服务器来运行 Minecraft 服务器。

> **注意：切勿将 Minecraft 服务器与物理服务器混淆**。前者指的是单个 Minecraft 服务端程序实例，后者指的是整个操作系统。

### Part 1 选择物理服务器

对运行 Minecraft 服务器而言，常见的物理服务器通常存在以下种类

- 面板服
  - 使用网页与 [FTP](https://baike.baidu.com/item/FTP%E5%8D%8F%E8%AE%AE/7651119) 管理服务器，效率较低，有配置作假的可能。大部分无法使用 MySQL 数据库等高级功能。甚至部分商家会限制使用一些高性能核心和资源消耗量较大的插件，如 CoreProtect。
- VPS
  - 中文名为「虚拟专用服务器」，即服务商在一台独立机上使用虚拟化平台，如 VMware，将独立机的资源分割，并分配给多个运行在本台独立机上的虚拟专享服务器。实现各虚拟专享服务器的操作系统、内存、进程等资源的隔离。可以使用 MySQL 数据库等高级功能，也可以挂机器人。在性能较高的情况下，一台 VPS 可以开放数个 Minecraft 服务器。自由性较高。
  - 你购买的 VPS 就是虚拟专享服务器中的一台。
- 独立机
  - 通俗易懂。即一台独立的物理机，资源全部属于自己。具有 VPS 的全部优点。

如果你想购买物理服务器，可以查看论坛的交易代理版块或前往专业的电商平台。尽可能选择运营时间长的服务商。**不要为了便宜而选择配置差、服务态度不好的服务商**。一个好的服务商可以让你事倍功半。

开服初期，人数较少。无需购买较高配置的物理服务器。一般来讲，模组服初期 4 核心 8 GB 内存足矣。应该随着服务器规模的扩张，逐渐升级物理服务器配置。

**注意：Minecraft 服务端通常是单核心运行的，服务器卡顿时不要随意升级核心数**

### Part 2 拥有物理服务器后

> 本小节讲述内容基于 64 位 Windows Server 2012 及以上版本的操作系统。

拥有物理服务器后，我们需要做一些准备。

1. **在物理服务器中安装 Java**。请前往 [Java 官网](www.java.com) 下载 64 位的安装包，并运行安装包，安装即可。如果你是在个人电脑上下载的安装包，可通过 Windows 自带的远程桌面、即时通讯软件，如 QQ、百度网盘传输至物理服务器。

   > 小文件建议使用远程桌面传输，大文件建议使用百度网盘或即时通讯软件。

2. **安装文本编辑器**。开模组服，必定编辑大量配置文件。这里建议使用 [VS Code]([https://code.visualstudio.com/](https://code.visualstudio.com/))，是微软开发的编辑器，功能强大。

3. **安装压缩软件**。Minecraft 服务端文件多、散。备份时直接传输文件夹必定浪费大量时间。某些插件的配置文件也可能会是压缩包形式，需解压后使用。这里建议使用 [7-Zip](https://www.7-zip.org/)，完全免费且开源。
### Part 3 了解端口

端口是设备与外界通讯交流的出口。一个 IP 地址 拥有 65536 个端口。如果把 IP 比作房子，那端口就是房子的门。

当你购买 VPS 时，服务商会告知你这台 VPS 的可用端口。运行一个 Minecraft 服务端会占用一个端口。

如果你使用独立机，除了一些特殊端口，如 3389（Windows 远程桌面默认端口），3306（MySQL 默认端口），所有端口都可以使用。为了防止冲突，我们一般使用 10000 以后的端口。

第一次运行 Minecraft 服务端，`server.properties` 内的默认端口为 25565。Minecraft 将 25565 端口视为 Minecraft 服务端的默认端口。如果一个 Minecraft 服务端的端口是 25565，那么在 Minecraft 多人游戏页面仅需填写 IP 即可。

### Part 4 小提示

Windows 防火墙可能会拦截玩家进入服务器，这时需要关闭防火墙。进入 控制面板-系统与安全，关闭即可。

CentOS（Linux 的一个发行版），关闭防火墙的方法是：

1. 使用 root 账号登录。

2. 输入以下命令

   ```
   systemctl disable firewalld.service
   systemctl stop firewalld.service
   ```

Linux 其它发行版的关闭方法可自行到搜索引擎搜索。

**开服初期，强烈不推荐没有接触过 Linux 系统的人在 Linux 上开服，建议先从 Windows 入手。直接使用一个没有接触过的系统，可能会导致许多不必要的麻烦。可能你在 Windows 上 2 分钟解决的问题。在 Linux 系统上因为自己的不熟悉导致许多问题需要询问别人，浪费大量时间。**

## 二、认识服务端

问：论坛服务端整合包版有很多优秀的服务端整合包，为什么我要自己制作？

答：因为自己制作服务端可以让你自己知道在哪里做过什么。如果直接使用现成的服务端，小白很难理解各个插件的作用，怎么配置，也不会提高你的技术水平。自己的经历是无价之宝。

### Part 1 服务端核心

我们回家需要钥匙。开启服务端也需要钥匙，而服务端核心就是这把“钥匙”。服务端核心是一个 jar 文件。由于本帖主要讨论模组服，这里仅介绍模组服主流版本的服务端核心。

- 1.7.10
  - [Uranium](https://www.mcbbs.net/thread-723871-1-1.html)
- 1.12.2
  - [CatServer](https://catserver.moe/)
  - [Mohist](https://www.mcbbs.net/thread-916340-1-1.html)

### Part 2 了解 EULA

下载完服务端核心和及其所需的 `libraries` 后，将其移动至同一文件夹。并在此文件夹内创建一个名为 `eula.txt` 的文件。再打开这个文本文件，写入

```
eula=true
```

保存，关闭即可。

EULA 是 End User Licence Agreement 的缩写。翻译为中文就是「最终用户许可协议」。同意了 Mojang 的最终用户许可协议才可以继续启动服务端。

至于最终用户许可协议的具体内容，自行前往 Mojang 官网寻找。

### Part 3 了解启动脚本

顾名思义，启动脚本是用来启动服务端的。运行了正确的启动脚本后，你的服务端将启动。初次启动后，将生成一些文件并加载。第二次及以后的启动，会加载你服务端的数据。加载数据需要一定的时间，模组服的加载时间比纯净服长。

初学者听到脚本这样的名词，可能会觉得困难。其实写启动脚本并不需要水平多么高的编程基础。写完后也就是调整一下内存等，改动不大。当然你也可以参考现成的，基本上复制后简单修改一下就可以用。

Windows 系统步骤如下：

1. 创建一个名为 `start.bat` 的文件。
2. 写入脚本，后保存

就这么简单，你学会~~废~~了吗？

我这里展示一份启动脚本，`rem` 后是注释，可以删除。你可以修改此脚本后直接使用。

```
@echo off
:head
set memory=4
title Minecraft xxx 已分配内存:%memory%G
echo 已设置内存为%memory%G
rem 这里是注释，上面的数字是分配的内存，单位为GB，自行修改上面即可，不需要改启动代码
set mainfile=xxx
rem 上面是服务端核心名字，不需要加文件后缀名，修改后不需要改启动代码
java -Xms%memory%G -Xmx%memory%G -XX:+UseG1GC -jar %mainfile%.jar -nogui
echo 服务器已关闭，将于10秒后重启
ping -n 10 127.0.0.1>nul
goto head
rem 跳回head，继续执行，达到崩服自动重启的效果
pause
```

保证启动脚本位于服务端根目录。运行后会弹出一个黑框框，不断的提示一些英文。最后看到 `Done` 就说明启动成功了。

这时候可以输入一些指令测试一下，如

```
say test
```

你将看到控制台显示 `[Server]test` 。然后你就可以输入 `/stop` 关服并安装你的插件、Mod 了。

### Part 4 服务端启动后生成了什么，有什么用？

| 文件名               | 备注                                                         |
| -------------------- | ------------------------------------------------------------ |
| bukkit.yml           | Bukkit 配置文件，关闭服务器，因白名单而被断开连接，服务器满员等文本可以在这里修改。Bukkit 系服务端会生成此文件。 |
| spigot.yml           | Spigot 配置文件，以 Spigot 为基础的服务端核心会生成此文件。有一些与优化相关的配置项目。 |
| ops.json             | 这里有服务器 OP 的 UUID 等信息。                             |
| server.properties    | 服务端配置文件。可以在这里面修改端口等。                     |
| whitelist.json       | 白名单。                                                     |
| bannned-players.json | 已封禁的玩家的 UUID 等信息。                                 |
| banned-ips.json      | 已封禁的 IP。                                                |
| config               | Mod 的配置文件夹。里面将包含所有已添加的 Mod 的配置文件。    |
| logs                 | 存储服务端日志。                                             |
| libraries            | 服务端库文件。                                               |
| plugins              | 插件文件夹。                                                 |

### Part 5 一般服务器必装插件

- [EssentialsX](http://www.mcbbs.net/thread-619883-1-1.html)
  - 基础插件，有 `/tpa` 指令那个。
- [PlugMan](https://www.mcbbs.net/thread-954532-1-1.html)
  - 可以不关服加载、卸载插件。
  - Mohist 等自带此功能的服务端核心可以不加此插件。
- [AuthMe](http://www.mcbbs.net/thread-442729-1-1.html) 
  - 登录插件
  - 使用正版登录或外置登录的服务器可不加此插件。

- [DoubleLoginFix](http://www.mcbbs.net/thread-817128-1-1.html)
  - 修复登录插件的 Bug。

因为本帖主要讨论模组服，插件就不多说了。有意者可浏览论坛服务端插件版块。

## 三、认识 YAML

我们用百度百科的一段介绍引入话题。

> YAML是一个可读性高，用来表达数据[序列化](https://baike.baidu.com/item/序列化)的格式。YAML参考了其他多种语言，包括：[C语言](https://baike.baidu.com/item/C语言)、[Python](https://baike.baidu.com/item/Python)、[Perl](https://baike.baidu.com/item/Perl)，并从[XML](https://baike.baidu.com/item/XML)、电子邮件的数据格式（RFC 2822）中获得灵感。Clark Evans在2001年首次发表了这种语言，另外Ingy d?t Net与Oren Ben-Kiki也是这语言的共同设计者。当前已经有数种编程语言或脚本语言支持（或者说解析）这种语言。
> YAML是"YAML Ain't a Markup Language"（YAML不是一种[标记语言](https://baike.baidu.com/item/标记语言)）的[递归缩写](https://baike.baidu.com/item/递归缩写)。在开发的这种语言时，YAML 的意思其实是："Yet Another Markup Language"（仍是一种[标记语言](https://baike.baidu.com/item/标记语言)），但为了强调这种语言以数据做为中心，而不是以标记语言为重点，而用反向缩略语重命名。

插件的配置文件大多数是 YAML 文件，即后缀名为 `.yml` ，每个插件都有自己的配置文件格式，根据默认配置的格式修改配置文件即可。

### Part 1 举个例子

我们拿 ChestCommands 菜单插件来举例子。

```
test:
  NAME: '&atest'
  LORE:
    - 'test'
  ID: 1
  KEEP-OPEN: true
  POSITION-X: 1
  POSITION-Y: 1
```

观察后，我们可以发现此段配置具有以下特点：

- 每子项前有相同数量的空格
- 英文冒号后有一个空格
- 文字前后有引号

一般来说，按照插件生成的默认配置的格式继续写即可。

### Part 2 拓展

**如果您没有编程基础，可跳过此小节，不影响后续阅读。**

Java 有 8 种基础数据类型，如下：

- byte
  - 位
- short
  - 短整数
- int
  - 整数
- long
  - 长整数
- float
  - 单精度
- double
  - 双精度
- char
  - 字符
- boolean
  - 布尔值

## 四、模组服客户端制作和文件解释

模组服需要使用专属客户端进入服务器。制作客户端也是非常简单的，不需要太多专业知识。

### Part 1 开始制作

[TODO]

### Part 2 文件解释

[TODO]

### Part 3 辅助 Mod 推荐

| 名称                             | 链接                                         |
| -------------------------------- | -------------------------------------------- |
| JourneyMap(小地图)               | http://www.mcbbs.net/thread-612917-1-1.html  |
| DamageIndicators(伤害与血量显示) | https://www.mcbbs.net/thread-137281-1-1.html |
| InputFix(中文输入)               | http://www.mcbbs.net/thread-83941-1-1.html   |
| Optifine(高清修复)               | http://www.mcbbs.net/thread-606019-1-1.html  |
| MouseTweak(鼠标手势)             | http://www.mcbbs.net/thread-69677-1-1.html   |
| BetterFPS(优化)                  | http://www.mcbbs.net/thread-539780-1-10.html |
| InventoryTweaks(R键整理)         | http://www.mcbbs.net/thread-877466-1-1.html  |
| CustomSkinLoader(皮肤)           | http://www.mcbbs.net/thread-269807-1-1.html  |
| NoEnoughItems(NEI)(合成表)       | http://www.mcbbs.net/thread-389937-1-1.html  |



## 五、自动赞助系统

维持服务器必然需要一定的资金。开商业服就是为了赚钱的。这里建议各位服主不要被所谓的「情怀」束缚。随着你服务器规模的扩大，玩家赞助有时无法及时回应。这时，自动赞助系统就是你的好帮手。

### Part 1 准备工作

安装以下插件：

- [Vault](https://www.mcbbs.net/thread-703488-1-1.html)
  - 经济前置插件
- [PlayerPoints](https://www.mcbbs.net/thread-514161-1-1.html)
  - 点券插件
- [ChestCommands](https://www.mcbbs.net/thread-933158-1-1.html)
  - 菜单插件
- [CommandCode](http://www.mcbbs.net/thread-346482-1-1.html)
  - 激活码插件

在你的服务器安装好这些插件后，我们需要找一个靠谱的自动发卡平台。一定要靠谱。有条件的可以自己搭建一个发卡平台。

### Part 2 自动发放点券

[TODO]

