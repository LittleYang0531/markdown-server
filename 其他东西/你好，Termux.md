# 你好，Termux!

## 前言

### Termux 是啥?

Termux 是一款能够让你在手机上使用命令行的软件。通过该软件，你可以实现搭建网站，编译 C++ 程序等基础功能，还能够实现安装 Linux 操作系统，通过命令行拍照等高级功能。而且最重要的一个特点就是无需 root，这就意味着任何手机都可以使用 Termux。

备注: 由于 Termux 的操作会涉及到一些系统层面上的东西，因此所导致的后果需要读者自行承担。

### Termux 对系统的限制

任何 **Android** 系统都可以使用 Termux，只要你能选择合适的版本装到你的手机上即可。Termux 似乎并没有 iOS 版本。如果读者想使用 iOS 手机上的命令行的话，请选择 iSH Shell。由于作者家里并无资金购买价格较昂贵的 iPhone 手机，因此本文在此不做 iSH Shell 的讨论。

备注: 作者未测试过 iSH Shell，因此请读者谨慎选择 iSH Shell，所导致的后果由读者自行承担。

### Termux 的功能

#### 基础功能: 

1. 你可以在你的手机上写 C++ 程序，并通过 Clang 编译器来编译它。
2. 你可以通过在你的手机上安装 nginx,php,apache 等软件包来搭建你的静态/动态网站。
3. 你可以通过脚本在你的手机上实现自动签到等功能。
4. ......

#### 高级功能:

1. 你可以在你的手机上安装 arm64 架构的 Linux。
2. 你可以通过 Termux:API 来使用命令行操作手机上的一些功能 (eg: 摄像机，通知，显示亮度)
3. 你可以通过一些大佬的脚本来为你的 Termux 安装图形化界面并用 VNC Viewer 进行远程连接。
4. ......

## Termux 的安装

想必看到以上对 Termux 的介绍，读者也想要在自己的手机上尝试一番 Termux 了吧。别着急，我们先从如何安装 Termux 开始说起。

请读者自行百度 Termux，跳出来的第一项就是 Termux 的官网。

你有三种途径来下载 Termux: F-Droid，Google Play 和酷安。个人推荐使用 F-Droid 进行下载，因为 F-Droid 并不需要使用梯子就能访问，而且网速还能够接受（毕竟是国外的网站嘛）,官网也推荐的是这种方法。注意不要下成 F-Droid 的安装包了！

下载好后，点击 com_termux_xxx.apk 进行 Termux 的安装。安装过程在此省略。

接下来，点开 Termux，进入命令行的主界面，读者可能会遇到 'Unable to install' 的问题。这是因为 Termux 在第一次开启时需要从远程服务器加载数据，而远程服务器在国内可能无法访问。不过现在问题应该已经解决了（反正作者一次都没遇到过），如果读者遇到了此问题，有三种方法可供参考: 

1. 梯子梯子梯子!!!
2. 如果用的 wifi，可以尝试切换成运营商流量
3. F-Droid > Google Play > 酷安 根据这个顺序尝试安装，如果不行再重复1、2 步骤操作

以上方法作者均未尝试过，请读者自行甄别其成功率并自行解决问题。

## Termux 的基本操作

### 字体调节

觉得字体太小，看不清楚？Termux 支持对命令行字体的调节，而且就和缩放图片一样简单。

什么，你说怎么调节？和缩放图片一样就行了啊。

### 多任务窗口

从您手机的最左侧向右滑，会跳出来一个白色的框框，再点击白色框框右下角的 'NEW SESSION'，就可以新建一个任务窗口了。

### 软件包安装

Termux 使用的是 pkg 软件包管理器，如果读者熟悉 apt 也是可以使用的。在此推荐使用 apt 软件包管理器，毕竟我们一般会使用的 Linux 发行版用的绝大多数还是 apt 软件包管理器，而且 pkg 和 apt 在大多数操作上还是一致的。

以下是 apt 软件包管理器的大致用法，Ubuntu/Debian 用法一致 (~~笑死，本 APT 具有超级牛力~~)，读者也可以输入 `apt help` 来获得这些帮助:
```bash
Usage: apt [options] command

命令行软件包管理器 apt 提供软件包搜索，管理和信息查询等功能。
它提供的功能与其他 APT 工具相同（像 apt-get 和 apt-cache），
但是默认情况下被设置得更适合交互。

最常使用的 command:
  list - 根据名称列出软件包
  search - 搜索软件包描述
  show - 显示软件包细节
  install - 安装软件包
  reinstall - 重新安装软件包
  remove - 移除软件包
  autoremove - 卸载所有自动安装且不再使用的软件包
  update - 更新可用软件包列表
  upgrade - 通过 安装/升级 软件来更新系统
  full-upgrade - 通过 卸载/安装/升级 来更新系统
  edit-sources - 编辑软件源信息文件
  satisfy - 使系统满足依赖关系字符串
  
参见 apt(8) 以获取更多关于可用命令的信息。
程序配置选项及语法都已经在 apt.conf(5) 中阐明。
欲知如何配置软件源，请参阅 sources.list(5)。
软件包及其版本偏好可以通过 apt_preferences(5) 来设置。
关于安全方面的细节可以参考 apt-secure(8).
                                           本 APT 具有超级牛力。
```

### 自定义快捷栏

