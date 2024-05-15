---
title: 在AEM中开发Sling模型导出程序
description: 此技术演练将逐步介绍如何设置AEM以与Sling模型导出器一起使用、如何使用导出器框架增强现有Sling模型以呈现为JSON，以及如何使用导出器选项和Jackson注释进一步自定义输出。
version: 6.4, 6.5
sub-product: Experience Manager, Experience Manager Sites
feature: APIs
doc-type: Technical Video
topic: Development
role: Developer
level: Intermediate
exl-id: fc321ed1-5cf7-4bbe-adc6-c4905af7b43c
duration: 932
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '363'
ht-degree: 0%

---

# 开发Sling模型导出程序

此技术演练将逐步介绍如何设置AEM以与Sling模型导出器一起使用、如何使用导出器框架增强现有Sling模型以呈现为JSON，以及如何使用导出器选项和Jackson注释进一步自定义输出。

Sling模型导出器在Sling模型v1.3.0中引入。这项新功能允许向Sling模型添加新注释，这些注释定义如何将模型导出为不同的Java对象，或者更常见的是，导出为不同的格式，如JSON。

Apache Sling提供了Jackson JSON导出器，以涵盖将Sling模型导出为JSON对象以供程序化Web使用者（如其他Web服务和JavaScript应用程序）使用的最常见案例。

## 为Sling模型导出器配置AEM

>[!VIDEO](https://video.tv.adobe.com/v/16862?quality=12&learn=on)

[!DNL Sling Model Exporter] 是 [!DNL Apache Sling] 而不是直接绑定到AEM产品发行周期。 [!DNL Sling Model Exporter] 与AEM 6.3及更高版本兼容。

## 的用例 [!DNL Sling Model Exporter]

>[!VIDEO](https://video.tv.adobe.com/v/16863?quality=12&learn=on)

[!DNL Sling Model Exporter] 非常适合利用已包含业务逻辑的Sling模型，这些业务逻辑通过HTL（或以前的JSP）支持HTML呈现，并展示与JSON相同的业务表示以供程序化Web服务或JavaScript应用程序使用。

## 创建Sling模型导出程序

>[!VIDEO](https://video.tv.adobe.com/v/16864?quality=12&learn=on)

正在启用 [!DNL Exporter] 支持 [!DNL Sling Model] 与添加 `@Exporter` Java类的注释。

## 应用Sling模型导出器选项

>[!VIDEO](https://video.tv.adobe.com/v/16865?quality=12&learn=on)

[!DNL Sling Model Exporter] 支持将每个模型的导出器选项传递给导出器实施，以驱动 [!DNL Sling Model] 最终导出。 这些选项通常“全局”应用于 [!DNL Sling Model] 导出，而不是按数据点导出，后者可通过下面所述的内联注释完成。

[!DNL Jackson Exporter] 选项包括：

* [映射器功能选项](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/MapperFeature.html)
* [序列化功能选项](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/SerializationFeature.html)

## 正在应用 [!DNL Jackson] 注释

>[!VIDEO](https://video.tv.adobe.com/v/16866?quality=12&learn=on)

导出程序实施还支持可在页面上内联应用的注释。 [!DNL Sling Model] 类，可以提供更细的控制数据导出方式。

* [[!DNL Jackson Exporter] 注释](https://github.com/FasterXML/jackson-annotations/wiki/Jackson-Annotations)

## 查看代码 {#view-the-code}

[SampleSlingModelExporter.java](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/core/src/main/java/com/adobe/acs/samples/models/SampleSlingModelExporter.java)

## 支持材料 {#supporting-materials}

* [[!DNL Jackson Mapper] 功能Javadoc](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/MapperFeature.html)
* [[!DNL Jackson Serialization] 功能Javadoc](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/SerializationFeature.html)

* [[!DNL Jackson Annotations] 文档](https://github.com/FasterXML/jackson-annotations/wiki/Jackson-Annotations)
