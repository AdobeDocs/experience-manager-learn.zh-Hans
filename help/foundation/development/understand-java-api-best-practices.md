---
title: 了解AEM中的Java API最佳实践
description: AEM构建于一个丰富的开放源码软件堆栈上，它公开了许多Java API，以供在开发过程中使用。 本文探讨了主要API，以及它们的使用时间和用途。
version: 6.2, 6.3, 6.4, 6.5
sub-product: 基础，资产，站点
feature: APIs
topics: best-practices, development
activity: develop
audience: developer
doc-type: article
topic: Development
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '2027'
ht-degree: 2%

---


# 了解Java API最佳实践

Adobe Experience Manager(AEM)构建于一个丰富的开放源码软件堆栈上，它公开了许多在开发过程中使用的Java API。 本文探讨了主要API，以及它们的使用时间和用途。

AEM基于4个主要Java API集构建。

* **Adobe Experience Manager (AEM)**

   * 产品抽象，如页面、资产、工作流等

* **[!DNL Apache Sling]Web Framework**

   * REST和基于资源的抽象，如资源、值映射和HTTP请求。

* **JCR([!DNL Apache Jackrabbit Oak])**

   * 数据和内容抽象，如节点、属性和会话。

* **[!DNL OSGi (Apache Felix)]**

   * OSGi应用程序容器抽象，如服务和(OSGi)组件。

## Java API首选项“经验法则”

一般规则是首选API/抽象以下顺序：

1. **AEM**
1. **[!DNL Sling]**
1. **JCR**
1. **OSGi**

如果AEM提供API，则首选它而非[!DNL Sling]、JCR和OSGi。 如果AEM不提供API，则优先[!DNL Sling]而非JCR和OSGi。

此顺序是一般规则，表示存在例外。 中断此规则的可接受理由有：

* 众所周知的例外，如下所述。
* 所需功能在更高级别的API中不可用。
* 在现有代码(自定义或AEM产品代码)的上下文中操作，该代码本身使用较不首选的API，而移动到新API的成本是不合理的。

   * 与创建混合，使用较低级别的API更好。

## AEM API

* [**AEM API JavaDocs**](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/index.html)

AEM API提供特定于按产品分类的使用案例的抽象和功能。

例如，AEM [PageManager](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/reference-materials/javadoc/com/day/cq/wcm/api/PageManager.html)和[Page](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/day/cq/wcm/foundation/model/Page.html) API为AEM中表示网页的`cq:Page`节点提供抽象。

虽然这些节点可通过[!DNL Sling] API作为资源使用，而JCR API作为节点使用，但AEM API为常见用例提供抽象。 使用AEM API可确保产品和AEM的自定义和扩展之间的一致行为。

### com.adobe.*与com.day。* API

AEM API具有由以下Java包标识的包内首选项（按首选项顺序排列）：

1. `com.adobe.cq`
1. `com.adobe.granite`
1. `com.day.cq`

`com.adobe.cq` 支持产品用例， `com.adobe.granite` 而支持跨产品平台用例，如工作流或任务(在产品之间使用：AEM Assets、站点等)。

`com.day.cq` 包含“原始”API。这些API解决了在Adobe获取[!DNL Day CQ]之前和/或之前存在的核心抽象和功能。 除非`com.adobe.cq`或`com.adobe.granite`提供（较新的）备选项，否则这些API是受支持的，不应避免。

[!DNL Content Fragments]和[!DNL Experience Fragments]等新抽象是在`com.adobe.cq`空间中构建的，而不是下面描述的`com.day.cq`。

### 查询API

