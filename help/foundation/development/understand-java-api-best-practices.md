---
title: AEM中的Java API最佳实践
description: AEM基于丰富的开源软件堆栈构建，该堆栈会公开许多Java API，以供在开发过程中使用。 本文探讨了主要API以及何时以及为何应使用它们。
version: 6.4, 6.5
feature: APIs
topic: Development
role: Developer
level: Beginner
exl-id: b613aa65-f64b-4851-a2af-52e28271ce88
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '2071'
ht-degree: 2%

---

# Java API最佳实践

Adobe Experience Manager(AEM)构建于一个丰富的开源软件堆栈上，该堆栈会公开许多Java API，以供在开发过程中使用。 本文探讨了主要API以及何时以及为何应使用它们。

AEM基于4个主Java API集构建。

* **Adobe Experience Manager (AEM)**

   * 产品抽象概念，如页面、资产、工作流等。

* **Apache Sling Web Framework**

   * REST和基于资源的抽象概念，如资源、值映射和HTTP请求。

* **JCR(Apache Jackrabbit Oak)**

   * 数据和内容抽象，如节点、属性和会话。

* **OSGi(Apache Felix)**

   * OSGi应用程序容器抽象，如服务和(OSGi)组件。

## Java API首选项“经验规则”

一般规则是首选按以下顺序使用API/抽象：

1. **AEM**
1. **Sling**
1. **JCR**
1. **OSGi**

如果AEM提供的API比较好 [!DNL Sling]、JCR和OSGi. 如果AEM不提供API，则首选 [!DNL Sling] JCR和OSGi。

此顺序是一般规则，表示存在例外。 违反此规则的可接受原因包括：

* 众所周知的例外，如下所述。
* 更高级别的API中不提供必需的功能。
* 在现有代码(自定义或AEM产品代码)的上下文中操作，该代码本身使用的是不太首选的API，而移动到新API的成本是不合理的。

   * 与创建混合使用，最好始终使用较低级别的API。

## AEM API

* [**AEM API JavaDocs**](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/index.html)

AEM API提供了特定于产品化用例的抽象概念和功能。

例如， AEM [PageManager](https://developer.adobe.com/experience-manager/reference-materials/cloud-service/javadoc/com/day/cq/wcm/api/PageManager.html) 和 [页面](https://developer.adobe.com/experience-manager/reference-materials/cloud-service/javadoc/com/day/cq/wcm/api/Page.html) API提供了 `cq:Page` AEM中表示网页的节点。

而这些节点可通过 [!DNL Sling] API作为资源，JCR API作为节点，AEM API为常见用例提供了抽象概念。 使用AEM API可确保产品之间的AEM行为保持一致，并可自定义和扩展到AEM。

### com.adobe.&#42; 与com.day.&#42; API

AEM API具有一个包内首选项，该首选项由以下Java包按照优先顺序标识：

1. `com.adobe.cq`
1. `com.adobe.granite`
1. `com.day.cq`

`com.adobe.cq` 支持产品用例， `com.adobe.granite` 支持跨产品平台用例，例如工作流或任务（在产品之间使用）：AEM Assets、站点等)。

`com.day.cq` 包含“原始”API。 这些API解决了在Adobe获取之前和/或围绕客户获取 [!DNL Day CQ]. 这些API受支持，不应避免使用，除非 `com.adobe.cq` 或 `com.adobe.granite` 提供（较新）替代方法。

新的抽象概念，例如 [!DNL Content Fragments] 和 [!DNL Experience Fragments] 是在 `com.adobe.cq` 空间而不是 `com.day.cq` 下文描述。

### 查询API

