---
title: 在Linux上安裝AEM Forms
description: 瞭解如何安裝適用於AEM Forms的32位元程式庫，以便在Linux安裝上運作。
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

# 安裝共用程式庫的32位元版本

在Linux上部署AEM FORMS OSGi或AEM Forms j2EE時，您必須確保已安裝並可以使用一組共用程式庫的32位元版本。  說明來自套件本身。

* expat （James Clark所撰寫的用於剖析XML的串流導向XML剖析器C程式庫）
* fontconfig （字型組態和自訂程式庫，設計用來在系統中尋找字型，並根據應用程式指定的要求選取字型）
* freetype (字型轉譯引擎，專為提供各種平台和環境的進階字型支援而開發。 它可以開啟和管理字型檔案，以及有效地載入、提示和演算個別字元。 它不是字型伺服器或完整的文字轉譯程式庫)
* glibc （GNU系統和GNU/Linux系統的核心程式庫，以及許多使用Linux作為核心的其他系統）
* libcurl （使用者端URL傳輸程式庫）
* libICE （使用者端間交換程式庫）
* libicu （提供強大且完整功能Unicode和地區設定支援的資料庫 — Unicode的國際元件）。 此程式庫需要64位元和32位元版本
* libSM （X11工作階段管理程式庫）
* libuuid （與DCE相容的通用唯一識別碼程式庫 — 用於產生物件的唯一識別碼，而物件可在本機系統之外存取）
* libX11 （X11使用者端資料庫）
* libXau （X11授權通訊協定 — 可用於限制使用者端存取顯示器）
* libxcb （X通訊協定C語言繫結 — XCB）
* libXext （X11通訊協定常見擴充功能的程式庫）
* libXinerama (X11擴充功能，支援將案頭擴充至多個顯示器。 其名稱是Cinerama的雙關語，這是一種使用多部投影機的寬熒幕電影格式。 libXinerama是與RandR擴充功能介面的程式庫)
* libXrandr （Xinerama擴充功能現在已基本過時，已被RandR擴充功能取代）
* libXrender （X Rendering Extension使用者端程式庫） nss-softokn-freebl （適用於網路安全服務的Freebl程式庫）
* zlib （通用、無專利、無損資料壓縮程式庫）

從Red Hat Enterprise Linux 6開始，程式庫32位元版本的副檔名為。686，而64位元版本的副檔名為.x86_64。 例如，expat.i686。 在RHEL 6之前，32位元版本的副檔名為.i386。 在安裝32位元版本之前，請確定已安裝最新的64位元版本。 如果程式庫的64位元版本比安裝的32位元版本舊，您會收到如下錯誤：

0m錯誤：受保護的multilib版本： libsepol-2.5-10.el7.x86_64 ！= libsepol-2.5-6.el7.i686 [0mError：發現多程式庫版本問題。]

## 首次安裝

在Red Hat Enterprise Linux上，使用YellowDog Update Modifier (YUM)進行安裝，如下所示：

1. yum安裝expat.i686
2. yum安裝fontconfig.i686
3. yum安裝freetype.i686
4. yum安裝glibc.i686
5. yum安裝libcurl.i686
6. yum安裝libICE.i686
7. 百勝安裝libicu.i686
8. 百勝安裝libicu
9. yum安裝libSM.i686
10. yum安裝libuuid.i686
11. yum安裝libX11.i686
12. yum安裝libXau.i686
13. yum安裝libxcb.i686
14. yum安裝libXxt.i686
15. 百勝安裝libXinerama.i686
16. yum安裝libXrandr.i686
17. yum安裝libXrender.i686
18. yum安裝nss-softokn-freebl.i686
19. yum安裝zlib.i686

## 符號連結

此外，您需要建立libcurl.so、libcrypto.so和libssl.so symlink，分別指向最新的32位元版本libcurl、libcrypto和libssl程式庫。 您可以在/usr/lib/ ln -s /usr/lib/libcurl.so.4.5.0 /usr/lib/libcurl.so ln -s /usr/lib/libcrypto.so.1.1.1c /usr/lib/libcrypto.so ln -s /usr/lib/libssl.so.1.1.1c /usr/lib/libssl.so中找到檔案

## 現有系統的更新

在更新過程中，x86_64和i686架構之間可能會發生衝突，例如：錯誤：交易檢查錯誤：安裝glibc-2.28-72.el8的/lib/ld-2.28.so檔案。i686與套件glibc32-2.28-42.1.el8.x86_64的檔案衝突

如果您遇到這種情況，請先解除安裝違規套件，就像在這種情況下：yum remove glibc32-2.28-42.1.el8.x86_64

話雖如此，但您仍希望x86_64和i686版本完全相同，例如從此輸出到指令： yum info glibc

上次中繼資料到期檢查： 0:41:33前於2020年1月18日星期六11:37:東部時間上午08點
已安裝的套件名稱： glibc版本： 2.28版本： 72.el8架構： i686大小： 13 M來源： glibc-2.28-72.el8.src.rpm存放庫： @System從存放庫： BaseOS摘要： GNU libc資料庫URL ： http://www.gnu.org/software/glibc/授權： LGPLv2+和LGPLv2+例外以及GPLv2+例外以及BSD和Inner-Net和ISC公共域和GFDL描述： glibc包中包含由使用的標準庫：系統上的多個程式。 為了節省磁碟空間和：記憶體以及簡化升級作業，通用系統程式碼會保留在同一個位置，並在程式之間共用。 此特定套件：包含最重要的一組共用程式庫：標準C ：程式庫和標準數學程式庫。 如果沒有這兩個程式庫，Linux系統將無法運作。

名稱： glibc版本：2.28版本： 72.el8架構： x86_64大小： 15 M來源： glibc-2.28-72.el8.src.rpm存放庫： @System從存放庫： BaseOS摘要： GNU libc程式庫URL ： http://www.gnu.org/software/glibc/授權： LGPLv2+和LGPLv2+例外以及GPLv2+例外以及BSD、Inner-Net和ISC公共域和GFDL描述： glibc包中包含由使用的標準庫：系統上的多個程式。 為了節省磁碟空間和：記憶體以及簡化升級作業，通用系統程式碼會保留在同一個位置，並在程式之間共用。 此特定套件：包含最重要的一組共用程式庫：標準C ：程式庫和標準數學程式庫。 如果沒有這兩個程式庫，Linux系統將無法運作。

## 一些有用的yum命令

Yum清單已安裝Yum搜尋 [part_of_package_name]
提供以下內容的年份 [package_name]
yum安裝 [package_name]
百勝重新安裝 [package_name]
Yum資訊 [package_name]
yum deplist [package_name]
Yum移除 [package_name]
yum check-update [package_name]
Yum更新 [package_name]
