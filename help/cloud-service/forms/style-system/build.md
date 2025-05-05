---
title: 在AEM Forms中使用样式系统
description: 构建主题项目
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
topic: Development
feature: Adaptive Forms
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
jira: KT-16276
exl-id: 4a02f494-ca0e-42d4-bbb9-6223ff8685e3
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '253'
ht-degree: 0%

---

# 测试更改

基于&#x200B;**&quot;Blank with Core Components&quot;**&#x200B;模板创建自适应表单。 将3个按钮拖放到表单上，并将其标记为“公司”、“营销”和“默认”。
通过选择如下所示的绘画画笔，为公司和营销按钮分配相应的样式变体。

![样式](assets/marketing-variation.png)

第三个按钮将应用默认样式。

## 构建主题项目

下一步是构建主题项目。 导航到主题项目的根文件夹，然后运行命令&#x200B;_&#x200B;**npm run build**&#x200B;_，如下面的屏幕快照所示。

![生成主题](assets/build-theme.png)

成功构建主题项目后，您便可以测试更改。

## 测试css的快速轻松方法

* 打开主题项目的dist文件夹下的theme.css文件。选择并复制整个文件内容。
* 预览在上一步中创建的表单。
* 右键单击其中一个按钮并选择“检查”以打开开发人员控制台。
* 在开发人员控制台中，单击theme.css以打开theme.css
* 使用CTR-A选择并删除theme.css的整个内容，然后点击删除按钮。
* 复制并粘贴您在上一步中构建的theme.css的内容。
* 这些按钮应会更新为如下所示的相应样式。

![最终按钮](assets/final-state-buttons.png)

## 推送更改

如果您对更改感到满意，可以使用[前端管道](https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/enable-frontend-pipeline-devops/create-frontend-pipeline)将更改推送到您的云实例
