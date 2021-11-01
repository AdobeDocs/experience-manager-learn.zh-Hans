---
title: 使用AEM现代化工具迁移到AEMas a Cloud Service
description: 了解如何使用AEM现代化工具升级现有AEM项目和内容，使其as a Cloud Service兼容。
version: Cloud Service
topic: Migration, Upgrade
role: Developer
level: Experienced
kt: 8629
thumbnail: 336965.jpeg
exl-id: 310f492c-0095-4015-81a4-27d76f288138
source-git-commit: 3657e7798774f9cc673ff6ccd8af1a555b1d4013
workflow-type: tm+mt
source-wordcount: '325'
ht-degree: 1%

---


# AEM 现代化工具

了解如何使用AEM现代化工具升级现有的AEM Sites内容，使其as a Cloud Service兼容并符合最佳实践。

>[!VIDEO](https://video.tv.adobe.com/v/336965/?quality=12&learn=on)

## 使用AEM现代化工具

![AEM现代化工具生命周期](./assets/aem-modernization-tools.png)

AEM现代化工具会自动转换由旧版静态模板、基础组件和parsys组成的现有AEM页面，以使用可编辑模板、AEM核心WCM组件和布局容器等现代方法。

### 关键活动

+ 克隆AEM 6.x生产以针对
+ 下载并安装 [最新AEM现代化工具](https://github.com/adobe/aem-modernize-tools/releases/latest) 通过包管理器在AEM 6.x生产克隆上

+ [页面结构转换器](https://opensource.adobe.com/aem-modernize-tools/pages/tools/page-structure.html) 使用布局容器将静态模板中的现有页面内容更新为已映射的可编辑模板
   + 使用OSGi配置定义转化规则
   + 针对现有页面运行页面结构转换器

+ [组件转换器](https://opensource.adobe.com/aem-modernize-tools/pages/tools/component.html) 使用布局容器将静态模板中的现有页面内容更新为已映射的可编辑模板
   + 通过JCR节点定义/XML定义转化规则
   + 针对现有页面运行组件转换器工具

+ [策略导入器](https://opensource.adobe.com/aem-modernize-tools/pages/tools/policy-importer.html) 从设计配置创建策略
   + 使用JCR节点定义/XML定义转化规则
   + 根据现有设计定义运行策略导入器
   + 将导入的策略应用于AEM组件和容器

+ [对话框转换器](https://opensource.adobe.com/aem-modernize-tools/pages/tools/dialog.html) 将经典(ExtJS)和基于CoralUI 2的组件对话框转换为基于CoralUI 3触屏UI的对话框。
   + 针对现有的ExtJS或基于Coral2 UI的对话框运行对话框转换器工具
   + 将转换的对话框同步回Git存储库

### 其他资源

+ [下载AEM Modernizations工具](https://github.com/adobe/aem-modernize-tools/releases/latest)
+ [AEM现代化工具文档](https://opensource.adobe.com/aem-modernize-tools/)
+ [AEM Gems — 介绍AEM现代化套件](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/Introducing-the-AEM-Modernization-Suite.html)
