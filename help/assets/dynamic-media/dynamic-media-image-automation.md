---
title: 用于透明度和内容自动化批处理的Dynamic Media
description: 了解如何使用AEM中的Dynamic Media创建虚拟演绎版、管理透明度并自动处理图像以实现可扩展内容重用。
feature: Image Profiles, Viewer Presets
topic: Content Management
role: User
level: Beginner, Intermediate, Experienced
doc-type: Feature Video
duration: 560
last-substantial-update: 2025-05-28T00:00:00Z
jira: KT-18197
source-git-commit: a509b7c41e6adb05e6c3b791ac2e99f635973322
workflow-type: tm+mt
source-wordcount: '262'
ht-degree: 6%

---


# 用于透明度和内容自动化批处理的Dynamic Media

了解如何使用AEM中的Dynamic Media创建虚拟演绎版、管理透明度并自动处理图像以实现可扩展内容重用。

>[!VIDEO](https://video.tv.adobe.com/v/3463055/?learn=on&enablevpops&captions=chi_hans)


## Dynamic Media资源示例

以下是视频中使用的Dynamic Media资产及其URL示例。

>[!BEGINTABS]

>[!TAB 图像透明度示例]

以下是视频中使用的Dynamic Media图像服务器示例URL。

| 预览 | 描述 | Dynamic Media URL |
|-----------|------------------|---------|
| ![默认](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20trans?bgc=255,255,255){width="250"} | 默认 | [链接](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20trans?bgc=255,255,255) |
| ![与无缝背景图像图层复合](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20trans?&amp;layer=1&amp;src=backdrop5-Camera&amp;size=8500,8500&amp;layer=2&amp;src=AdobeStock_322150086%20trans){width="250"} | 与无缝背景图像层组合 | [链接](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20trans?&amp;layer=1&amp;src=backdrop5-Camera&amp;size=8500,8500&amp;layer=2&amp;src=AdobeStock_322150086%20trans) |
| ![红色背景](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20trans?&amp;layer=1&amp;color=200,50,50&amp;size=8500,8500&amp;layer=2&amp;src=AdobeStock_322150086%20trans){width="250"} | 红色背景 | [链接](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20trans?&amp;layer=1&amp;color=200,50,50&amp;size=8500,8500&amp;layer=2&amp;src=AdobeStock_322150086%20trans) |
| ![剪切到OVAL路径](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?clipPathE=round&amp;bgc=255,255,255){width="250"} | 剪切到椭圆路径 | [链接](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?clipPathE=round&amp;bgc=255,255,255) |


>[!TAB 图像路径示例]

以下是视频中使用的Dynamic Media图像服务器示例URL。

| 预览 | 描述 | Dynamic Media URL |
|-----------|------------------|---------|
| ![已规范化为80像素宽（无透明度）](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?wid=800){width="250"} | 已规范化为80像素宽（无透明度）{width="250"} | [链接](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?wid=800) |
| ![裁切到路径](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?cropPathE=Path%201&amp;wid=800){width="250"} | 裁切到路径 | [链接](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?cropPathE=Path%201&amp;wid=800) |
| ![剪辑到路径](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?clipPathE=Path%201&amp;wid=800){width="250"} | 剪切到路径 | [链接](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?clipPathE=Path%201&amp;wid=800) |
| ![剪切到路径和裁切到路径](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?clipPathE=Path%201&amp;cropPathE=Path%201&amp;wid=800){width="250"} | 剪切到路径和裁切到路径 | [链接](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?clipPathE=Path%201&amp;cropPathE=Path%201&amp;wid=800) |
| ![剪辑到其他路径](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?clipPathE=round&amp;wid=800){width="250"} | 剪辑到其他路径 | [链接](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?clipPathE=round&amp;wid=800) |
| ![剪辑到其他路径并制作红色背景](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086fullpaths?cropPathE=round&amp;clipPathE=round&amp;bgc=200,50,50&amp;wid=800){width="250"} | 剪切到另一条路径并制作红色背景 | [链接](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086fullpaths?cropPathE=round&amp;clipPathE=round&amp;bgc=200,50,50&amp;wid=800) |

>[!ENDTABS]


## Dynamic Media图像服务器API

* [Dynamic Media图像服务和渲染API](https://experienceleague.adobe.com/zh-hans/docs/dynamic-media-developer-resources/image-serving-api/image-serving-api/http-protocol-reference/c-http-protocol-reference)
* [Dynamic Media快照预览](https://snapshot.scene7.com/)
