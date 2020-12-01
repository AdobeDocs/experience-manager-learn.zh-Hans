---
title: 将智能裁剪与AEM Assets动态媒体结合使用
seo-title: 将智能裁剪与AEM Assets动态媒体结合使用
description: Smart Crop使用Adobe Sensei来消除耗时且成本高昂的裁剪内容任务，以实现响应式设计。
seo-description: Smart Crop使用Adobe Sensei来消除耗时且成本高昂的裁剪内容任务，以实现响应式设计。
uuid: 2cb27aa8-644d-4b17-8ffc-f6a99f95cfd2
discoiquuid: e4b8534c-fa64-491f-86ec-4dbe50cd6bf7
sub-product: 动态媒体
feature: smart-crop, image-profiles
topics: images, renditions, authoring
doc-type: feature video
audience: all
activity: use
version: 6.4, 6.5
translation-type: tm+mt
source-git-commit: 9cf01dbf9461df4cc96d5bd0a96c0d4d900af089
workflow-type: tm+mt
source-wordcount: '273'
ht-degree: 0%

---


# 将智能裁剪与AEM AssetsDynamic Media一起使用{#using-smart-crop-with-aem-assets-dynamic-media}

Smart Crop使用Adobe Sensei来消除耗时且成本高昂的裁剪内容任务，以实现响应式设计。

>[!VIDEO](https://video.tv.adobe.com/v/21519/)

>[!NOTE]
>
>视频假定AEM实例在Dynamic Media S7模式下运行。 [有关使用Dynamic Media设置AEM的说明，请参阅此处。](https://helpx.adobe.com/experience-manager/6-3/assets/using/config-dynamic-fp-14410.html)

## Adobe Experience Manager的动态媒体智能裁剪功能包括

* AEM资产管理员可根据设备宽度和高度轻松创建智能裁剪的图像用户档案。
* 您可以对单个资产执行智能裁剪，也可以对文件夹中的所有资产执行智能裁剪。
* 可以调整智能裁剪编辑布局的大小以增强可见性。
* AEM Sites的Dynamic Media组件支持智能裁剪。
* 智能裁剪资产的已发布URL可用于接受托管资产的第三方应用程序。

>[!NOTE]
>
>智能裁剪坐标取决于长宽比。 即，对于图像用户档案中的各种智能裁剪设置，如果长宽比对于图像用户档案中添加的尺寸是相同的，则会将相同的长宽比发送到Dynamic Media。 因此，智能裁剪编辑器中将建议使用相同的裁剪区域。 例如，如果裁剪设置为100x100和200x200，则系统将生成相同的智能裁剪。
