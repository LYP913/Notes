`目录 start`
 
- [Linux系统](#linux系统)
    - [系统管理](#系统管理)
        - [窗口管理器](#窗口管理器)
        - [文件系统对比](#文件系统对比)
        - [桌面环境对比](#桌面环境对比)
        - [文件管理器对比](#文件管理器对比)
        - [终端模拟器对比](#终端模拟器对比)
        - [用户管理](#用户管理)
        - [用户组管理](#用户组管理)
        - [时间管理](#时间管理)
        - [自启服务管理](#自启服务管理)
    - [软件管理](#软件管理)
        - [软件源列表](#软件源列表)
        - [包管理器](#包管理器)
        - [源码编译安装](#源码编译安装)
    - [终端命令](#终端命令)
        - [Shell内建命令](#shell内建命令)
    - [安装Linux发行版](#安装linux发行版)
- [Tips](#tips)
    - [一行执行多条命令](#一行执行多条命令)
    - [让命令在后台运行](#让命令在后台运行)
        - [关闭ssh回话不能运行](#关闭ssh回话不能运行)
        - [关闭ssh回话仍能运行](#关闭ssh回话仍能运行)
    - [修改主机名](#修改主机名)
- [终端快捷键](#终端快捷键)
    - [Delete](#delete)
    - [Convert](#convert)
    - [Jump](#jump)
    - [Search](#search)
    - [Control](#control)

`目录 end` |_2018-08-29_| [码云](https://gitee.com/gin9) | [CSDN](http://blog.csdn.net/kcp606) | [OSChina](https://my.oschina.net/kcp1104) | [cnblogs](http://www.cnblogs.com/kuangcp)
****************************************
# Linux系统
> 只是记录了debian系的Linux, 不过也是大同小异

## 系统管理
> sudo 其实是软件 早该意识到的，所有的命令都是可执行文件  
> [笔记: 发行版之别](/Linux/Release_Experience.md)

### 窗口管理器
- [awesome window manager](https://awesomewm.org/) `平铺式的`

### 文件系统对比
> [参考博客: 如何选择文件系统：EXT4、Btrfs 和 XFS ](https://linux.cn/article-7083-1.html)

目前 Linux 大多采用 ext3 

### 桌面环境对比
- gnome 占用资源中等，个人对该桌面不感冒
- xfce 占用资源少，操作类似于xp
- kde 功能强大，占用资源中等
    - [知乎 KDE如何配置得漂亮大气？](https://www.zhihu.com/question/54147372)
- dde deepin设计的桌面环境，小bug略多，但是美观操作方便, 但是扩展性不好

***************************
### 文件管理器对比
> 有单窗口，双列，命令，简洁轻量，笨重完整 各种各样的选择

- `nautilus` Gnome默认 挺好用，但是不能自动挂载分区
- `deepin-filemanager` deepin默认，较为方便，但是打开手机会卡根本打不开
- `pcmanfm` 左边侧栏目录树 会同步nautilus的配置`5m`
- `rox-filer` 特别小，单击打开，迅速定位文件，适合找东西用
- `thunar` 解决了nautilus的缺点，内存也很省 `21M`
- `dolphin` 多标签页，目录树方式查看
- `nemo` mint默认的，功能齐全，会同步nautilus的配置，同样有目录树而且是两边都有 `21M`
- `tuxcmd` Tux Commander 双列，小，直接的目录树，学习成本高点 `2M`

*******************************
### 终端模拟器对比
> [详细](/Skills/Soft/Terminal.md)

**************************************
### 用户管理
- 添加用户 `sudo adduser username` 
    - 对比 `useradd`只是新建一个用户不会创建主目录
- 添加到sudo组 ，使用命令更安全：`sudo gpasswd -a $USER sudo` 但是要注销或者重启才生效貌似
- 或者：添加用户到用户组：`adduser user group`
    -  或者：使用修改文件的方式：（不推荐） 但是在docker中跑Ubuntu新建用户时很有用，也可以不用动文件，添加进组是有效的，看情况吧
    - `chmod 777 /etc/sudoers`  然后直接`sudo visudo`就是调用vi来打开文件的简写
        - 找不到文件说明没有安装sudo -> root用户 `apt install sudo `
    - 添加一行 `kuang  ALL=(ALL:ALL)ALL` Centos:`kuang   ALL=(ALL)       ALL`
    - `chmod 440 /etc/sudoers`
        - `rwx 对应一个三位的二进制数， 1/0 表示开关`
- 查看是否设置成功 ： `groups username`
- 删除用户以及对应的home目录：`sudo deluser username --remove-home` 

*****
- _切换用户_ `su` 
- `su -l username` 当前用户的环境下登录用户（当成一个程序一样可以退出登录）

*****
- _修改密码_ `passwd`
    - `passwd user`
    - `echo "root:caishi" | chpasswd` 如果是普通用户就是 sudo chpasswd
*****
- _修改相关信息_ `usermod` 

| verb | long verb | comment |
|:----:|:----|:----|
| -d | --home HOME_DIR          |  用户的新主目录 |
| -e | --expiredate EXPIRE_DATE |  设定帐户过期的日期为 EXPIRE_DATE |
| -f | --inactive INACTIVE      |  过期 INACTIVE 天数后，设定密码为失效状态 |
| -g | --gid GROUP              |  强制使用 GROUP 为新主组 |
| -G | --groups GROUPS          |  新的附加组列表 GROUPS |
| -a | --append GROUP           |  将用户追加至上边 -G 中提到的附加组中，并不从其它组中删除此用 |户
| -l | --login LOGIN            |  新的登录名称 |
| -L | --lock                   |  锁定用户帐号 |
| -m | --move-home              |  将家目录内容移至新位置 (仅于 -d 一起使用) |
| -p | --password PASSWORD      |  将加密过的密码 (PASSWORD) 设为新密码 |
| -R | --root CHROOT_DIR        |  chroot 到的目录 |
| -s | --shell SHELL            |  该用户帐号的新登录 shell |
| -U | --unlock                 |  解锁用户帐号 |
> [所有参数说明](https://gitee.com/kcp1104/codes/gca14wtqvm67l9j5r0deb56#usermod.md)

******
- `passwd 选项 用户名` 更改口令(密码)
    - `-l 锁定口令，禁用账号`  `-u 口令解锁` `-d 账号无口令` `-f 强迫用户下次登录时修改口令`
    - 当前用户 `passwd` 就是修改当前用户口令 超级用户就可以命令后接用户名，修改任意用户

******
- pwcov 注：同步用户从/etc/passwd 到/etc/shadow
- pwck 注：pwck是校验用户配置文件/etc/passwd 和/etc/shadow 文件内容是否合法或完整;
- pwunconv 注：是pwcov 的立逆向操作，是从/etc/shadow和 /etc/passwd 创建/etc/passwd ，然后会删除 /etc/shadow 文件;
- finger 注：查看用户信息工具
- id 注：查看用户的UID、GID及所归属的用户组
- chfn 注：更改用户信息工具
- visudo 注：visodo 是编辑 /etc/sudoers 的命令;也可以不用这个命令，直接用vi 来编辑 /etc/sudoers 的效果是一样的;

### 用户组管理
> [相关总结网页](http://www.runoob.com/linux/linux-user-manage.html)

- 修改用户至指定组 `sudo usermod -G 用户组 用户`
- _显示用户所在组_ `groups`
    - 缺省是当前用户, 若指定即输出指定用户的用户组
- _添加用户组_ `groupadd`
    - 缺省参数 就是新建用户组
    - `-g GID` 指定新用户组的组标识号GID 
    - `-o` 一般和g共用 表示新用户组的GID可以与系统已有用户组的GID相同。

- _删除用户组_ `groupdel` 

- `groupmod 选项 用户组`
    - -g GID 为用户组指定新的组标识号。
    - -o 与-g选项同时使用，用户组的新GID可以与系统已有用户组的GID相同。
    - -n 新用户组 将用户组的名字改为新名字

- grpck 检查`/etc/group`文件是否正确
- grpconv 注：通过/etc/group和/etc/gshadow 的文件内容来同步或创建/etc/gshadow ，如果/etc/gshadow 不存在则创建;
-  注：通过/etc/group 和/etc/gshadow 文件内容来同步或创建/etc/group ，然后删除gshadow文件
### 时间管理
> [同步Linux服务器时间](http://www.cnblogs.com/chenmh/p/5485829.html)

**同步时间**
1. 修改时区 `cp -y /usr/share/zoneinfo/Asia/Shanghai /etc/localtime`
2. 同步时间 `/usr/sbin/ntpdate -u cn.pool.ntp.org`
3. 查看硬件时间 `hwclock -r`
    - 如果不同步就需要写入时间 `hwclock -w` _因为系统重启是参考硬件时间的_

**自动同步时间**
1. 配置开机自动校验 `vim /etc/rc.d/rc.local`
    - `/usr/sbin/ntpdate -u cn.pool.ntp.org> /dev/null 2>&1; /sbin/hwclock -w`
2. 配置定时任务 `crontab -e`
    - `00 10 * * * root /usr/sbin/ntpdate -u cn.pool.ntp.org > /dev/null 2>&1; /sbin/hwclock -w `

### 自启服务管理
> /etc/init.d/ 是服务的存放目录
1. 列出所有服务的状态 service --status-all

1. 移除MySQL的自启 `sudo update-rc.d -f mysql remove`
2. 设置MySQL随机启动 `sudo update-rc.d mysql defaults`
3. 设定MySQL启动顺序 `update-rc.d mysql defaults 90` 数字越小, 启动顺序越前

- sysv-rc-conf  略微图形化的管理服务的开机自启
- chkconfig 简单的输出服务自启状态

_系统运行级别_
```
    0        系统停机状态
    1        单用户或系统维护状态
    2~5      多用户状态
    6        重新启动 
```
******************
## 软件管理
### 软件源列表
- apt 的默认配置文件是 `/etc/apt/source.list`
    - 以及 sources.list.d/ 目录下的 *.list 文件 (最好将list文件都进行备份 备份文件为 *.save)

- [参考博客 阿里云的软件源](https://hacpai.com/article/1482807364546?p=1&m=0)
- [wiki-源列表说明](http://wiki.ubuntu.com.cn/%E6%BA%90%E5%88%97%E8%A1%A8)

1. 源 URL 后的单词: 
    1. main: 完全的自由软件。
    1. restricted: 不完全的自由软件。
    1. universe: Ubuntu官方不提供支持与补丁，全靠社区支持。
    1. multiverse: 非自由软件，完全不提供支持和补丁。

1. 添加私有源ppa
    - 若不能添加私有源ppa：
        - debain：`sudo apt install software-properties-common python-software-properties`
        - Ubuntu `sudo apt install python-software-properties`
    - 添加：`sudo add-apt-repository ppa:dotcloud/lxc-docker `
	- 删除ppa : `cd  /etc/apt/sources.list.d/` 打开该目录下文件把对应的ppa的一行注释掉或删掉就行了


1. 添加一个源列表

- 例如添加 nginx: 新建文件 `/etc/apt/sources.list.d/nginx.list` 
```
    deb http://nginx.org/packages/mainline/debian/ jessie nginx
    deb-src http://nginx.org/packages/mainline/debian/ jessie nginx
```
- `curl http://nginx.org/keys/nginx_signing.key | apt-key add -`
    - 把签名添加进来才能正常 apt update

### 包管理器
> dpkg
1. 查看已安装的应用 `dpkg --list`
1. 显示已安装包的详情 `dpkg -s package`
1. 安装deb包
	- ` sudo  dpkg  -i  *.deb`

> apt-get / apt 
1. `install 包名`  安装指定包的最新版
    - `-y` 参数可以省去确认
    - `-s` 模拟安装
    - `package=version` 安装指定版本的包
1. list 列出所有可安装的包
    - package 列出已安装的 该package 的信息 `加上 -a`: 所有版本

1. 只卸载程序，保留配置文件 `sudo apt remove 应用名`
1. 彻底卸载应用 `sudo apt--purge remove 应用名`

- apt-cache showpkg/policy/madison/show package
    - showpkg (特别详细) 列出所有版本以及来源, MD5 ...
    - policy (基本信息) 列出所有版本以及来源
    - madison (简略显示) 内容同上
    - show 查询指定包的详情(已安装的版本信息)

> snap
- [official doc: snap](https://snapcraft.io/docs/core/usage) 

### 源码编译安装
1. make install 源代码安装
    - 1.解压缩 `tar -zxf nagios-4.0.2.tar.gz ` 
    - 2.进入目录 `cd nagios-4.0.2`
    - 3.配置 `./configure --prefix=/usr/local/nagios  ` 
    - 4.编译 `make all`
    - 5.安装 `make install && make install-init && make install-commandmode && make install-config`

**********************************************
## 终端命令
> /bin/* 系统自带的命令
例如 which命令, 查找到命令的位置

> /usr/bin/* 用户安装终端应用的目录 以下往往是系统自带的
- wc -l file _统计文件行数_
- md5sum 报文摘要算法 

### Shell内建命令
- whence 查看命令的真实面貌 (zsh中的内建命令)
- where 查找命令的位置 (Zsh中内建命令)

> [更多常用工具列表](/Linux/Tool/Terminal.md)
*****************************************************
## 安装Linux发行版
- 下载指定的镜像包，使用对应的刻录软件刻录U盘(Windows就是软碟通,Linux没怎么用过,只用过深度的U盘启动盘制作工具挺好的)
- 进入U盘安装模式，分区：
    - 分配 1/5 的 `/` ext4
    - 分配 3/5 的 `/home` ext4
    - 分配 500-1000m 的 `/boot/efi` fat32格式
- 如果是双系统:
    - 如果新手直接全部` / `就行了，再加个交换分区 
    - 如果为了日后重装系统方便,那么分两个区 `/` 和 `/home`
        - 这样的话,就建议大量软件使用解压版,这样重装系统带来的影响最小,那么`/home`就要分大一点
        - 例如我: `/`只用了22G `/home`用了40G(公司的Deepin分了100G /home 用了75G了)

> 新手的话特别注意不要随意用sudo然后更改配置文件，容易导致系统crash（除非你明确的知道这个更改操作的作用）

*****************************************************
# Tips
> man help 后接使用的命令，就可以得到用户手册和帮助文档

## 一行执行多条命令 
- ` && ` 第2条命令只有在第1条命令成功执行之后才执行 根据命令产生的退出码判断是否执行成功（0成功，非0失败）
- `|| ` 执行不成功（产生了一个非0的退出码）时，才执行后面的命令
- ` ; ` 顺序执行多条命令，当;号前的命令执行完（不管是否执行成功），才执行;后的命令。 
- ` & `  并行执行命令，没有顺序

- [tty 虚拟终端等概念](https://www.ibm.com/developerworks/cn/linux/l-cn-termi-hanzi/)

- Centos上which并不是命令, 而是别名!
    - `which='alias | /usr/bin/which --tty-only --read-alias --show-dot --show-tilde'`

**************
## 让命令在后台运行
> [原博客](https://www.ibm.com/developerworks/cn/linux/l-cn-nohup/)

- 命令后接 & （只是让进程躲到当前终端的后台去了 hup信号仍然影响）

`nohup， disown, screen, setid `
- 运行的命令不因 用户注销，网络中断等因素而中断
    - 让进程对hup信号免疫 nohup disown
    - 让进程在新的会话中运行 setid screen

### 关闭ssh回话不能运行
> 1.没有使用任何修饰原有命令  
> 2.只在原有命令后加&

### 关闭ssh回话仍能运行
- 使用`nohup`就能屏蔽hup信号，默认输出到 nohup.out `nohup 命令 &`
    - 将所有输出重定向到空设备  `nohup 命令>/dev/null 2>&1`
    - 例如 在当前目录后台打开文件管理器 `(dde-file-manager . &) >/dev/null 2>&1`

- `(命令 &)` 屏蔽了hup信号

*************
## 修改主机名
- `sudo hostname linux` 重启终端即可看到修改
- 但是重启电脑会恢复原有名字修改如下文件永久： `sudo gedit /etc/hostname` 也许需要更改`/etc/hosts`
- 立即生效,也要重新登录 `hostname -F /etc/hostname `

*************************
# 终端快捷键

- `鼠标中键` 粘贴鼠标左键已选择的文本 **VSCode中也适用**
- `!num` history 中第 num 条命令
- `!!` 上一条命令
- `ls !$` 执行命令ls，并以上一条命令的参数为其参数
- `!?string?` 执行含有string字符串的最新命令
- `Ctrl L` 清屏等价于clear，清除所有这个 shell 提示屏幕中显示的数据。 `Mysql也适用`
- `reset` 刷新 shell 提示屏幕。如果字符不清晰或乱码的话，在 shell 提示下键入这个命令会刷新屏幕。
- `Ctrl ；` 显示最近五条剪贴板内容
- Ctrl Alt Backspace : 杀死你当前的 X 会话。杀死图形化桌面会话，把你返回到登录屏幕。如果正常退出步骤不起作用，你可以使用这种方法。
- Ctrl Alt Delete : 关机和重新引导 Red Hat Linux。关闭你当前的会话然后重新引导 OS。只有在正常关机步骤不起作用时才使用这种方法。
- Ctrl Alt Fn: 切换屏幕。 根据默认设置，从 [F1] 到 [F6] 是 shell 提示屏幕， [F7] 是图形化屏幕。`但是deepin是F1为图形化`

## Delete
| Controller | Key | comment |
|:---|:----|:----|
| Ctrl | D | 删除光标后字符,等价于Delete键（命令行若无任何字符，则相当于exit；处理 多行标准输入时也表示EOF） |
| Ctrl | H | 退格删除一个字符，相当于通常的Backspace键 |
| Ctrl | U | 删除光标之前到 行首 的字符 (Zsh中是删除整行)|
| Esc | W | 删除光标之前到 行首 的字符|
| Ctrl | K | 删除光标之前到 行尾 的字符 |
| Ctrl | W | 删除光标之前的一个单词 |
| Alt | D | 删除光标之后的一个单词 |
| Ctrl | Y | 粘贴上次删除的所有字符 |
| Ctrl | _ | 撤销修改 等价于 `Ctrl x u` |

**************************
## Convert
| Controller | Key | comment |
|:---|:----|:----|
| Ctrl | T | 互换当前字符,光标后移 |
| Alt | T | 互换当前单词与前一个单词,光标后移 等价于 `Esc T`|
| Alt | D | 将当前单词全部转为大写,光标后移 |
| Alt | C | 将当前单词首字母转为大写,光标后移 |
| Alt | L | 将当前单词全部转为小写,光标后移(zsh无效) |

*******************
## Jump
| Controller | Key | comment |
|:---|:----|:----|
| Ctrl | C | 取消运行当前行输入的命令，相当于Ctrl + Break |
| Ctrl | A | 光标移动到行首（Ahead of line），相当于通常的Home键 |
| Ctrl | E | 光标移动到行尾（End of line） |
| Ctrl | F | 光标向前(Forward)移动一个字符位置 |
| Ctrl | B | 光标往回(Backward)移动一个字符位置 |
| Alt | F | 光标向前(Forward)移动一个单词位置 |
| Alt | B | 光标往回(Backward)移动一个单词位置 |
| Esc | F | 光标向前(Forward)移动到当前单词的头部 |
| Esc | B | 光标往回(Backward)移动到当前单词的尾部 |

*****************************
## Search
| Controller | Key | comment |
|:---|:----|:----|
| Ctrl | P | 调出命令历史中的前一条（Previous）命令，相当于通常的上箭头 |
| Ctrl | N | 调出命令历史中的下一条（Next）命令，相当于通常的下箭头 |
| Ctrl | O | 运行上翻下翻出来的命令, 并且自动将下一条命令填入 |
| Ctrl | R | 向上搜索相关命令（reverse-i-search）继续按 Ctrl R 则继续搜索上一条 |
| Ctrl | S | 与 Ctrl R 类似, 但是是向下搜索 |

**************************
## Control
| Controller | Key | comment |
|:---|:----|:----|
|Ctrl |Z|暂停程序 |
| Ctrl | S | 停止回显当前Shell |
| Ctrl | Q | 恢复回显当前Shell |
