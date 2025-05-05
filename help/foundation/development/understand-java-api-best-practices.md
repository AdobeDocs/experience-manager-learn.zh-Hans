---
title: Java&amp；trade；AEM中的API最佳实践
description: AEM构建在丰富的开源软件栈栈上，该栈栈公开许多Java&amp；trade；API以在开发过程中使用。 本文探讨了主要API以及何时使用它们以及为什么使用它们。
version: Experience Manager 6.4, Experience Manager 6.5
feature: APIs
topic: Development
role: Developer
level: Beginner
doc-type: Article
exl-id: b613aa65-f64b-4851-a2af-52e28271ce88
last-substantial-update: 2022-06-24T00:00:00Z
thumbnail: aem-java-bp.jpg
duration: 416
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1726'
ht-degree: 0%

---

# Java™ API最佳实践

Adobe Experience Manager (AEM)构建在丰富的开源软件栈栈上，该栈栈公开许多Java™ API以供在开发期间使用。 本文探讨了主要API以及何时使用它们以及为什么使用它们。

AEM基于四个主要Java™ API集构建。

* **Adobe Experience Manager (AEM)**

   * 产品抽象，如页面、资产、工作流等。

* **Apache Sling Web Framework**

   * REST和基于资源的抽象，如资源、值映射和HTTP请求。

* **JCR (Apache Jackrabbit Oak)**

   * 数据和内容抽象，如节点、属性和会话。

* **OSGi (Apache Felix)**

   * OSGi应用程序容器抽象，例如服务和(OSGi)组件。

## Java™ API首选项“经验法则”

一般规则是按照以下顺序优先使用API/抽象：

1. **AEM**
1. **Sling**
1. **JCR**
1. **OSGi**

如果API由AEM提供，则首选它而不是[!DNL Sling]、JCR和OSGi。 如果AEM不提供API，则首选[!DNL Sling]，而不是JCR和OSGi。

此顺序是一般规则，表示存在例外。 可以接受脱离此规则的原因包括：

* 众所周知的例外，如下所述。
* 所需功能在更高级别的API中不可用。
* 在现有代码(自定义或AEM产品代码)的上下文中操作，而现有代码本身使用不太首选的API，迁移到新API的成本是不合理的。

   * 始终使用较低级别的API比创建混合更好。

## AEM API

* [**AEM API JavaDocs**](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/index.html)

AEM API提供了特定于产品化用例的抽象和功能。

