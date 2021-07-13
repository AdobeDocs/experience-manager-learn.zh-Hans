---
title: 在AEM Assets Dynamic Media中使用智能裁剪
description: 智能裁剪使用Adobe Sensei来消除裁剪内容以进行响应式设计所耗时且成本高昂的任务。
sub-product: dynamic-media
feature: 智能裁剪、图像配置文件
version: 6.4, 6.5
topic: 内容管理
role: User
level: Beginner
source-git-commit: b0bca57676813bd353213b4808f99c463272de85
workflow-type: tm+mt
source-wordcount: '253'
ht-degree: 3%

---


# 在AEM Assets Dynamic Media中使用智能裁剪{#using-smart-crop-with-aem-assets-dynamic-media}

智能裁剪使用Adobe Sensei来消除裁剪内容以进行响应式设计所耗时且成本高昂的任务。

>[!VIDEO](https://video.tv.adobe.com/v/21519/)

>[!NOTE]
>
>视频假定您的AEM实例在Dynamic Media S7模式下运行。 [有关在Dynamic Media中设置AEM的说明，请参阅此处。](https://helpx.adobe.com/cn/experience-manager/6-3/assets/using/config-dynamic-fp-14410.html)

## Adobe Experience Manager的Dynamic Media智能裁剪功能包括

* AEM资产管理员可以根据设备宽度和高度轻松创建用于智能裁剪的图像配置文件。
* 可以对单个资产执行智能裁剪，也可以对文件夹中的所有资产执行智能裁剪。
* 可以调整智能裁剪编辑布局的大小，以提高可见性。
* AEM Sites的Dynamic Media组件支持智能裁剪。
* 智能裁剪资产的已发布URL可用于接受托管资产的第三方应用程序。

>[!NOTE]
>
>智能裁切坐标取决于宽高比。 也就是说，对于图像配置文件中的各种智能裁剪设置，如果图像配置文件中添加的维度的宽高比相同，则会将相同的宽高比发送到Dynamic Media。 因此，智能裁剪编辑器中将建议使用相同的裁剪区域。 例如，如果裁剪设置为100x100和200x200，则系统将生成相同的智能裁剪。
