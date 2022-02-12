# 你好，Termux! - Android 命令行神器使用介绍

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

在脚本执行过程中，可能会让您下载各种各样的软件包。不要抗拒，按照给定的方法一个个慢慢装就行了。

执行完后，应该会跳出来语言选择的界面，选择您喜欢的语言，然后我们就可以进入正式的安装步骤了。

tmoe 不仅有很强大的 Linux 一键安装功能，还有更多的针对于 Android-Termux 的强大功能，例如终端美化，局域网音频传输等。由于本文讨论的是 Linux 的安装，在此请读者自行探索这些功能。

### 安装

来到脚本首页，进入 `proot容器` 界面，然后再选择 `arm64发行版列表`，在这里你能看到 Tmoe 所能支持的所有的 Linux 版本。这里作者并不推荐使用 `每周构建` 版本，由于是每周构建，因此你也不知道会出些什么问题，还是老老实实安装稳定版本比较好。

选择自己喜欢的 Linux 版本进行安装，注意不要安装到 `Chroot专属` 去了。在这里，作者就选择 CCF 所使用的 Ubuntu 20.04 LTS 来进行安装。

选择后，进入管理界面，再选择 `启动 proot xxx` 来进行安装您的 Linux。整个安装过程由脚本封装，因此您并不需要多输任何指令就可以完成安装。

### 配置

安装完成，应该会回到安装前的画面，这时您只需要再次选择 `启动 proot xxx` 就可以进入到您所安装的 Linux 里了。

如果您想要更快地进入您的 Linux，安装完成后应该会有快捷指令，例如 `Debian/Ubuntu` 的快捷指令则为 `debian`。

进入 Linux 后，我们需要配置一些东西。首先，您需要使用 `apt update` 来更新您的软件源或使用切换为其他的软件源，否则您会发现您不能安装任何的软件包，因为其默认的软件源版本较老旧，继续使用可能会出现问题。

接下来，您就可以在您的手机上尽情享受 Linux 带来的便利了。

## Android 中 Linux 的一些操作

鉴于 proot 容器对您所安装的 Linux 的限制，有些操作可能不能在 Termux 上正确地运行，例如 ftp 的安装，这就是作者不推荐已经 ROOT 的用户安装 proot 容器的原因。当然，如果您是非 ROOT 用户，也不用担心，这些问题我们都可以一步步解决。

当然，如果您是 Linux 指令方面的 `Lεɡεηdàгy GгàηdΜàsτεг`，您应该能找到这些矛盾的根源并且成功的解决它。

接下来的教程是依赖于 Ubuntu 20.04 LTS arm64 版本的，其他 Linux 版本的操作方法的本质相同，在此不过多解释。

### 网站的搭建

要想运行一个网站，首先就需要有网站的源码包和能够运行网站的软件包，还要配置各种能够和网站进行交互的 cgi 程序。源码包根据自己需求到网上随便找一个就行了，对于能够运行网站的软件包，选择 `nginx`，`apache` 等都可以。在此，作者使用 `nginx`+`mysql`+`php`+`C++` 来搭建一个 Online Judge。

本教程将使用 `php` 作为前端语言打造 GUI，`C++` 作为后端语言运行程序。

如果读者有其他的需要，也可以自行更换前端或后端语言，不过

首先安装 `nginx`,`mysql`,`php`,`C++` 的软件包:

```bash
apt install nginx mysql php g++ 
```

由于需要用到 `mysql` 和 `json` 来进行交互，因此我们还需要安装必要的底层插件包

```bash
apt install php-mysql libmysqlclient libjsoncpp
```

接下来，解压自己网站的源码包，记住根目录的绝对路径，然后打开 `/etc/nginx/sites-enabled/default` 进行配置: 

```bash
server {
	# 必须配置
	listen 8080;
	listen [::]:8080;
	root /etc/judge/web; # 根目录的绝对路径
	client_max_body_size 100m; # 最大上传文件大小
	location ~ \.php$ {
		include snippets/fastcgi-php.conf;
		fastcgi_pass unix:/var/run/php/php7.4-fpm.sock;
	}
	
	# 可选配置
	location @rewrite { # url 重写配置
		rewrite ^(/index.php)+(.*)$ /index.php?path=error&err=404%20Not%Found;
		rewrite ^([/])+([^.?]+)+([?])+(.*)$ /index.php?path=$2&$4;
		rewrite ^([/])+([^.?]+)$ /index.php?path=$2;
	}
	location / {
		try_files $url $url/ @rewrite;
		error_page 404 /index.php?path=error&err=404%20Not%20Found;
		error_page 403 /index.php?path=error&err=403%20Forbidden;
	}
}
```

