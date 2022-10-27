---
title: 在Linux上安装AEM Forms
description: 了解如何安装32位库以便AEM Forms在Linux安装上工作。
feature: Adaptive Forms
type: Tutorial
version: 6.4, 6.5
topic: Development
role: Developer
level: Beginner
kt: 7593
exl-id: b9809561-e9bd-4c67-bc18-5cab3e4aa138
last-substantial-update: 2019-06-09T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '945'
ht-degree: 0%

---

# 安装32位版本的共享库

在AEM FORMS上部署OSGi或AEM Forms j2EE时，您必须确保安装并提供一组共享库的32位版本。  描述来自包本身。

* expat（面向流的XML解析器C库，用于解析XML，由James Clark编写）
* fontconfig（字体配置和自定义库，旨在查找系统内的字体，并根据应用程序指定的要求选择它们）
* freetype(字体渲染引擎，为各种平台和环境提供高级字体支持而开发。 它可以打开和管理字体文件，以及有效地加载、提示和渲染单个字形。 它不是字体服务器或完整的文本渲染库)
* glibc（用于GNU系统和GNU/Linux系统的核心库，以及许多使用Linux作为内核的其他系统）
* libcurl（客户端URL传输库）
* libICE（客户端间Exchange库）
* libicu（提供强大且功能齐全的Unicode和区域设置支持的库 — Unicode国际组件）。 此库的64位和32位版本都是必需的
* libSM（X11会话管理库）
* libuuid（DCE兼容的通用唯一标识符库，用于为可能在本地系统之外可访问的对象生成唯一标识符）
* libX11（X11客户端库）
* libXau（X11授权协议 — 可用于限制客户端对显示屏的访问）
* libxcb（X协议C — 语言绑定 — XCB）
* libXext（用于X11协议的常见扩展的库）
* libXinerama(X11扩展，支持跨多个显示屏扩展桌面。 Cinerama是一种使用多台投影仪的宽屏电影格式。 libXinerama是与RandR扩展接口的库)
* libXrandr（Xinerama扩展现在已基本过时 — 它已被RandR扩展所取代）
* libXrender（X渲染扩展客户端库）nss-softokn-freebl（用于网络安全服务的自由库）
* zlib（通用、无专利、无损的数据压缩库）

从Red Hat Enterprise Linux 6开始，库的32位版本的文件扩展名为。686，而64位版本的扩展名为.x86_64。 例如expat.i686。 在RHEL 6之前，32位版本的扩展名为.i386。 在安装32位版本之前，请确保安装了最新的64位版本。 如果库的64位版本比所安装的32位版本旧，则会收到如下错误：

0m错误：受保护的多库版本：libsepol-2.5-10.el7.x86_64 != libsepol-2.5-6.el7.i686 [0m错误：发现多库版本问题。]

## 首次安装

在Red Hat Enterprise Linux上，使用YellowDog Update Modifier(YUM)进行安装，如下所示：

1. yum install expat.i686
2. yum install fontconfig.i686
3. yum install freetype.i686
4. yum install glibc.i686
5. yum install libcurl.i686
6. yum install libICE.i686
7. yum install libicu.i686
8. 百胜安装
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

## 符号链接

此外，您还需要创建libcurl.so、libcrypto.so和libssl.so符号链接，分别指向libcurl、libcrypto和libssl库的最新32位版本。 您可以在/usr/lib/ ln -s /usr/lib/libcurl.so.4.5.0 /usr/lib/libcurl.so ln -s /usr/lib/libcrypto.so.1.1.1c /usr/lib/libcrypto.so ln -s /usr/lib/libssl.so.1.1.1c /usr/lib/libssl.so中找到文件

## 对现有系统的更新

在更新期间，x86_64和i686体系结构之间可能会发生冲突，例如：错误：交易检查错误：安装glibc-2.28-72.el8.i686的文件/lib/ld-2.28.so与包glibc32-2.28-42.1.el8.x86_64中的文件冲突

如果遇到此问题，请先卸载违规包，如下所示：yum remove glibc32-2.28-42.1.el8.x86_64

所有说完，您希望x86_64和i686版本完全相同，例如从此输出到命令：yom info glibc

上次元数据过期检查：0:41:33年1月18日星期六11:37:东部时间上午08点。
已安装的包名称：glibc版本：2.28版本：72.el8架构：i686大小：13M来源：glibc-2.28-72.el8.src.rpm存储库：@System从存储库：BaseOS摘要：GNU libc库URL :http://www.gnu.org/software/glibc/许可证：LGPLv2+和LGPLv2+（含例外）以及GPLv2+和GPLv2+（含例外）以及BSD和内网、ISC和公共域和GFDL描述：glibc包包含由使用的标准库：系统上有多个程序。 为了节省磁盘空间和：内存，为方便升级，常用系统代码为：放在一个地方，并在各项目之间共享。 此特定包：包含最重要的共享库集：标准C :库和标准数学库。 如果没有这两个库，即：Linux系统无法正常运行。

名称：glibc版本：2.28版本：72.el8架构：x86_64大小：15M来源：glibc-2.28-72.el8.src.rpm存储库：@System从存储库：BaseOS摘要：GNU libc库URL :http://www.gnu.org/software/glibc/许可证：LGPLv2+和LGPLv2+（含例外）以及GPLv2+和GPLv2+（含例外）以及BSD和内网、ISC和公共域和GFDL描述：glibc包包含由使用的标准库：系统上有多个程序。 为了节省磁盘空间和：内存，为方便升级，常用系统代码为：放在一个地方，并在各项目之间共享。 此特定包：包含最重要的共享库集：标准C :库和标准数学库。 如果没有这两个库，即：Linux系统无法正常运行。

## 一些有用的yum命令

已安装yum list的yum搜索 [part_of_package_name]
yum whatprovies [package_name]
yum安装 [package_name]
重新安装 [package_name]
yum信息 [package_name]
百味食腐 [package_name]
yum remove [package_name]
yum check update [package_name]
yum更新 [package_name]
