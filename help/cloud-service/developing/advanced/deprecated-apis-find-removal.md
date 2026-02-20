---
title: 在AEM as a Cloud Service中查找并删除已弃用的API
description: 了解如何在AEM as a Cloud Service中查找和删除已弃用的API。
version: Experience Manager as a Cloud Service
role: Developer, Architect
level: Beginner
doc-type: tutorial
duration: null
jira: KT-20288
thumbnail: KT-20288.png
last-substantial-update: 2026-02-09T00:00:00Z
exl-id: 287894ea-9cc1-4c27-ac7e-967ad46f4789
source-git-commit: 08dd5006ebf4ebd94e8bc60594f8f1541feb810f
workflow-type: tm+mt
source-wordcount: '603'
ht-degree: 2%

---

# 在AEM as a Cloud Service中查找并删除已弃用的API

了解如何在AEM as a Cloud Service中查找和删除已弃用的API。

## 概述

要确保应用程序的安全性和性能，并且您可以继续使用Cloud Manager管道部署代码，请从项目中移除已弃用的API。

在本教程中，您将了解如何使用[AEM Analyzer Maven插件](https://github.com/adobe/aemanalyser-maven-plugin/blob/main/aemanalyser-maven-plugin/README.md)在AEM as a Cloud Service环境中查找和删除已弃用的API。

## 关于已弃用API的通知

已弃用的API的使用情况和修复此问题的注意事项会定期报告，让我们查看一些示例。

- AEM as a Cloud Service **操作中心**&#x200B;通知您项目中的&#x200B;_已弃用的API_。
  操作中心![已弃用的API](./assets/deprecated-apis/actions-center-deprecated-apis.png)

- Cloud Manager管道报告中的&#x200B;**代码扫描**&#x200B;步骤在您的项目中弃用的API，请查看&#x200B;**下载详细信息**报告，以查看弃用的API的完整列表。
  ![代码扫描中已弃用的API](./assets/deprecated-apis/code-scanning-summary.png)

- Cloud Manager管道中的&#x200B;**工件准备**&#x200B;步骤报告项目中已弃用的API，**下载日志**&#x200B;并在日志文件中查找&#x200B;_Analyzer警告_。

  ```
  2026-02-20 15:40:48.376 Analyser warnings have been found 
  2026-02-20 15:40:48.376 The analyser found the following warnings for author and publish : 
  2026-02-20 15:40:48.376 [region-deprecated-api] com.adobe.aem.guides:aem-guides-wknd.core:4.0.5-SNAPSHOT: Usage of deprecated package found : org.apache.commons.lang : Commons Lang 2 is in maintenance mode. Commons Lang 3 should be used instead. Deprecated since 2021-04-30 For removal : 2021-12-31 (com.adobe.aem.guides:aem-guides-wknd.all:4.0.5-SNAPSHOT)
  2026-02-20 15:40:56.458 Convert Merge Analyse finished.
  ```


## 如何查找已弃用的API

按照以下步骤在AEM as a Cloud Service项目中查找已弃用的API。

1. **使用最新的AEM Analyzer Maven插件**

   在您的AEM项目中，使用最新版本的[AEM分析器Maven插件](https://github.com/adobe/aemanalyser-maven-plugin/blob/main/aemanalyser-maven-plugin/README.md)。

   - 在主`pom.xml`中，通常会声明插件版本。 将您的版本与最新的[发布版本](https://mvnrepository.com/artifact/com.adobe.aem/aemanalyser-maven-plugin)进行比较。

     ```xml
     ...
     <aemanalyser.version>1.6.16</aemanalyser.version> <!-- Latest released version as of 20-Feb-2026 -->
     ...
     <!-- AEM Analyser Plugin -->
     <plugin>
         <groupId>com.adobe.aem</groupId>
         <artifactId>aemanalyser-maven-plugin</artifactId>
         <version>${aemanalyser.version}</version>
         <extensions>true</extensions>
     </plugin>
     ...
     ```

   - 该插件会根据最新可用的AEM SDK进行测试。 在您的项目的`pom.xml`文件中使用最新的AEM SDK版本。 它有助于将已弃用的API显示为IDE警告。

     ```xml
     ...
     <aem.sdk.api>2026.2.24464.20260214T050318Z-260100</aem.sdk.api> <!-- Latest available AEM SDK version as of 20-Feb-2026 -->
     ...
     ```

   - 确保`all`模块在`verify`阶段中运行插件。

     ```xml
     ...
     <build>
         <plugins>
             ...
             <plugin>
                 <groupId>com.adobe.aem</groupId>
                 <artifactId>aemanalyser-maven-plugin</artifactId>
                 <extensions>true</extensions>
                 <executions>
                     <execution>
                         <id>analyse-project</id>
                         <phase>verify</phase>
                         <goals>
                             <goal>project-analyse</goal>
                         </goals>
                     </execution>
                 </executions>
             </plugin>
             ...
         </plugins>
     </build>
     ...
     ```

2. **运行生成并检查警告**

   运行`mvn clean install`时，分析器将已弃用的API报告为输出中的&#x200B;**[WARNING]**&#x200B;消息。 例如：

   ```shell
   ...
   [WARNING] The analyser found the following warnings for author and publish :
   [WARNING] [region-deprecated-api] com.adobe.aem.guides:aem-guides-wknd.core:4.0.5-SNAPSHOT: Usage of deprecated package found : org.apache.commons.lang : Commons Lang 2 is in maintenance mode. Commons Lang 3 should be used instead. Deprecated since 2021-04-30 For removal : 2021-12-31 (com.adobe.aem.guides:aem-guides-wknd.all:4.0.5-SNAPSHOT)
   ...
   ```

   当专注于构建成功或失败时，很容易忽略这些消息。

3. **获取已弃用的API的清晰列表**

   上述步骤也提供了相同的信息。 但是，对`verify`模块运行`all`阶段可在一个位置查看所有&#x200B;**[WARNING]**&#x200B;消息。 例如：

   ```shell
   $ mvn clean verify -pl all
   ```

   生成输出中的&#x200B;**[WARNING]**&#x200B;消息列出了项目中已弃用的API。

## 如何删除已弃用的API

AEM分析器报告&#x200B;**什么**&#x200B;已被弃用，并提供有关如何修复该问题的&#x200B;**建议**。 但是，请使用下表选择正确的操作，并在需要更多详细信息时按照链接的文档操作。

### 已弃用的API修正策略

| Analyzer警告类型 | 它指示的内容 | 建议的操作 | 引用 |
| --------------------- | ----------------- | ------------------ | --------- |
| 已弃用的AEM API | 将从AEM as a Cloud Service删除API | 将用法替换为支持的公共API | [API删除指南](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/release-notes/deprecated-removed-features#api-removal-guidance) |
| 已弃用的AEM包或类 | 不再支持包或类 | 重构代码以使用推荐的替代方案 | [已弃用的API](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/release-notes/deprecated-removed-features#aem-apis) |
| 已弃用的第三方库 | 将来的SDK中将不支持库 | 升级依赖项并重构使用情况 | [一般准则](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/release-notes/deprecated-removed-features#api-removal-guidance) |
| 已弃用的Sling/OSGi模式 | 检测到旧批注或API | 迁移到新版Sling和OSGi API | [删除Sling/OSGi模式](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/release-notes/deprecated-removed-features#api-removal-guidance) |
| 计划删除（未来日期） | API仍然有效，但稍后会强制删除 | 在管道实施之前计划清理 | [发行说明](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-cloud-service/content/release-notes/home) |

### 实践指导

- 将Analyzer警告视为&#x200B;**未来管道故障**，而不是可选消息。
- 使用&#x200B;**最新的AEM SDK**&#x200B;在本地修复已弃用的API。
- 保持Analyzer输出清洁，以避免在未来AEM升级期间出现问题。

尽早修复已弃用的API可使您的项目&#x200B;**安全升级且部署就绪**。

## 其他资源

- [AEM Analyzer Maven插件](https://github.com/adobe/aemanalyser-maven-plugin/blob/main/aemanalyser-maven-plugin/README.md)
- [已弃用和已删除的功能和API](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/release-notes/deprecated-removed-features#api-removal-guidance)
