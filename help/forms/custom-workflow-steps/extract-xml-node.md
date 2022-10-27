---
title: 从提交的数据xml中提取节点
description: 自定义进程步骤，将负载文件夹下的写文档添加到文件系统
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: kt-9860
exl-id: 5282034f-275a-479d-aacb-fc5387da793d
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '179'
ht-degree: 0%

---

# 从提交的数据xml中提取节点

此自定义流程步骤是通过从另一个xml文档中提取节点来创建新的xml文档。 当要将提交的数据与xdp模板合并以生成PDF时，您需要使用此模板。 例如，在提交自适应表单时，需要与xdp模板合并的数据就位于数据元素内。 在这种情况下，您需要通过提取相应的数据元素来创建另一个xml文档。

以下屏幕快照显示了您需要传递到自定义流程步骤的参数
![过程步骤](assets/create-xml-process-step.png)
以下是参数
* Data.xml — 要从中提取节点的xml文件
* datamerge.xml — 使用提取的节点创建的新xml
* /afData/afUnboundData/data — 要提取的节点


以下屏幕截图显示了在有效负荷文件夹下创建datamerge.xml的内容
![create-xml](assets/create-xml.png)

[可从此处下载自定义包](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)