运行 `service nginx start` 以运行

### FTP 的安装

由于 FTP 依赖的是 21/20 端口，而 proot 并不允许我们使用 1024 及以下的端口，因此我们需要修改 FTP 运行的端口才能正常地使用 FTP 的服务。

首先安装 vsftpd 以支持我们的 ftp 服务:

```bash
apt install vsftpd
```

顺带说一句，Ubuntu 并不支持 pkg 指令，因此如果读者使用的是 pkg 指令，请自行熟练对 apt 指令的使用。

接下来，我们需要配置 vsftpd 来修改我们 FTP 运行的端口以及 FTP 的根目录等重要配置。

请注意 vsftpd 的配置文件为 `/etc/vsftpd.conf` 而不是网上普遍的 `/etc/vsftpd/vsftpd.conf`。

使用 vim 等文本编辑器来编辑 vsftpd 的配置文件，如果没有文本编辑器的话可以自己安装一个，接下来的操作我们可能还需要用到。

接下来，您需要修改 / 添加以下配置: 

```bash
# 必须配置
listen=YES # 如果您家使用的是 ipv4 网络，则需要开启此配置
listen_ipv6=NO # 如果您家支持 ipv6 网络，则需要开启此配置
listen_port=8021 # ftp 监听端口，您可以修改为任意大于 1024 的端口号
connect_from_port_20=NO
ftp_data_port=8020 # ftp 数据端口，您可以修改为任意大于 1024 的端口号
secure_chroot_dir=/var/run/vsftpd/empty
allow_writeable_chroot=YES

# 被动模式配置
pasv_enable=YES
pasv_min_port=10001 # ftp 被动模式最小端口
pasv_max_port=10003 # ftp 被动模式最大端口
pasv_promiscuous=YES

# 可选配置
local_root=/root/ # 根目录
utf8_filesystem=YES # UTF8 编码支持
ftp_banner=Welcome to my FTP server. # 登录欢迎语
anonymous_enable=NO # 是否允许匿名登陆
local_enable=YES # 是否允许以本地账户登录
write_enable=YES # 是否给予本地账户权限
```

当然还有其他更多的可选配置，读者可以自行百度搜索。

接下来输入 `service vsftpd start` 来启动您的 ftp 服务器。

顺带一提，`systemctl` 指令在该系统下是不可用的，因此您只能使用 `system` 指令来管理您的服务。

在 Windows 下输入 `ftp://用户名:密码@ip:ftp监听端口` 即可访问您的 ftp 服务器。

### 图形桌面的安装

如果您不习惯使用命令行，您还可以在您的 Linux 里安装一个图形界面。

还记得 Tmoe 脚本吗，这东西在当前的 Linux 环境下也是可以使用的。只不过这次比较不同，它先会跳出来一个 `您想要对这个小可爱做什么` 的界面，在这里，我们选择 `Tools` 选项。

又出来了熟悉的界面。这次我们选择第一项 `GUI:图形界面`，再选择 `proot_DE`。接下来您可以选择您喜欢的图形桌面，点击安装即可。

至于如何使用图形桌面，这里我们需要使用一款叫做 VNC Viewer 的软件。直接在百度搜索 VNC Viewer 就能看到官网，在官网进行下载即可，任意操作系统均可使用您 Linux 的图形桌面。

回到 Tmoe 脚本主页，接下来我们来配置远程桌面服务器。选择 `vnc/x/rdp:远程桌面` 选项，再选择 `tightvnc/tigervnc`，自行选择究竟是使用 `tightvnc` 还是 `tigervnc`，直接安装即可。稍后，您就会发现回到了安装前的界面，那就表示已经安装好了。

退出 Tmoe 脚本，输入 `startvnc` 即可开启 VNC 服务。如果您想要直接在本机上访问，在 VNC Viewer 中输入 `127.0.0.1:xxxx` 的那个地址即可，如果您想要在局域网内的其他设备访问，输入除 `127.0.0.1:xxxx` 的其他地址均可。

## 后记

本文写于 2022/2/12，若由于时间原因，导致文章内容出现错误，作者会及时纠正 (应该吧)。

还有在使用 Termux 时，注意一下自己的内存消耗，不然一不小心您的 Termux 就会占用您几十个 G 的空间。

当然，Termux 还有很多的操作都还没有细讲，读者可以自行探索 Termux 的其他操作。