快捷栏是什么？就是你手机键盘上面的那一圈黑色的框框。但是有些我们常用的快捷键并没有显示，因此就有了可以自定义您快捷栏的方法。

先输入以下指令:

```bash
vim ~/.termux/termux.properties
```

如果您的手机提示找不到 `vim` 指令的话，用刚才讲到的软件包安装的方法安装 `vim` 软件包就行了。

接下来，您需要在打开的文件中有则修改，无则添加下面这一段话:

```bash
extra-keys=[['ESC','-','/','HOME','UP','END','PGUP','DEL'],['TAB','CTRL','ALT','LEFT','DOWN','RIGHT','PGDN','BKSP']]
```

重启 Termux，您就应该能看到新的快捷栏了。

### 启动提示

如果您经常使用 Termux，您应该会注意到 Termux 刚启动时的提示语。对于经常使用的人来说，这些启动语就会显得比较多余。

要想修改启动时的提示语，运行

```bash
vim ~/../usr/etc/motd
```

打开文件后，随意修改为自己想要的提示语即可。

### 快捷键

如果您边敲指令边听歌，想要提高歌的音量时，您一定会注意到这样是改不了音量的。这是因为，Termux 默认将音量下键指定为了 Ctrl 键，将音量上键指定为了另一种特殊的按键 (Termux 特有)。通过这些按键，您可以快速敲出 Ctrl+C 等的效果，相对应的，您就失去了它本来的功能。

至于如何恢复音量上下键原有的功能，读者可以自己去搜索引擎上搜索。(因为我也不会...)

### 源相关

如果您在使用 apt/pkg 安装软件包时，您会发现其实安装的速度非常慢 (不排除有某些特殊情况)，那是因为 Termux 默认使用的是官方源。

要想获得更快的安装速度，在此推荐使用清华源，安装命令如下:

```bash
termux-change-repo
```

这是官方自带的软件源切换器。当然，如果您喜欢自己手动实现的话，您也可以使用 

```bash
sed -i 's@^\(deb.*stable main\)$@#\1\ndeb https://mirrors.tuna.tsinghua.edu.cn/termux/termux-packages-24 stable main@' $PREFIX/etc/apt/sources.list
sed -i 's@^\(deb.*games stable\)$@#\1\ndeb https://mirrors.tuna.tsinghua.edu.cn/termux/game-packages-24 games stable@' $PREFIX/etc/apt/sources.list.d/game.list
sed -i 's@^\(deb.*science stable\)$@#\1\ndeb https://mirrors.tuna.tsinghua.edu.cn/termux/science-packages-24 science stable@' $PREFIX/etc/apt/sources.list.d/science.list
apt update && apt upgrade
```

来更新您的软件源，只不过其安装不如第一种稳定，且可能会有其他的风险发生。

清华源官网使用帮助: [termux | 镜像站使用帮助 | 清华大学开源软件镜像站 | Tsinghua Open Source Mirror](https://mirrors.tuna.tsinghua.edu.cn/help/termux/)

## 在 Termux 环境下安装 Linux

### 前言

注意: 网上有着许许多多的 Linux 一键安装脚本，在此作者使用的 Tmoe 的脚本来安装 Termux。

当然，国光制作的 Linux 一键安装脚本也能直接安装，支持的版本也很多。但跟 Tmoe 相比，安装后的 Linux 无法使用 mount 指令，这就导致在搭建某些服务时由于没有权限使用 mount 指令而安装失败 ~~(点名 HUSTOJ)~~。

### 差异

Android 上的 Linux 有着两种容器供您选择，一种是 proot 容器而另一种则是 chroot 容器。

对于非 ROOT 的用户，您能且仅能安装 proot 容器的 Linux。而对于 ROOT 的用户，您还可以安装 chroot 容器的 Linux。

proot 容器和 chroot 容器最大的区别则在于对端口的开放程度。proot 仅允许您使用 1024 以上的端口，这就导致了一些想要搭建网站的人不能使用 80 以及 443 等常用端口，但 chroot 则是全端口开放的，这就预示着您可以使用**任意的端口** (要是该端口被占用就另当别论)。

如果您是 ROOT 用户，您可以参考 [在你的Android手机上运行Linux - wendster 的博客 - 洛谷博客](https://www.luogu.com.cn/blog/wendster/play-linux-on-your-android-phone) 来在您的手机上安装 chroot 的 Linux。

接下来的教程会为您介绍如何在 Android 上安装 proot 容器的 Linux。

### 准备

安装 Tmoe 您需要准备的软件包有 `curl`，这些软件包您都可以通过上文讲的方法来安装。

Tmoe 项目官网: [GitHub - 2moe/tmoe-linux: Without any basic knowledge of linux shell, you can easily install and configure a GNU/Linux graphical desktop environment on 📱Android termux and 💻WSL .🍰You can also run VSCode on your android phone.](https://github.com/2moe/tmoe-linux)

~~(这标题可真长啊)~~

首先，您需要获取到 Tmoe 的使用脚本。

```bash
. <(curl -L git.io/linux.sh) # 针对能够进入到 Github 的用户
. <(curl -L gitee.com/mo2/linux/raw/2/2) # 针对无法进入 Github 的用户
curl -Lo l l.tmoe.me; sh l # 如果您记不住上面的链接，或以上方法都出错了，就可以使用该指令
```

