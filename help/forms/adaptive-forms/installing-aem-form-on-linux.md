---
title: 在Linux上安装AEM Forms
description: 了解如何安装适用于AEM Forms的32位库，以便在Linux上正常运行。
feature: Adaptive Forms
type: Tutorial
version: 6.4, 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-7593
exl-id: b9809561-e9bd-4c67-bc18-5cab3e4aa138
last-substantial-update: 2019-06-09T00:00:00Z
duration: 216
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '938'
ht-degree: 0%

---

# 安装32位版本的共享库

在Linux上部署AEM FORMS OSGi或AEM Forms j2EE时，您必须确保安装一组共享库的32位版本并且这些版本可用。  说明来自包本身。

* expat（由James Clark编写的用于解析XML的面向流的XML解析器C库）
* fontconfig（字体配置和定制库，用于在系统中定位字体，并根据应用程序指定的要求选择字体）
* freetype(字体渲染引擎，开发用于为各种平台和环境提供高级字体支持。 它可以打开和管理字体文件，以及有效地加载、提示和渲染单个字形。 它不是字体服务器或完整的文本渲染库)
* glibc（用于GNU系统和GNU/Linux系统的核心库，以及许多使用Linux作为内核的其他系统）
* libcurl（客户端URL传输库）
* libICE（客户端间交换库）
* libicu（提供强大且功能齐全的Unicode和区域设置支持的库 — Unicode的国际组件）。 此库需要64位和32位版本
* libSM （X11会话管理库）
* libuuid（DCE兼容的通用唯一标识符库 — 用于为可能在本地系统之外访问的对象生成唯一标识符）
* libX11（X11客户端库）
* libXau （X11授权协议 — 用于限制客户端访问显示器）
* libxcb （X协议C语言绑定 — XCB）
* libXext（用于X11协议常用扩展的库）
* libXinerama (X11扩展，支持将桌面扩展到多个显示器。 这个名字是Cinerama上的一个双关语，这是一种使用多个投影机的宽屏电影格式。 libXinerama是与RandR扩展进行接口的库)
* libXrandr（Xinerama扩展现已基本过时，已被RandR扩展所取代）
* libXrender （X Rendering Extension客户端库）nss-softokn-freebl（用于网络安全服务的Freebl库）
* zlib（通用、无专利、无损数据压缩库）

从Red Hat Enterprise Linux 6开始，库的32位版本的文件扩展名为。686，而64位版本的文件扩展名为.x86_64。 例如，expat.i686。 在RHEL 6之前，32位版本的扩展名为.i386。 在安装32位版本之前，请确保安装了最新的64位版本。 如果库的64位版本比正在安装的32位版本旧，您将收到如下错误：

0mError：受保护的多库版本： libsepol-2.5-10.el7.x86_64 ！= libsepol-2.5-6.el7.i686 [0mError：发现多库版本问题。]

## 首次安装

在Red Hat Enterprise Linux上，使用YellowDog更新修饰符(YUM)进行安装，如下所示：

1. yum安装expat.i686
2. yum install fontconfig.i686
3. yum安装freetype.i686
4. yum安装glibc.i686
5. yum安装libcurl.i686
6. yum安装libICE.i686
7. 百胜安装libicu.i686
8. 百年安装libicu
9. yum安装libSM.i686
10. yum安装libuuid.i686
11. yum安装libX11.i686
12. yum安装libXau.i686
13. yum安装libxcb.i686
14. yum安装libXt.i686
15. 百胜安装libXinerama.i686
16. yum安装libXrandr.i686
17. yum安装libXrender.i686
18. yum安装nss-softokn-freebl.i686
19. yum安装zlib.i686

## 符号链接

此外，您需要创建libcurl.so、libcrypto.so和libssl.so symlink以分别指向libcurl、libcrypto和libssl库的最新32位版本。 您可以在/usr/lib/ ln -s /usr/lib/libcurl.so.4.5.0 /usr/lib/libcurl.so ln -s /usr/lib/libcrypto.so.1.1.1c /usr/lib/libcrypto.so ln -s /usr/lib/libssl.so.1.1.1c /usr/lib/libssl.so中找到这些文件

## 现有系统的更新

在更新过程中，x86_64和i686体系结构之间可能存在冲突，例如：错误：事务检查错误：文件/lib/ld-2.28.so（来自glibc-2.28-72.el8.i686）与包glibc32-2.28-42.1.el8.x86_64中的文件冲突

如果遇到这种情况，请首先卸载有问题的包，如本例中：yum remove glibc32-2.28-42.1.el8.x86_64

话虽如此，但您仍希望x86_64和i686版本完全相同，例如从以下输出到命令： yum info glibc

上次元数据过期检查：0:41:33前于2020年1月18日星期六11:37:东部标准时间上午8点。
已安装的包名称：glibc版本：2.28版本：72.el8架构：i686大小：13 M源：glibc-2.28-72.el8.src.rpm存储库：@System从存储库：BaseOS摘要：GNU libc库URL：http://www.gnu.org/software/glibc/许可证：LGPLv2+和LGPLv2+（例外）；GPLv2+和GPLv2+（例外）；BSD和Inner-Net和ISC公共域和GFDL描述：glibc包中包含由使用的标准库：系统上的多个程序。 为了节省磁盘空间和：内存，以及简化升级，将通用系统代码：保留在一个位置并在程序之间共享。 此特定包：包含最重要的共享库集：标准C ：库和标准数学库。 如果没有这两个库，则Linux系统将无法正常运行。

名称：glibc版本：2.28发行版：72.el8体系结构：x86_64大小：15 M源：glibc-2.28-72.el8.src.rpm存储库：@System从存储库：BaseOS摘要：GNU libc库URL：http://www.gnu.org/software/glibc/许可证：LGPLv2+和LGPLv2+（例外）；GPLv2+和GPLv2+（例外）；BSD、Inner-Net和ISC公共域和GFDL描述：Glibc包中包含由使用的标准库：系统上的多个程序。 为了节省磁盘空间和：内存，以及简化升级，将通用系统代码：保留在一个位置并在程序之间共享。 此特定包：包含最重要的共享库集：标准C ：库和标准数学库。 如果没有这两个库，则Linux系统将无法正常运行。

## 一些有用的yum命令

Yum列表已安装Yum搜索 [part_of_package_name]
提供以下内容的年份 [package_name]
百年安装 [package_name]
百胜重新安装 [package_name]
年信息 [package_name]
百胜去处 [package_name]
删除yum [package_name]
yum check-update [package_name]
百年更新 [package_name]