AEM支持多种查询语言。 3种主要语言为[JCR-SQL2](https://docs.jboss.org/jbossdna/0.7/manuals/reference/html/jcr-query-and-search.html)、XPath和[AEM 查询 Builder](https://helpx.adobe.com/cn/experience-manager/6-5/sites/developing/using/querybuilder-api.html)。

最重要的问题是在整个代码库中保持一致的查询语言，以降低理解的复杂性和成本。

所有查询语言都有效的性能用户档案，因为[!DNL Apache Oak]将它们传输到JCR-SQL2以进行最终查询执行，而与查询时间本身相比，转换到JCR-SQL2的时间可忽略不计。

首选的API是[AEM 查询 Builder](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/querybuilder-api.html)，它是最高级抽象，为查询构建、执行和检索结果提供了强大的API，并提供以下内容：

* 简单、参数化的查询构建(查询参数建模为映射)
* 本机[Java API和HTTP API](https://helpx.adobe.com/cn/experience-manager/6-3/sites/developing/using/querybuilder-api.html)
* [OOTB查询调试器](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/querybuilder-api.html#TestingandDebugging)
* [OOTB预测支](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/querybuilder-predicate-reference.html) 持常见查询要求

* 可扩展的API，允许开发自定义[查询谓词](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/implementing-custom-predicate-evaluator.html)
* JCR-SQL2和XPath可以通过[[!DNL Sling]](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html#findResources-java.lang.String-java.lang.String-)和[JCR API](https://docs.adobe.com/docs/en/spec/jsr170/javadocs/jcr-2.0/javax/jcr/query/package-summary.html)直接执行，返回结果分别为[[!DNL Sling] Resources](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/Resource.html)或[JCR Nodes](https://docs.adobe.com/docs/en/spec/jsr170/javadocs/jcr-2.0/javax/jcr/Node.html)。

>[!CAUTION]
>
>AEM QueryBuilder API会泄露ResourceResolver对象。 要减轻此漏洞，请遵循此[代码示例](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/core/src/main/java/com/adobe/acs/samples/search/querybuilder/impl/SampleQueryBuilder.java#L164)。


## [!DNL Sling] API

* [**Apache  [!DNL Sling] API JavaDocs**](https://sling.apache.org/apidocs/sling10/)

[Apache [!DNL Sling]](https://sling.apache.org/) 是支持AEM的REST风格的Web框架。[!DNL Sling] 提供HTTP请求路由，将JCR节点建模为资源，提供安全上下文等。

[!DNL Sling] API具有为扩展而构建的额外优势，这意味着与扩展性较低的JCR API相比，使用API构建的应用程序的行为 [!DNL Sling] 更简单、安全。

### [!DNL Sling] API的常用

* 以[[!DNL Sling Resources]](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/Resource.html)的形式访问JCR节点，并通过[ValueMaps](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ValueMap.html)访问其数据。

* 通过[ResourceResolver](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html)提供安全上下文。
* 通过ResourceResolver的[创建/移动/复制/删除方法](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html)创建和删除资源。
* 通过[ModiableValueMap](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ModifiableValueMap.html)更新属性。
* 构建请求处理构件块

   * [Servlet](https://sling.apache.org/documentation/the-sling-engine/servlets.html)
   * [Servlet过滤器](https://sling.apache.org/documentation/the-sling-engine/filters.html)

* 异步工作处理构件块

   * [事件和作业处理程序](https://sling.apache.org/documentation/bundles/apache-sling-eventing-and-job-handling.html)
   * [调度程序](https://sling.apache.org/documentation/bundles/scheduler-service-commons-scheduler.html)
   * [Sling 模型](https://sling.apache.org/documentation/bundles/models.html)

* [服务用户](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-service-users.html)

## JCR API

* **[JCR 2.0 JavaDocs](https://docs.adobe.com/docs/en/spec/javax.jcr/javadocs/jcr-2.0/index.html)**

[JCR（Java内容存储库）2.0 API](https://docs.adobe.com/docs/en/spec/javax.jcr/javadocs/jcr-2.0/index.html)是JCR实施规范的一部分(对于AEM，为[Apache Jackrabbit Oak](https://jackrabbit.apache.org/oak/))。 所有JCR实施都必须符合并实施这些API，因此，它是与AEM内容交互的最低级API。

JCR本身是一个基于层次/树的NoSQL数据存储AEM用作其内容存储库。 JCR拥有大量受支持的API，从内容CRUD到查询内容。 尽管有这种强大的API，但与更高级别的AEM和[!DNL Sling]抽象相比，它们仍很少被青睐。

总是喜欢JCR API，而不是Apache Jackrabbit Oak API。 JCR API用于与JCR存储库&#x200B;***交互***，而Oak API用于&#x200B;***实现*** JCR存储库。

### 对JCR API的常见误解

虽然JCR是AEM内容存储库，但其API并非与内容交互的首选方法。 相反，您更喜欢AEM API（页面、资产、标记等） 或Sling Resource API，因为它们提供了更好的抽象。

>[!CAUTION]
>
>JCR API的会话和节点接口在AEM应用程序中的广泛使用是代码味。 请确保不应改用[!DNL Sling] API。

### JCR API的常用

* [访问控制管理](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-service-users.html)
* [可授权的管理（用户/组）](https://jackrabbit.apache.org/api/2.8/org/apache/jackrabbit/api/security/user/package-summary.html)
* JCR观察(监听JCR事件)
* 创建深层节点结构

   * 虽然Sling API支持创建资源，但JCR API在[JcrUtils](https://jackrabbit.apache.org/api/2.10/index.html?org/apache/jackrabbit/commons/JcrUtils.html)和[JcrUtil](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/day/cq/commons/jcr/JcrUtil.html)中具有方便的方法，可加快创建深层结构的速度。

## OSGi API

* [**OSGi R6 JavaDocs**](https://osgi.org/javadoc/r6/cmpn/index.html?overview-summary.html)
* **[OSGi Declationative Services 1.2组件注释JavaDocs](https://osgi.org/javadoc/r6/cmpn/org/osgi/service/component/annotations/package-summary.html)**
* **[OSGi Declationative Services 1.2元类型注释JavaDocs](https://osgi.org/javadoc/r6/cmpn/org/osgi/service/metatype/annotations/package-summary.html)**
* [**OSGi Framework JavaDocs**](https://osgi.org/javadoc/r6/core/org/osgi/framework/package-summary.html)

OSGi API与更高级别的API(AEM、[!DNL Sling]和JCR)之间几乎没有重叠，使用OSGi API的需求非常少，并且需要高级AEM开发专业知识。

### OSGi与Apache Felix API

OSGi定义了所有OSGi容器必须实现和符合的规范。 AEM OSGi实施Apache Felix也提供了几个自己的API。

* 相对于Apache Felix API(`org.apache.felix`)，首选OSGi API(`org.osgi`)。

### OSGi API的常用

* 用于声明OSGi服务和组件的OSGi注释。

   * 在声明OSGi服务和组件时，首选[OSGi声明服务(DS)1.2注释](https://osgi.org/javadoc/r6/cmpn/org/osgi/service/component/annotations/package-summary.html)，而非[Felix SCR注释](https://felix.apache.org/documentation/subprojects/apache-felix-maven-scr-plugin/scr-annotations.html)

* 用于动态代码内[取消/注册OSGi服务/组件](https://osgi.org/javadoc/r6/core/org/osgi/framework/package-summary.html)的OSGi API。

   * 当不需要条件式OSGi服务/组件管理时（通常情况下），最好使用OSGi DS 1.2注释。

## 规则的例外

以下是上述定义的规则的常见例外。

### AEM Asset API

* 比[`com.adobe.granite.asset.api`](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/asset/api/package-summary.html)更偏好[`com.day.cq.dam.api`](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/day/cq/dam/api/package-summary.html)。

   * 而`com.day.cq`资产API则为AEM资产管理用例提供了更多免费工具。
   * Granite Assets API支持低级资产管理用例（版本、关系）。

### 查询API

* AEM QueryBuilder不支持某些查询函数，如[subgenties](https://jackrabbit.apache.org/oak/docs/query/query-engine.html#Suggestions)、拼写检查和索引提示等其它不常用函数。 要查询这些函数，首选JCR-SQL2。

### [!DNL Sling] Servlet注册  {#sling-servlet-registration}

* [!DNL Sling] servlet注册，首选 [OSGi DS 1.2注释，带@SlingServletResourceTypesover](https://sling.apache.org/documentation/the-sling-engine/servlets.html)   `@SlingServlet`

### [!DNL Sling] 过滤器注册  {#sling-filter-registration}

* [!DNL Sling] 筛选器注册，首选 [OSGi DS 1.2批注，带@SlingServletFilterover](https://sling.apache.org/documentation/the-sling-engine/filters.html)   `@SlingFilter`

## 有用的代码片段

以下是有用的Java代码片段，它们使用讨论的API说明常见用例的最佳实践。 这些片段还说明了如何从较不首选的API移动到较首选的API。

### 到[!DNL Sling] ResourceResolver的JCR会话

#### 自动关闭Sling ResourceResolver

自AEM 6.2起，[!DNL Sling] ResourceResolver在[try-with-resources](https://docs.oracle.com/javase/tutorial/essential/exceptions/tryResourceClose.html)语句中为`AutoClosable`。 使用此语法，不需要显式调用`resourceResolver .close()`。

```java
@Reference
ResourceResolverFactory rrf;
...
Map<String, Object> authInfo = new HashMap<String, Object>();
authInfo.put(JcrResourceConstants.AUTHENTICATION_INFO_SESSION, jcrSession);

try (ResourceResolver resourceResolver = rrf.getResourceResolver(authInfo)) {
    // Do work with the resourceResolver
} catch (LoginException e) { .. }
```

#### 手动关闭的Sling ResourceResolver

如果不能使用上述自动关闭技术，则必须手动关闭`finally`块中的ResourceResolver。

```java
@Reference
ResourceResolverFactory rrf;
...
Map<String, Object> authInfo = new HashMap<String, Object>();
authInfo.put(JcrResourceConstants.AUTHENTICATION_INFO_SESSION, jcrSession);

ResourceResolver resourceResolver = null;

try {
    resourceResolver = rrf.getResourceResolver(authInfo);
    // Do work with the resourceResolver
} catch (LoginException e) {
   ...
} finally {
    if (resourceResolver != null) { resourceResolver.close(); }
}
```

### JCR到[!DNL Sling] [!DNL Resource]的路径

```java
Resource resource = ResourceResolver.getResource("/path/to/the/resource");
```

### JCR节点到[!DNL Sling] [!DNL Resource]

```java
Resource resource = resourceResolver.getResource(node.getPath());
```

### [!DNL Sling] [!DNL Resource] 到AEM资产

#### 建议的方法

`DamUtil.resolveToAsset(..)`根据需要，通过 `dam:Asset` 向上遍历树，解析位于“资产”对象下的任何资源。

```java
Asset asset = DamUtil.resolveToAsset(resource);
```

#### 替代方法

使资源适应资产需要资源本身作为`dam:Asset`节点。

```java
Asset asset = resource.adaptTo(Asset.class);
```

### [!DNL Sling] 资源至AEM页

#### 建议的方法

`pageManager.getContainingPage(..)` 根据需要向上 `cq:Page` 走树，解析位于Page对象下的任何资源。

```java
PageManager pageManager = resourceResolver.adaptTo(PageManager.class);
Page page = pageManager.getContainingPage(resource);
Page page2 = pageManager.getContainingPage("/content/path/to/page/jcr:content/or/component");
```

#### 替代方法{#alternative-approach-1}

使资源适应页面需要资源本身作为`cq:Page`节点。

```java
Page page = resource.adaptTo(Page.class);
```

### 阅读AEM页面属性

使用Page对象的getter获取已知属性（`getTitle()`、`getDescription()`等） 和`page.getProperties()`以获取用于检索其他属性的`[cq:Page]/jcr:content` ValueMap。

```java
Page page = resource.adaptTo(Page.class);
String title = page.getTitle();
Calendar value = page.getProperties().get("cq:lastModified", Calendar.getInstance());
```

### 阅读AEM资产元数据属性

资产API提供了从`[dam:Asset]/jcr:content/metadata`节点读取属性的简便方法。 请注意，这不是ValueMap，不支持第2个参数（默认值和自动类型转换）。

```java
Asset asset = resource.adaptTo(Asset.class);
String title = asset.getMetadataValue("dc:title");
Calendar lastModified = (Calendar) asset.getMetadata("cq:lastModified");
```

### 读取[!DNL Sling] [!DNL Resource]属性{#read-sling-resource-properties}

当属性存储在AEM API(Page， Asset)无法直接访问的位置（属性或相对资源）中时，[!DNL Sling]资源和ValueMaps可用于获取数据。

```java
ValueMap properties = resource.getValueMap();
String value = properties.get("jcr:title", "Default title");
String relativeResourceValue = properties.get("relative/propertyName", "Default value");
```

在这种情况下，AEM对象可能必须转换为[!DNL Sling] [!DNL Resource]以有效定位所需的属性或子资源。

#### AEM页至[!DNL Sling] [!DNL Resource]

```java
Resource resource = page.adaptTo(Resource.class);
```

#### AEM资产到[!DNL Sling] [!DNL Resource]

```java
Resource resource = asset.adaptTo(Resource.class);
```

### 使用[!DNL Sling]的ModifableValueMap写入属性

使用[!DNL Sling]的[ ModiableValueMap](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ModifiableValueMap.html)将属性写入节点。 这只能写入直接节点（不支持相对属性路径）。

请注意，对`.adaptTo(ModifiableValueMap.class)`的调用要求对资源具有写入权限，否则将返回null。

```java
ModifiableValueMap properties = resource.adaptTo(ModifiableValueMap.class);

properties.put("newPropertyName", "new value");
properties.put("propertyNameToUpdate", "updated value");
properties.remove("propertyToRemove");

resource.getResourceResolver().commit();
```

### 创建AEM页面

在AEM中正确定义和初始化页面时，需要始终使用PageManager创建页面，就像使用页面模板一样。

```java
String templatePath = "/conf/my-app/settings/wcm/templates/content-page";
boolean autoSave = true;

PageManager pageManager = resourceResolver.adaptTo(PageManager.class);
pageManager.create("/content/parent/path", "my-new-page", templatePath, "My New Page Title", autoSave);

if (!autoSave) { resourceResolver.commit(); }
```

### 创建[!DNL Sling]资源

ResourceResolver支持创建资源的基本操作。 创建更高级抽象时(AEM页面、资产、标记等) 使用各自经理提供的方法。

```java
resourceResolver.create(parentResource, "my-node-name", new ImmutableMap.Builder<String, Object>()
           .put("jcr:primaryType", "nt:unstructured")
           .put("jcr:title", "Hello world")
           .put("propertyName", "Other initial properties")
           .build());

resourceResolver.commit();
```

### 删除[!DNL Sling]资源

ResourceResolver支持删除资源。 创建更高级抽象时(AEM页面、资产、标记等) 使用各自经理提供的方法。

```java
resourceResolver.delete(resource);

resourceResolver.commit();
```
