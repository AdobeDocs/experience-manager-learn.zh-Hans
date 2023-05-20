---
title: 自訂名稱空間
description: 瞭解如何定義自訂名稱空間並將其部署到AEMas a Cloud Service。
version: Cloud Service
topic: Development, Content Management
feature: Metadata
role: Developer
level: Intermediate
kt: 11618
thumbnail: 3412319.jpg
last-substantial-update: 2022-12-14T00:00:00Z
exl-id: e86ddc9d-ce44-407a-a20c-fb3297bb0eb2
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '230'
ht-degree: 6%

---

# 自訂名稱空間

瞭解如何定義和部署自訂 [名稱空間](https://developer.adobe.com/experience-manager/reference-materials/spec/jcr/1.0/4.5_Namespaces.html) 至AEMas a Cloud Service。

自訂名稱空間是JCR屬性在之前的選用部分。 `:`. AEM使用幾個名稱空間，例如：

+ `jcr` JCR系統屬性
+ `cq` for AEM (先前稱為Adobe CQ)屬性
+ `dam` DAM資產專用的AEM屬性
+ `dc` 都柏林核心屬性

...和其他許多專案。

名稱空間可用來表示屬性的範圍和意圖。 建立自訂名稱空間（通常是您的公司名稱）有助於清楚識別AEM實作特有的節點或屬性，並包含您的企業專屬的資料。

自訂名稱空間的管理位置 [Sling存放庫初始化(repoinit)](https://sling.apache.org/documentation/bundles/repository-initialization.html) 指令碼，並部署至AEMas a Cloud Service做為OSGi設定 — 並新增至 [AEM專案的](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html) `ui.config` 專案。

>[!VIDEO](https://video.tv.adobe.com/v/3412319?quality=12&learn=on)

## 资源

+ [Sling存放庫初始化(repoinit)檔案](https://sling.apache.org/documentation/bundles/repository-initialization.html#repoinit-parser-test-scenarios)

## 程式碼

下列程式碼可用來設定 `wknd` 名稱空間。

### RepositoryInitializer OSGi設定

`/ui.config/src/main/content/jcr_root/apps/wknd-examples/osgiconfig/config/org.apache.sling.jcr.repoinit.RepositoryInitializer~wknd-examples-namespaces.cfg.json`

```json
{

    "scripts": [
        "register namespace (wknd) https://site.wknd/1.0"
    ]
}
```

這可讓自訂屬性使用 `wknd` 名稱空間，表示為 `register namespace` 指示，用於AEM。 如需更進階的指令碼定義，請檢閱 [Sling存放庫初始化(repoinit)檔案](https://sling.apache.org/documentation/bundles/repository-initialization.html#repoinit-parser-test-scenarios).
