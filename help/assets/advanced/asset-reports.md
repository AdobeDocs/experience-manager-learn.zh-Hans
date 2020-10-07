---
title: AEM Assets资产报道
description: 'AEM Assets提供了企业级报告框架，可通过直观的用户体验扩展到大型存储库。 '
feature: reports
topics: authoring, operations, performance, metadata
audience: all
doc-type: feature video
activity: use
version: 6.3, 6.4, 6.5
kt: 648
translation-type: tm+mt
source-git-commit: 10784dce34443adfa1fc6dc324242b1c021d2a17
workflow-type: tm+mt
source-wordcount: '91'
ht-degree: 2%

---


# 资产报表{#using-reports-in-aem-assets}

AEM Assets提供了企业级报告框架，可通过直观的用户体验扩展到大型存储库。

>[!VIDEO](https://video.tv.adobe.com/v/22140/?quality=12&learn=on)

## Microsoft Excel公式 {#excel-formulas}

视频中使用以下公式在Microsoft Excel中生成按大小排列的资产图表。

### 资产大小标准化为字节 {#asset-size-normalization-to-bytes}

```
=IF(RIGHT(D2,2)="KB",
      LEFT(D2,(LEN(D2)-2))*1024,
  IF(RIGHT(D2,2)="MB",
      LEFT(D2,(LEN(D2)-2))*1024*1024,
  IF(RIGHT(D2,2)="GB",
      LEFT(D2,(LEN(D2)-2))*1024*1024*1024,
  IF(RIGHT(D2,2)="TB",
      LEFT(D2,(LEN(D2)-2))*1024*1024*1024*1024, 0))))
```

### 按大小划分的资产计数 {#asset-count-by-size}

#### 小于200 KB {#less-than-kb}

```
=COUNTIFS(E2:E1000,"< 200000")
```

#### 200 KB至500 KB {#kb-to-kb}

```
=COUNTIFS(E2:E1000,">= 200000", E2:E1000,"<= 500000")
```

#### 大于500 KB {#greater-than-kb}

```
=COUNTIFS(E2:E1000,"> 500000")
```

## 其他资源{#additional-resources}

下载 [包含图表的所有资产Excel文件](./assets/asset-reports/all-assets.xlsx)
