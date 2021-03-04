---
title: 将Smart Crop与AEM Assets Dynamic Media结合使用
description: Smart Crop使用Adobe Sensei消除了为响应式设计而裁切内容的耗时且成本高昂的任务。
sub-product: dynamic-media
feature: 智能裁剪，图像用户档案
version: 6.4, 6.5
topic: 内容管理
role: 业务从业者
level: 初学者
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '256'
ht-degree: 3%

---


# 将Smart Crop与AEM Assets Dynamic Media一起使用{#using-smart-crop-with-aem-assets-dynamic-media}

Smart Crop使用Adobe Sensei消除了为响应式设计而裁切内容的耗时且成本高昂的任务。

>[!VIDEO](https://video.tv.adobe.com/v/21519/)

>[!NOTE]
>
>视频假定您的AEM实例在Dynamic Media S7模式下运行。 [有关使用Dynamic Media设置AEM的说明，请参阅此处。](https://helpx.adobe.com/cn/experience-manager/6-3/assets/using/config-dynamic-fp-14410.html)

## Adobe Experience Manager的Dynamic Media Smart Crop功能包括

* AEM资产管理员可以根据设备宽度和高度轻松创建智能裁剪的图像用户档案。
* 您可以对单个资产执行智能裁剪，也可以对文件夹中的所有资产执行智能裁剪。
* 可以调整智能裁剪编辑布局的大小，以提高可见性。
* AEM Sites的Dynamic Media组件支持智能裁剪。
* 智能裁剪资产的已发布URL可用于接受托管资产的第三方应用程序。

>[!NOTE]
>
>智能裁剪坐标取决于长宽比。 即，对于图像用户档案中的各种智能裁剪设置，如果对于图像用户档案中添加的尺寸，宽高比相同，则会将相同的宽高比发送到Dynamic Media。 因此，在智能裁剪编辑器中将建议使用相同的裁剪区域。 例如，如果裁剪设置为100x100和200x200，则系统将生成相同的智能裁剪。
