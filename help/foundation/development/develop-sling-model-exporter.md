---
title: 在AEM开发Sling模型出口商
description: 本技术演练了如何设置AEM以与Sling Model Exporter一起使用、使用Exporter框架增强现有的Sling Model以再现为JSON，以及如何使用Exporter选项和Jackson批注进一步自定义输出。
version: 6.3, 6.4, 6.5
sub-product: 基础，内容服务
feature: sling-models, sling-model-exporter
topics: content-delivery, development, headless
activity: develop
audience: developer
doc-type: technical video
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '421'
ht-degree: 0%

---


# 开发Sling模型出口商

本技术演练了如何设置AEM以与Sling Model Exporter一起使用、使用Exporter框架增强现有的Sling Model以再现为JSON，以及如何使用Exporter选项和Jackson批注进一步自定义输出。

Sling Model Exporter是在Sling Models v1.3.0中引入的。此新功能允许向Sling Models添加新注释，这些注释定义了如何将模型作为不同的Java对象导出，或更常用地序列化为不同格式（如JSON）。

Apache Sling提供一个Jackson JSON导出器，用于涵盖将Sling Models导出为JSON对象以供其他Web服务和JavaScript应用程序等程序化Web用户使用的最常见情况。

## 为Sling Model Exporter配置AEM

>[!VIDEO](https://video.tv.adobe.com/v/16862/?quality=12&learn=on)

[!DNL Sling Model Exporter] 是项目的一个功 [!DNL Apache Sling] 能，不直接绑定到AEM产品发布周期。 [!DNL Sling Model Exporter] 与AEM 6.3及更高版本兼容。

## 用例 [!DNL Sling Model Exporter]

>[!VIDEO](https://video.tv.adobe.com/v/16863/?quality=12&learn=on)

[!DNL Sling Model Exporter] 非常适合利用已包含通过HTL（或以前称为JSP）支持HTML再现的业务逻辑的Sling Models，并通过程序化Web服务或JavaScript应用程序提供与JSON相同的业务表示形式以供使用。

## 创建Sling模型导出器

>[!VIDEO](https://video.tv.adobe.com/v/16864/?quality=12&learn=on)

在 [!DNL Exporter] 上启用 [!DNL Sling Model] 支持与向Java类添 `@Exporter` 加注释一样简单。

## 应用Sling Model Exporter选项

>[!VIDEO](https://video.tv.adobe.com/v/16865/?quality=12&learn=on)

[!DNL Sling Model Exporter] 支持将每型号导出器选项传递到导出器实施，以驱动最终 [!DNL Sling Model] 导出的方式。 这些选项通常“全局”应用于导出方 [!DNL Sling Model] 式，而不是通过下面描述的内嵌注释完成的数据点。

[!DNL Jackson Exporter] 选项包括：

* [映射器功能选项](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/MapperFeature.html)
* [序列化功能选项](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/SerializationFeature.html)

## 应用注 [!DNL Jackson] 释

>[!VIDEO](https://video.tv.adobe.com/v/16866/?quality=12&learn=on)

导出程序实现还支持可内嵌在类上应用的 [!DNL Sling Model] 注释，这些注释可以提供更精细的数据导出控制级别。

* [[!DNL Jackson Exporter] 批注](https://github.com/FasterXML/jackson-annotations/wiki/Jackson-Annotations)

## 视图代码 {#view-the-code}

[SampleSlingModelExporter.java](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/core/src/main/java/com/adobe/acs/samples/models/SampleSlingModelExporter.java)

## 支持材料 {#supporting-materials}

* [[!DNL Jackson Mapper] 功能Javadoc](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/MapperFeature.html)
* [[!DNL Jackson Serialization] 功能Javadoc](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/SerializationFeature.html)

* [[!DNL Jackson Annotations] 文档](https://github.com/FasterXML/jackson-annotations/wiki/Jackson-Annotations)
