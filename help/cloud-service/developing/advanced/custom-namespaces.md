---
title: 自定义命名空间
description: 了解如何定义自定义命名空间并将其部署到AEM as a Cloud Service。
version: Experience Manager as a Cloud Service
topic: Development, Content Management
feature: Metadata
role: Developer
level: Intermediate
jira: KT-11618
thumbnail: 3412319.jpg
last-substantial-update: 2022-12-14T00:00:00Z
exl-id: e86ddc9d-ce44-407a-a20c-fb3297bb0eb2
duration: 496
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '203'
ht-degree: 2%

---

# 自定义命名空间

了解如何定义自定义[命名空间](https://developer.adobe.com/experience-manager/reference-materials/spec/jcr/1.0/4.5_Namespaces.html)并将其部署到AEM as a Cloud Service。

自定义命名空间是`:`之前的JCR属性的可选部分。 AEM使用多个命名空间，例如：

+ JCR系统属性为`jcr`
+ 用于AEM（以前称为Adobe CQ）属性的`cq`
+ 特定于DAM资源的AEM资产的`dam`
+ 都柏林核心属性的`dc`

...和其他很多人。

命名空间可用于表示属性的范围和用途。 创建自定义命名空间（通常是您的公司名称）有助于明确识别AEM实施特定的节点或资产，并包含特定于您的业务的数据。

自定义命名空间在[Sling存储库初始化(repoinit)](https://sling.apache.org/documentation/bundles/repository-initialization.html)脚本中进行管理，并作为OSGi配置部署到AEM as a Cloud Service — 并添加到您的[AEM项目的](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=zh-Hans) `ui.config`项目中。

>[!VIDEO](https://video.tv.adobe.com/v/3412319?quality=12&learn=on)

## 资源

+ [Sling存储库初始化(repoinit)文档](https://sling.apache.org/documentation/bundles/repository-initialization.html#repoinit-parser-test-scenarios)

## 代码

以下代码用于配置`wknd`命名空间。

### RepositoryInitializer OSGi配置

`/ui.config/src/main/content/jcr_root/apps/wknd-examples/osgiconfig/config/org.apache.sling.jcr.repoinit.RepositoryInitializer~wknd-examples-namespaces.cfg.json`

```json
{

    "scripts": [
        "register namespace (wknd) https://site.wknd/1.0"
    ]
}
```

这允许在AEM中使用使用`wknd`命名空间的自定义属性（表示为`register namespace`指令之后的第一个参数）。 有关更高级的脚本定义，请查看[Sling存储库初始化(repoinit)文档](https://sling.apache.org/documentation/bundles/repository-initialization.html#repoinit-parser-test-scenarios)中的示例。
