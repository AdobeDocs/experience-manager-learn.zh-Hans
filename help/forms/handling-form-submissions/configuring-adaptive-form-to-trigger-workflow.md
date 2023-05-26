---
title: 配置自适应表单以触发AEM Workflow概述
description: 在表单提交时触发AEM工作流时配置有效负载选项
feature: Workflow
topics: integrations
audience: developer
doc-type: article
activity: setup
version: 6.4,6.5
kt: 5407
thumbnail: 40258.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 9f1dbd02-774a-4b84-90fa-02d4e468cbac
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '165'
ht-degree: 3%

---

# 配置自适应表单以触发AEM Workflow

## 前提条件

此工作流中使用的示例表单基于自定义自适应表单模板，需要将其导入您的AEM服务器。 在导入模板后，需要导入提供的示例表单。

### 获取自适应表单模板

* 下载 [自适应表单模板](assets/af-form-template.zip)
* [使用包管理器导入模板](http://localhost:4502/crx/packmgr/index.jsp)
* 上传并安装自适应表单模板

### 获取自适应表单示例

* 下载 [自适应表单](assets/peak-application-form.zip)
* 浏览到 [表单和文档](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* 单击创建 — >文件上传
* 自适应表单示例放置在名为的文件夹中 [应用程序Forms](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments/applicationforms)

以下视频介绍如何配置自适应表单以触发AEM Workflow
>[!VIDEO](https://video.tv.adobe.com/v/40258?quality=12&learn=on)

以下视频说明了crx存储库中的工作流有效负载和其他详细信息

>[!VIDEO](https://video.tv.adobe.com/v/40259?quality=12&learn=on)
