---
title: 在Linux上安装AEM Forms
description: 了解如何为AEM Forms安装32位库以在Linux安装中工作。
feature: 自适应表单
audience: developer
doc-type: article
activity: setup
version: 6.4, 6.5
topic: 开发
role: Developer
level: Beginner
kt: 7593
translation-type: tm+mt
source-git-commit: 9583006352ca6a20a763c9d5ec7ba15c3791e897
workflow-type: tm+mt
source-wordcount: '948'
ht-degree: 0%

---


# 安装32位版本的共享库

当AEM FORMS.在Linux上部署OSGi或AEM Forms j2EE时，您必须确保安装并提供一组共享库的32位版本。  说明来自包本身。

* expat（面向流的XML分析器C库，用于分析XML，由James Clark编写）
* fontconfig（字体配置和自定义库，设计用于在系统中查找字体并根据应用程序指定的要求选择它们）
* freetype(字体渲染引擎，开发目的是为各种平台和环境提供高级字体支持。 它可以打开和管理字体文件，并有效地加载、提示和渲染各种字形。 它不是字体服务器或完整的文本渲染库)
* glibc（用于GNU系统和GNU/Linux系统的核心库，以及许多使用Linux作为内核的其他系统）
* libcurl（客户端URL传输库）
* libICE（客户端间Exchange库）
* libicu（提供强大且功能齐备的Unicode和区域设置支持的库 — Unicode国际组件）。 此库的64位和32位版本都是必需的
* libSM（X11会话管理库）
* libuuid(数字内容编辑器兼容的全局唯一标识符库 — 用于为可访问本地系统之外的对象生成唯一标识符)
* libX11（X11客户端库）
* libXau（X11授权协议 — 可用于限制客户端对显示屏的访问）
* libxcb（X协议C语言绑定 — XCB）
* libXext（用于X11协议的常见扩展的库）
* libXinerama（X11扩展），支持跨多个显示器扩展桌面。 Cinerama是一种使用多张投影仪的宽屏电影格式。 libXinerama是与RandR扩展接口的库)
* libXrandr（Xinerama扩展现在已基本过时 — 它已被RandR扩展所取代）
* libXrender（X Rendering Extension客户端库）
nss-softokn-freebl（用于网络安全服务的Freebl库）
* zlib（通用、无专利、无损数据压缩库）

从Red Hat Enterprise Linux 6开始，库的32位版本的文件扩展名为。686，而64位版本的扩展名为.x86_64。 例如，expat.i686。 在RHEL 6之前，32位版本的扩展名为.i386。 在安装32位版本之前，请确保安装了最新的64位版本。 如果库的64位版本比正在安装的32位版本旧，您将收到如下错误：

0mError:受保护的多库版本：libsepol-2.5-10.el7.x86_64 != libsepol-2.5-6.el7.i686 [0m错误：发现多库版本问题。]

## 首次安装

在Red Hat Enterprise Linux上，使用YellowDog Update Modifier(YUM)安装，如下所示：

1. yum install expat.i686
2. yum install fontconfig.i686
3. yum install freetype.i686
4. yum install glibc.i686
5. yum install libcurl.i686
6. yum install libICE.i686
7. yum install libicu.i686
8. yum install libicu
9. yum install libSM.i686
10. yum install libuuid.i686
11. yum install libX11.i686
12. yum install libXau.i686
13. yum install libxcb.i686
14. yum install libXext.i686
15. yum install libXinerama.i686
16. yum install libXrandr.i686
17. yum install libXrender.i686
18. yum install nss-softokn-freebl.i686
19. yum install zlib.i686

## Symlinks

此外，您还需要创建libcurl.so、libcrypto.so和libssl.so符号链接，它们分别指向libcurl、libcrypto和libssl库的最新32位版本。 您可以在/usr/lib/中找到文件
ln -s /usr/lib/libcurl.so.4.5.0 /usr/lib/libcurl.so
ln -s /usr/lib/libcrypto.so.1.1.1c /usr/lib/libcrypto.so
ln -s /usr/lib/libssl.so.1.1.1c /usr/lib/libssl.so

## 对现有系统的更新

在更新期间，x86_64和i686架构之间可能会发生冲突，例如：
错误：事务检查错误：
文件/lib/ld-2.28.so（来自glibc-2.28-72.el8.i686）与来自glibc32-2.28-42.1.el8.x86_64包的文件冲突

如果遇到此问题，请首先卸载违规包，如下例所示：
yum remove glibc32-2.28-42.1.el8.x86_64

所有说完，您希望x86_64和i686版本完全相同，例如，从此输出到命令：
yum info glibc

上次元数据过期检查：0:41:33 2020年1月18日星期六早11:37:08点
已安装的包
名称：glibc
版本：2.28
版本：72.el8
架构：i686
大小：13 M
来源：glibc-2.28-72.el8.src.rpm
存储库：@System
从回购：BaseOS
摘要：GNU libc库
URL:http://www.gnu.org/software/glibc/
许可：LGPLv2+和LGPLv2+（存在例外），GPLv2+和GPLv2+（存在例外），BSD和Inner-Net以及ISC和Public Domain和GFDL
说明：glibc包包含标准库，它们由：系统上的多个项目。 为了节省磁盘空间和：内存，而且使升级更加容易，通用系统代码是：放在一个地方，在项目之间共享。 此特定包：包含最重要的共享库集：标准C:库和标准数学库。 没有这两个库，a :Linux系统无法正常工作。

名称：glibc
版本：2.28
版本：72.el8
架构：x86_64
大小：15 M
来源：glibc-2.28-72.el8.src.rpm
存储库：@System
从回购：BaseOS
摘要：GNU libc库
URL:http://www.gnu.org/software/glibc/
许可：LGPLv2+和LGPLv2+（存在例外），GPLv2+和GPLv2+（存在例外），BSD和Inner-Net以及ISC和Public Domain和GFDL
说明：glibc包包含标准库，它们由：系统上的多个项目。 为了节省磁盘空间和：内存，而且使升级更加容易，通用系统代码是：放在一个地方，在项目之间共享。 此特定包：包含最重要的共享库集：标准C:库和标准数学库。 没有这两个库，a :Linux系统无法正常工作。

## 一些有用的yum命令

yum列表已安装
yum search [part_of_package_name]
yum whatprovies [package_name]
yum install [package_name]
yum reinstall [package name]
yum info [package_name]
yum deplist [package_name]
yum remove [package_name]
yum check-update [package_name]
yum update [package_name]