例如，AEM的[PageManager](https://developer.adobe.com/experience-manager/reference-materials/cloud-service/javadoc/com/day/cq/wcm/api/PageManager.html)和[Page](https://developer.adobe.com/experience-manager/reference-materials/cloud-service/javadoc/com/day/cq/wcm/api/Page.html) API为AEM中表示网页的`cq:Page`节点提供了抽象概念。

虽然这些节点可通过[!DNL Sling] API作为资源使用，可通过JCR API作为节点使用，但AEM的API为常见用例提供了抽象概念。 使用AEM API可确保AEM产品与AEM的自定义项和扩展之间的行为一致。

### com.adobe.&#42;与com.day。&#42; API

AEM API具有包内首选项，按首选项顺序由以下Java™包标识：

1. `com.adobe.cq`
1. `com.adobe.granite`
1. `com.day.cq`

`com.adobe.cq`包支持产品用例，而`com.adobe.granite`支持跨产品平台用例，例如工作流或任务(跨产品使用：AEM Assets、Sites等)。

`com.day.cq`包包含“原始”API。 这些API用于解决在Adobe获取[!DNL Day CQ]之前和/或之后存在的核心抽象概念及功能。 这些API受支持，应避免使用，除非`com.adobe.cq`或`com.adobe.granite`包不提供（较新的）替代方案。

新的抽象概念（如[!DNL Content Fragments]和[!DNL Experience Fragments]）构建在`com.adobe.cq`空间中，而不是如下所述的`com.day.cq`中。

### 查询API

AEM支持多种查询语言。 三种主要语言为[JCR-SQL2](https://docs.jboss.org/jbossdna/0.7/manuals/reference/html/jcr-query-and-search.html)、XPath和[AEM Query Builder](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/query-builder/querybuilder-api.html)。

最重要的考虑是在代码库中维护一致的查询语言，以降低复杂性和理解成本。

所有查询语言都有效地具有相同的性能配置文件，因为[!DNL Apache Oak]将它们传输到JCR-SQL2以便最终查询执行，并且与JCR-SQL2的查询时间本身相比，转换到JCR-SQL2的时间可以忽略不计。

首选的API是[AEM Query Builder](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/query-builder/querybuilder-api.html)，它是最高级别的抽象，为构建、执行和检索查询结果提供了一个强大的API，并提供以下内容：

* 简单、参数化查询构造（以Map建模的查询参数）
* 本机[Java™ API和HTTP API](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions.html)
* [AEM Query Debugger](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/query-builder/querybuilder-api.html)
* [AEM谓词](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/query-builder/querybuilder-predicate-reference.html)支持常见查询要求

* 可扩展API，允许开发自定义[查询谓词](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions.html)
* JCR-SQL2和XPath可以通过[[!DNL Sling]](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html#findResources-java.lang.String-java.lang.String-)和[JCR API](https://developer.adobe.com/experience-manager/reference-materials/spec/javax.jcr/javadocs/jcr-2.0/index.html)直接执行，分别返回结果[[!DNL Sling] 资源](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/Resource.html)或[JCR节点](https://developer.adobe.com/experience-manager/reference-materials/spec/javax.jcr/javadocs/jcr-2.0/javax/jcr/Node.html)。

>[!CAUTION]
>
>AEM QueryBuilder API泄漏ResourceResolver对象。 要缓解此泄露，请按照此[代码示例](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/core/src/main/java/com/adobe/acs/samples/search/querybuilder/impl/SampleQueryBuilder.java#L164)操作。
>

## [!DNL Sling] API

* [**Apache [!DNL Sling] API JavaDocs**](https://sling.apache.org/apidocs/sling10/)

[Apache [!DNL Sling]](https://sling.apache.org/)是支持AEM的RESTful Web框架。 [!DNL Sling]提供HTTP请求路由，将JCR节点建模为资源，提供安全上下文等。

[!DNL Sling] API具有为扩展而构建的附加好处，这意味着与扩展性较低的JCR API相比，增强使用[!DNL Sling] API构建的应用程序的行为通常更容易、更安全。

### [!DNL Sling] API的常见用法

* 以[[!DNL Sling Resources]](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/Resource.html)身份访问JCR节点，并通过[ValueMaps](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ValueMap.html)访问其数据。

* 通过[ResourceResolver](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html)提供安全上下文。
* 通过ResourceResolver的[创建/移动/复制/删除方法](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html)创建和删除资源。
* 正在通过[ModifiableValueMap](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ModifiableValueMap.html)更新属性。
* 构建请求处理构建基块

   * [个Servlet](https://sling.apache.org/documentation/the-sling-engine/servlets.html)
   * [Servlet筛选器](https://sling.apache.org/documentation/the-sling-engine/filters.html)

* 异步处理构建基块

   * [事件和作业处理程序](https://sling.apache.org/documentation/bundles/apache-sling-eventing-and-job-handling.html)
   * [计划程序](https://sling.apache.org/documentation/bundles/scheduler-service-commons-scheduler.html)
   * [Sling 模型](https://sling.apache.org/documentation/bundles/models.html)

* [服务用户](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security-service-users.html)

## JCR API

* **[JCR 2.0 JavaDocs](https://developer.adobe.com/experience-manager/reference-materials/spec/javax.jcr/javadocs/jcr-2.0/index.html)**

[JCR (Java™ Content Repository) 2.0 API](https://developer.adobe.com/experience-manager/reference-materials/spec/javax.jcr/javadocs/jcr-2.0/index.html)是JCR实现规范的一部分(在AEM中，为[Apache Jackrabbit Oak](https://jackrabbit.apache.org/oak/docs/))。 所有JCR实施都必须符合并实施这些API，因此，它是与AEM内容进行交互的最低级别API。

JCR本身是基于层次结构/树的NoSQL数据存储，AEM将其用作内容存储库。 JCR具有大量受支持的API，从内容CRUD到查询内容。 尽管具有此强大的API，但很少比更高级别的AEM和[!DNL Sling]抽象更适合使用它们。

与Apache Jackrabbit Oak API相比，始终首选使用JCR API。 JCR API用于与JCR存储库&#x200B;***交互***，而Oak API用于实现&#x200B;***JCR存储库***。

### 关于JCR API的常见误解

虽然JCR是AEM的内容存储库，但其API不是与内容进行交互的首选方法。 相反，您更喜欢AEM API(Page、Assets、Tag等)或Sling资源API，因为它们提供了更好的抽象功能。

>[!CAUTION]
>
>在AEM应用程序中广泛使用JCR API的会话和节点接口是一种代码异味。 确保改用[!DNL Sling] API。

### JCR API的常见用法

* [访问控制管理](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security-service-users.html)
* [可授权管理（用户/组）](https://jackrabbit.apache.org/api/2.12/org/apache/jackrabbit/api/security/user/package-summary.html)
* JCR观察（侦听JCR事件）
* 创建深层节点结构

   * 虽然Sling API支持创建资源，但JCR API在[JcrUtils](https://jackrabbit.apache.org/api/2.12/org/apache/jackrabbit/commons/JcrUtils.html)和[JcrUtil](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/day/cq/commons/jcr/JcrUtil.html)中提供了方便的方法，可加快创建深层结构。

## OSGi API

* [**OSGi R6 JavaDocs**](https://docs.osgi.org/javadoc/r6/cmpn/index.html?overview-summary.html)
* **[OSGi Declarative Services 1.2组件注释JavaDocs](https://docs.osgi.org/javadoc/r6/cmpn/org/osgi/service/component/annotations/package-summary.html)**
* **[OSGi Declarative Services 1.2元类型注释JavaDocs](https://docs.osgi.org/javadoc/r6/cmpn/org/osgi/service/metatype/annotations/package-summary.html)**
* [**OSGi框架JavaDocs**](https://docs.osgi.org/javadoc/r6/core/org/osgi/framework/package-summary.html)

OSGi API与更高级别的API(AEM、[!DNL Sling]和JCR)之间很少重叠，很少需要使用OSGi API，并且需要高级别的AEM开发专业知识。

### OSGi与Apache Felix API

OSGi定义所有OSGi容器都必须实现并遵循的规范。 AEM的OSGi实施Apache Felix也提供了若干自己的API。

* 与Apache Felix API (`org.apache.felix`)相比，首选OSGi API (`org.osgi`)。

### OSGi API的常见用法

* 用于声明OSGi服务和组件的OSGi注释。

   * 在声明OSGi服务和组件时，优先使用[OSGi Declarative Services (DS) 1.2 Annotations](https://docs.osgi.org/javadoc/r6/cmpn/org/osgi/service/component/annotations/package-summary.html)，而不是[Felix SCR Annotations](https://felix.apache.org/documentation/subprojects/apache-felix-maven-scr-plugin/scr-annotations.html)

* 用于代码内动态[取消/注册OSGi服务/组件](https://docs.osgi.org/javadoc/r6/core/org/osgi/framework/package-summary.html)的OSGi API。

   * 当不需要条件OSGi服务/组件管理时（最常见的情况是），首选使用OSGi DS 1.2注释。

## 规则的例外

以下是上述定义的规则的常见例外。

### OSGi API

在处理低级OSGi抽象概念（例如在OSGi组件属性中定义或读取）时，`org.osgi`提供的较新抽象概念优先于较高级别的Sling抽象概念。 竞争的Sling抽象尚未标记为`@Deprecated`并建议`org.osgi`替代方案。

另请注意，与`sling:OsgiConfig`格式相比，OSGi配置节点定义更倾向于使用`cfg.json`。

### AEM Asset API

* 与[`com.adobe.granite.asset.api`](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/adobe/granite/asset/api/package-summary.html)相比，优先选择[`com.day.cq.dam.api`](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/day/cq/dam/api/package-summary.html)。

   * 而`com.day.cq` Assets API为AEM的资源管理用例提供了更多免费工具。
   * Granite Assets API支持低级资产管理用例（版本、关系）。

### 查询API

* AEM QueryBuilder不支持某些查询函数，例如[建议](https://jackrabbit.apache.org/oak/docs/query/query-engine.html#Suggestions)、拼写检查和索引提示以及其他不太常见的函数。 首选使用JCR-SQL2函数进行查询。

### [!DNL Sling] Servlet注册 {#sling-servlet-registration}

* [!DNL Sling] servlet注册，与`@SlingServlet`相比，更喜欢带有@SlingServletResourceTypes[&#128279;](https://sling.apache.org/documentation/the-sling-engine/servlets.html)的OSGi DS 1.2注释

### [!DNL Sling]筛选器注册 {#sling-filter-registration}

* [!DNL Sling]筛选器注册，首选带有[&#128279;](https://sling.apache.org/documentation/the-sling-engine/filters.html)的OSGi DS 1.2@SlingServletFilter注释而非`@SlingFilter`

## 有用的代码段

下面是一些有用的Java™代码片段，它们说明了使用讨论的API的常见用例的最佳实践。 这些代码片段还说明了如何从不太首选的API迁移到更首选的API。

### 到[!DNL Sling] ResourceResolver的JCR会话

#### 自动关闭Sling ResourceResolver

自AEM 6.2起，在[try-with-resources](https://docs.oracle.com/javase/tutorial/essential/exceptions/tryResourceClose.html)语句中，[!DNL Sling] ResourceResolver为`AutoClosable`。 使用此语法时，不需要显式调用`resourceResolver .close()`。

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

如果无法使用以上显示的自动关闭技术，则必须在`finally`块中手动关闭ResourceResolvers。

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

### [!DNL Sling]的JCR路径[!DNL Resource]

```java
Resource resource = ResourceResolver.getResource("/path/to/the/resource");
```

### JCR节点到[!DNL Sling] [!DNL Resource]

```java
Resource resource = resourceResolver.getResource(node.getPath());
```

### [!DNL Sling] [!DNL Resource]到AEM资源

#### 推荐的方法

`DamUtil.resolveToAsset(..)`函数根据需要向上浏览树将`dam:Asset`下的任何资源解析为Asset对象。

```java
Asset asset = DamUtil.resolveToAsset(resource);
```

#### 替代方法

使资源适应资产需要资源本身是`dam:Asset`节点。

```java
Asset asset = resource.adaptTo(Asset.class);
```

### [!DNL Sling]资源到AEM页面

#### 推荐的方法

`pageManager.getContainingPage(..)`根据需要通过向上浏览树将`cq:Page`下的任何资源解析为页面对象。

```java
PageManager pageManager = resourceResolver.adaptTo(PageManager.class);
Page page = pageManager.getContainingPage(resource);
Page page2 = pageManager.getContainingPage("/content/path/to/page/jcr:content/or/component");
```

#### 替代方法 {#alternative-approach-1}

使资源适应页面要求资源本身是`cq:Page`节点。

```java
Page page = resource.adaptTo(Page.class);
```

### 读取AEM页面属性

使用页面对象的getter获取已知属性（`getTitle()`、`getDescription()`等）和`page.getProperties()`获取`[cq:Page]/jcr:content`值映射以检索其他属性。

```java
Page page = resource.adaptTo(Page.class);
String title = page.getTitle();
Calendar value = page.getProperties().get("cq:lastModified", Calendar.getInstance());
```

### 读取AEM资源元数据属性

资产API提供了从`[dam:Asset]/jcr:content/metadata`节点读取属性的简便方法。 这不是ValueMap，不支持第二个参数（缺省值和自动类型转换）。

```java
Asset asset = resource.adaptTo(Asset.class);
String title = asset.getMetadataValue("dc:title");
Calendar lastModified = (Calendar) asset.getMetadata("cq:lastModified");
```

### 读取[!DNL Sling] [!DNL Resource]属性 {#read-sling-resource-properties}

当属性存储在AEM API(Page、Asset)无法直接访问的位置（属性或相对资源）时，[!DNL Sling]资源和值映射可用于获取数据。

```java
ValueMap properties = resource.getValueMap();
String value = properties.get("jcr:title", "Default title");
String relativeResourceValue = properties.get("relative/propertyName", "Default value");
```

在这种情况下，可能必须将AEM对象转换为[!DNL Sling] [!DNL Resource]才能有效地找到所需的属性或子资源。

#### AEM页面至[!DNL Sling] [!DNL Resource]

```java
Resource resource = page.adaptTo(Resource.class);
```

#### AEM资源到[!DNL Sling] [!DNL Resource]

```java
Resource resource = asset.adaptTo(Resource.class);
```

### 使用[!DNL Sling]的ModifiableValueMap写入属性

使用[!DNL Sling]的[ModifiableValueMap](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ModifiableValueMap.html)将属性写入节点。 这只能写入直接节点（不支持相对属性路径）。

请注意，对`.adaptTo(ModifiableValueMap.class)`的调用需要对该资源的写入权限，否则将返回null。

```java
ModifiableValueMap properties = resource.adaptTo(ModifiableValueMap.class);

properties.put("newPropertyName", "new value");
properties.put("propertyNameToUpdate", "updated value");
properties.remove("propertyToRemove");

resource.getResourceResolver().commit();
```

### 创建AEM页面

始终使用PageManager在生成页面模板时创建页面，这是在AEM中正确定义和初始化页面所必需的。

```java
String templatePath = "/conf/my-app/settings/wcm/templates/content-page";
boolean autoSave = true;

PageManager pageManager = resourceResolver.adaptTo(PageManager.class);
pageManager.create("/content/parent/path", "my-new-page", templatePath, "My New Page Title", autoSave);

if (!autoSave) { resourceResolver.commit(); }
```

### 创建[!DNL Sling]资源

ResourceResolver支持用于创建资源的基本操作。 在创建更高级别的抽象概念(AEM Pages、Assets、Tags等)时，请使用其各自经理提供的方法。

```java
resourceResolver.create(parentResource, "my-node-name", new ImmutableMap.Builder<String, Object>()
           .put("jcr:primaryType", "nt:unstructured")
           .put("jcr:title", "Hello world")
           .put("propertyName", "Other initial properties")
           .build());

resourceResolver.commit();
```

### 删除[!DNL Sling]资源

ResourceResolver支持删除资源。 在创建更高级别的抽象概念(AEM Pages、Assets、Tags等)时，请使用其各自经理提供的方法。

```java
resourceResolver.delete(resource);

resourceResolver.commit();
```