AEM支持多种查询语言。 3种主要语言包括 [JCR-SQL2](https://docs.jboss.org/jbossdna/0.7/manuals/reference/html/jcr-query-and-search.html)、 XPath和 [AEM查询生成器](https://helpx.adobe.com/cn/experience-manager/6-5/sites/developing/using/querybuilder-api.html).

最重要的问题是在整个代码库中维护一致的查询语言，以降低复杂性和了解成本。

与 [!DNL Apache Oak] 将它们转移到JCR-SQL2中以执行最终查询，与查询时间本身相比，转换到JCR-SQL2的时间可以忽略不计。

首选API是 [AEM查询生成器](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/querybuilder-api.html)，这是最高级别的抽象，为构建、执行和检索查询结果提供了一个强大的API，并提供了以下内容：

* 简单、参数化的查询构建（建模为映射的查询参数）
* 本机 [Java API和HTTP API](https://helpx.adobe.com/cn/experience-manager/6-3/sites/developing/using/querybuilder-api.html)
* [AEM查询调试器](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/querybuilder-api.html#TestingandDebugging)
* [AEM谓词](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/querybuilder-predicate-reference.html) 支持常见查询要求

* 可扩展API，允许开发自定义 [查询谓词](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/implementing-custom-predicate-evaluator.html)
* JCR-SQL2和XPath可以直接通过 [[!DNL Sling]](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html#findResources-java.lang.String-java.lang.String-) 和 [JCR API](https://developer.adobe.com/experience-manager/reference-materials/spec/javax.jcr/javadocs/jcr-2.0/index.html)，返回结果 [[!DNL Sling] 资源](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/Resource.html) 或 [JCR节点](https://developer.adobe.com/experience-manager/reference-materials/spec/javax.jcr/javadocs/jcr-2.0/javax/jcr/Node.html)，分别为。

>[!CAUTION]
>
>AEM QueryBuilder API会泄漏ResourceResolver对象。 要缓解此漏洞，请遵循 [代码示例](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/core/src/main/java/com/adobe/acs/samples/search/querybuilder/impl/SampleQueryBuilder.java#L164).

## [!DNL Sling] API

* [**Apache [!DNL Sling] API JavaDocs**](https://sling.apache.org/apidocs/sling10/)

[Apache [!DNL Sling]](https://sling.apache.org/) 是支持AEM的RESTful Web框架。 [!DNL Sling] 提供HTTP请求路由、将JCR节点建模为资源、提供安全上下文等。

[!DNL Sling] 为扩展构建API具有额外的好处，这意味着增加使用构建的应用程序的行为通常会更简单、更安全 [!DNL Sling] API，而不是扩展性较差的JCR API。

### 的常见用法 [!DNL Sling] API

* 作为访问JCR节点 [[!DNL Sling Resources]](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/Resource.html) 通过 [值映射](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ValueMap.html).

* 通过提供安全上下文 [ResourceResolver](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html).
* 通过ResourceResolver的创建和删除资源 [创建/移动/复制/删除方法](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html).
* 通过更新属性 [ModiableValueMap](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ModifiableValueMap.html).
* 构建请求处理构建基块

   * [Servlet](https://sling.apache.org/documentation/the-sling-engine/servlets.html)
   * [Servlet过滤器](https://sling.apache.org/documentation/the-sling-engine/filters.html)

* 异步工作处理构建块

   * [事件和作业处理程序](https://sling.apache.org/documentation/bundles/apache-sling-eventing-and-job-handling.html)
   * [调度程序](https://sling.apache.org/documentation/bundles/scheduler-service-commons-scheduler.html)
   * [Sling 模型](https://sling.apache.org/documentation/bundles/models.html)

* [服务用户](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-service-users.html)

## JCR API

* **[JCR 2.0 JavaDocs](https://developer.adobe.com/experience-manager/reference-materials/spec/javax.jcr/javadocs/jcr-2.0/index.html)**

的 [JCR（Java内容存储库）2.0 API](https://developer.adobe.com/experience-manager/reference-materials/spec/javax.jcr/javadocs/jcr-2.0/index.html) 是JCR实施规范的一部分(对于AEM, [Apache Jackrabbit Oak](https://jackrabbit.apache.org/oak/))。 所有JCR实施都必须符合并实施这些API，因此，是与AEM内容交互的最低级别API。

JCR本身是基于层次/树的NoSQL数据存储AEM用作其内容存储库。 JCR具有大量受支持的API，从内容CRUD到查询内容，不一而足。 尽管有这个强大的API，但与更高级别的AEM和 [!DNL Sling] 抽象。

与Apache Jackrabbit Oak API相比，始终更喜欢JCR API。 JCR API适用于 ***交互*** 具有JCR存储库，而Oak API适用于 ***实施*** JCR存储库。

### 关于JCR API的常见误解

虽然JCR是AEM内容存储库，但其API并不是与内容交互的首选方法。 相反，首选AEM API（页面、资产、标记等） 或Sling资源API，因为它们提供了更好的抽象概念。

>[!CAUTION]
>
>在AEM应用程序中，JCR API的会话和节点接口的广泛使用是代码气味。 确保 [!DNL Sling] 不应改用API。

### JCR API的常见用法

* [访问控制管理](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security-service-users.html)
* [可授权的管理（用户/组）](https://jackrabbit.apache.org/api/2.12/org/apache/jackrabbit/api/security/user/package-summary.html)
* JCR观察（侦听JCR事件）
* 创建深层节点结构

   * 虽然Sling API支持创建资源，但JCR API在 [JcrUtils](https://jackrabbit.apache.org/api/2.12/org/apache/jackrabbit/commons/JcrUtils.html) 和 [JcrUtil](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/day/cq/commons/jcr/JcrUtil.html) 加快了深层结构的创建。

## OSGi API

* [**OSGi R6 JavaDocs**](https://osgi.org/javadoc/r6/cmpn/index.html?overview-summary.html)
* **[OSGi声明性服务1.2组件批注JavaDocs](https://osgi.org/javadoc/r6/cmpn/org/osgi/service/component/annotations/package-summary.html)**
* **[OSGi声明性服务1.2元类型批注JavaDocs](https://osgi.org/javadoc/r6/cmpn/org/osgi/service/metatype/annotations/package-summary.html)**
* [**OSGi框架JavaDocs**](https://osgi.org/javadoc/r6/core/org/osgi/framework/package-summary.html)

OSGi API与更高级别的API(AEM、 [!DNL Sling]、和JCR)，并且很少需要使用OSGi API，并且需要高级AEM开发专业知识。

### OSGi与Apache Felix API

OSGi定义了所有OSGi容器必须实施和符合的规范。 AEM OSGi实施Apache Felix也提供了几个自己的API。

* 首选OSGi API(`org.osgi`)覆盖Apache Felix API(`org.apache.felix`)。

### OSGi API的常见用法

* 用于声明OSGi服务和组件的OSGi批注。

   * 首选 [OSGi声明性服务(DS)1.2注释](https://osgi.org/javadoc/r6/cmpn/org/osgi/service/component/annotations/package-summary.html) over [Felix SCR注释](https://felix.apache.org/documentation/subprojects/apache-felix-maven-scr-plugin/scr-annotations.html) 用于声明OSGi服务和组件

* 用于动态代码内的OSGi API [取消/注册OSGi服务/组件](https://osgi.org/javadoc/r6/core/org/osgi/framework/package-summary.html).

   * 当不需要条件性OSGi服务/组件管理时（大多数情况下是这样），最好使用OSGi DS 1.2注释。

## 规则例外

以下是上述规则的常见例外。

### OSGi API

在处理低级OSGi抽象（如在OSGi组件属性中定义或读取）时，提供的较新抽象 `org.osgi` 优先于较高级别的Sling基础。 相互竞争的Sling抽象概念尚未被标记为 `@Deprecated` 建议 `org.osgi` 替换。

另请注意OSGi配置节点定义首选 `cfg.json` 在 `sling:OsgiConfig` 格式。

### AEM Asset API

* 首选 [ `com.day.cq.dam.api`](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/day/cq/dam/api/package-summary.html) over [ `com.adobe.granite.asset.api`](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/asset/api/package-summary.html).

   * 而 `com.day.cq` Assets API为AEM资产管理用例提供了更免费的工具。
   * Granite Assets API支持低级别资产管理用例（版本、关系）。

### 查询API

* AEM QueryBuilder不支持某些查询函数，例如 [建议](https://jackrabbit.apache.org/oak/docs/query/query-engine.html#Suggestions)、拼写检查和索引提示以及其他不太常用的函数。 要使用这些函数进行查询，首选JCR-SQL2。

### [!DNL Sling] Servlet注册 {#sling-servlet-registration}

* [!DNL Sling] Servlet注册，首选 [OSGi DS 1.2批注，带@SlingServletResourceTypes](https://sling.apache.org/documentation/the-sling-engine/servlets.html) over `@SlingServlet`

### [!DNL Sling] 过滤器注册 {#sling-filter-registration}

* [!DNL Sling] 筛选器注册，首选 [OSGi DS 1.2批注，带@SlingServletFilter](https://sling.apache.org/documentation/the-sling-engine/filters.html) over `@SlingFilter`

## 有用的代码片段

以下是有用的Java代码片段，它们使用讨论的API说明常见用例的最佳实践。 这些片段还说明了如何从较不首选的API移动到更首选的API。

### JCR会话至 [!DNL Sling] ResourceResolver

#### 自动关闭Sling ResourceResolver

自AEM 6.2起， [!DNL Sling] ResourceResolver为 `AutoClosable` 在 [试用资源](https://docs.oracle.com/javase/tutorial/essential/exceptions/tryResourceClose.html) 语句。 使用此语法，即 `resourceResolver .close()` 不需要。

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

ResourceResolvers必须在 `finally` 块，则无法使用上面显示的自动关闭技术。

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

### JCR路径 [!DNL Sling] [!DNL Resource]

```java
Resource resource = ResourceResolver.getResource("/path/to/the/resource");
```

### JCR节点到 [!DNL Sling] [!DNL Resource]

```java
Resource resource = resourceResolver.getResource(node.getPath());
```

### [!DNL Sling] [!DNL Resource] AEM资产

#### 建议的方法

`DamUtil.resolveToAsset(..)`解析下的任何资源 `dam:Asset` 按需要向上走树，以返回到资产对象。

```java
Asset asset = DamUtil.resolveToAsset(resource);
```

#### 替代方法

使资源适应资产需要资源本身作为 `dam:Asset` 节点。

```java
Asset asset = resource.adaptTo(Asset.class);
```

### [!DNL Sling] “资源到AEM”页

#### 建议的方法

`pageManager.getContainingPage(..)` 解析下的任何资源 `cq:Page` 根据需要向上走树以返回到Page对象。

```java
PageManager pageManager = resourceResolver.adaptTo(PageManager.class);
Page page = pageManager.getContainingPage(resource);
Page page2 = pageManager.getContainingPage("/content/path/to/page/jcr:content/or/component");
```

#### 替代方法 {#alternative-approach-1}

使资源适应页面需要资源本身作为 `cq:Page` 节点。

```java
Page page = resource.adaptTo(Page.class);
```

### 读取AEM页面属性

使用页面对象的getter获取已知属性(`getTitle()`, `getDescription()`等) 和 `page.getProperties()` 获取 `[cq:Page]/jcr:content` 用于检索其他属性的ValueMap。

```java
Page page = resource.adaptTo(Page.class);
String title = page.getTitle();
Calendar value = page.getProperties().get("cq:lastModified", Calendar.getInstance());
```

### 读取AEM资产元数据属性

资产API为从 `[dam:Asset]/jcr:content/metadata` 节点。 请注意，这不是ValueMap，不支持第2个参数（默认值和自动类型转换）。

```java
Asset asset = resource.adaptTo(Asset.class);
String title = asset.getMetadataValue("dc:title");
Calendar lastModified = (Calendar) asset.getMetadata("cq:lastModified");
```

### 读取 [!DNL Sling] [!DNL Resource] 属性 {#read-sling-resource-properties}

当属性存储在AEM API（页面、资产）无法直接访问的位置（属性或相对资源）中时， [!DNL Sling] 资源和值映射可用于获取数据。

```java
ValueMap properties = resource.getValueMap();
String value = properties.get("jcr:title", "Default title");
String relativeResourceValue = properties.get("relative/propertyName", "Default value");
```

在这种情况下，AEM对象可能必须转换为 [!DNL Sling] [!DNL Resource] 以有效地找到所需属性或子资源。

#### AEM页面到 [!DNL Sling] [!DNL Resource]

```java
Resource resource = page.adaptTo(Resource.class);
```

#### AEM资产到 [!DNL Sling] [!DNL Resource]

```java
Resource resource = asset.adaptTo(Resource.class);
```

### 使用写入属性 [!DNL Sling]s ModiableValueMap

使用 [!DNL Sling]&#39;s [ModiableValueMap](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ModifiableValueMap.html) 将属性写入节点。 这只能写入到即时节点（不支持相对属性路径）。

请注意 `.adaptTo(ModifiableValueMap.class)` 需要对资源具有写入权限，否则将返回null。

```java
ModifiableValueMap properties = resource.adaptTo(ModifiableValueMap.class);

properties.put("newPropertyName", "new value");
properties.put("propertyNameToUpdate", "updated value");
properties.remove("propertyToRemove");

resource.getResourceResolver().commit();
```

### 创建AEM页面

在AEM中正确定义和初始化页面时，需要始终使用PageManager创建页面模板。

```java
String templatePath = "/conf/my-app/settings/wcm/templates/content-page";
boolean autoSave = true;

PageManager pageManager = resourceResolver.adaptTo(PageManager.class);
pageManager.create("/content/parent/path", "my-new-page", templatePath, "My New Page Title", autoSave);

if (!autoSave) { resourceResolver.commit(); }
```

### 创建 [!DNL Sling] 资源

ResourceResolver支持创建资源的基本操作。 创建更高级别的抽象概念(AEM页面、资产、标记等) 使用其各自经理提供的方法。

```java
resourceResolver.create(parentResource, "my-node-name", new ImmutableMap.Builder<String, Object>()
           .put("jcr:primaryType", "nt:unstructured")
           .put("jcr:title", "Hello world")
           .put("propertyName", "Other initial properties")
           .build());

resourceResolver.commit();
```

### 删除 [!DNL Sling] 资源

ResourceResolver支持删除资源。 创建更高级别的抽象概念(AEM页面、资产、标记等) 使用其各自经理提供的方法。

```java
resourceResolver.delete(resource);

resourceResolver.commit();
```
