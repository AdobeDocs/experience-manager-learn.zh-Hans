---
title: 在AEM中开发Sling模型导出程序
description: 本技术演示了如何设置AEM以与Sling模型导出器一起使用，使用导出器框架增强现有的Sling模型以呈现为JSON，以及如何使用导出器选项和Jackson批注进一步自定义输出。
version: 6.3, 6.4, 6.5
sub-product: 基础，内容服务
feature: API
topics: content-delivery, development, headless
activity: develop
audience: developer
doc-type: technical video
topic: 开发
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '423'
ht-degree: 0%

---


# 开发Sling模型导出程序

本技术演示了如何设置AEM以与Sling模型导出器一起使用，使用导出器框架增强现有的Sling模型以呈现为JSON，以及如何使用导出器选项和Jackson批注进一步自定义输出。

Sling模型导出程序在Sling模型v1.3.0中引入。这项新功能允许向Sling模型添加新注释，以定义如何将模型导出为不同的Java对象，或者更常用的方式，序列化为不同的格式，如JSON。

Apache Sling提供了Jackson JSON导出程序，以涵盖将Sling模型导出为JSON对象以供其他Web服务和JavaScript应用程序等编程Web用户使用的最常见情况。

## 为Sling模型导出程序配置AEM

>[!VIDEO](https://video.tv.adobe.com/v/16862/?quality=12&learn=on)

[!DNL Sling Model Exporter] 是项目的一项 [!DNL Apache Sling] 功能，不会直接绑定到AEM产品发布周期。[!DNL Sling Model Exporter] 与AEM 6.3及更高版本兼容。

## [!DNL Sling Model Exporter]的用例

>[!VIDEO](https://video.tv.adobe.com/v/16863/?quality=12&learn=on)

[!DNL Sling Model Exporter] 非常适合利用已包含业务逻辑的Sling模型，该业务逻辑通过HTL（或以前的JSP）支持HTML呈现，并显示与JSON相同的业务表示形式，以供编程Web服务或JavaScript应用程序使用。

## 创建Sling模型导出器

>[!VIDEO](https://video.tv.adobe.com/v/16864/?quality=12&learn=on)

在[!DNL Sling Model]上启用[!DNL Exporter]支持与将`@Exporter`注释添加到Java类一样简单。

## 应用Sling模型导出器选项

>[!VIDEO](https://video.tv.adobe.com/v/16865/?quality=12&learn=on)

[!DNL Sling Model Exporter] 支持将按模型导出器选项传递到导出器实施，以驱动最终 [!DNL Sling Model] 导出的方式。与通过下面描述的内联注释来完成的数据点相比，这些选项通常会将“全局”应用于[!DNL Sling Model]的导出方式。

[!DNL Jackson Exporter] 选项包括：

* [映射器功能选项](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/MapperFeature.html)
* [序列化功能选项](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/SerializationFeature.html)

## 应用[!DNL Jackson]注释

>[!VIDEO](https://video.tv.adobe.com/v/16866/?quality=12&learn=on)

导出程序实施还支持可在[!DNL Sling Model]类上内联应用的注释，这些注释可提供更精细的数据导出控制。

* [[!DNL Jackson Exporter] 批注](https://github.com/FasterXML/jackson-annotations/wiki/Jackson-Annotations)

## 查看代码{#view-the-code}

[SampleSlingModelExporter.java](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/core/src/main/java/com/adobe/acs/samples/models/SampleSlingModelExporter.java)

## 辅助材料{#supporting-materials}

* [[!DNL Jackson Mapper] 功能Javadoc](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/MapperFeature.html)
* [[!DNL Jackson Serialization] 功能Javadoc](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/SerializationFeature.html)

* [[!DNL Jackson Annotations] 文档](https://github.com/FasterXML/jackson-annotations/wiki/Jackson-Annotations)
