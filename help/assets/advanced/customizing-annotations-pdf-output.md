---
title: 在AEM Assets中自定义注释
description: AEM Assets格式和样式。
feature: Collaboration
version: 6.3, 6.4, 6.5
topic: Collaboration
role: Developer
level: Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '64'
ht-degree: 4%

---


# 在AEM Assets中自定义注释{#using-annotations-in-aem-assets}

AEM支持自定义注释输出为PDF。

## PDF批注sling:OsgiConfig定义

要自定义PDF批注，请在AEM项目下创建&#x200B;**sling:OsgiConfig**&#x200B;节点

`/apps/my-project/config.author/com.day.cq.dam.core.impl.annotation.pdf.AnnotationPdfConfig.xml` 并根据需要调整值：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
    jcr:primaryType="sling:OsgiConfig"

    cq.dam.config.annotation.pdf.document.padding.vertical="{Long}20"
    cq.dam.config.annotation.pdf.reviewStatus.color.changesRequested="fad269"
    cq.dam.config.annotation.pdf.document.height="{Long}792"
    cq.dam.config.annotation.pdf.document.width="{Long}612"
    cq.dam.config.annotation.pdf.reviewStatus.width="{Long}150"
    cq.dam.config.annotation.pdf.font.size="{Long}7"
    cq.dam.config.annotation.pdf.font.color="3B3B3B"
    cq.dam.config.annotation.pdf.font.light="A4A4A4"
    cq.dam.config.annotation.pdf.reviewStatus.color.rejected="fa7d73"
    cq.dam.config.annotation.pdf.minImageHeight="{Long}100"
    cq.dam.config.annotation.pdf.reviewStatus.color.approved="009933"
    cq.dam.config.annotation.pdf.marginTextImage="{Long}14"
    cq.dam.config.annotation.pdf.document.padding.horizontal="{Long}20"
    cq.dam.config.annotation.pdf.annotationMarker.width="{Long}5"
    />
```
