---
title: AEM中的Java&trade； API最佳作法
description: AEM是以豐富的開放原始碼軟體棧疊為基礎，在開發期間會公開許多Java&trade； API。 本文章探索主要API以及應何時及為何使用。
version: 6.4, 6.5
feature: APIs
topic: Development
role: Developer
level: Beginner
exl-id: b613aa65-f64b-4851-a2af-52e28271ce88
last-substantial-update: 2022-06-24T00:00:00Z
thumbnail: aem-java-bp.jpg
source-git-commit: a156877ff4439ad21fb79f231d273b8983924199
workflow-type: tm+mt
source-wordcount: '2079'
ht-degree: 2%

---

# Java™ API最佳作法

Adobe Experience Manager (AEM)是以豐富的開放原始碼軟體棧疊為基礎，在開發期間會公開許多Java™ API以供使用。 本文章探索主要API以及應何時及為何使用。

AEM建置在四個主要Java™ API集上。

* **Adobe Experience Manager (AEM)**

   * 產品抽象概念，例如頁面、資產、工作流程等。

* **Apache Sling Web框架**

   * REST和以資源為基礎的抽象化，例如資源、值對映和HTTP請求。

* **JCR (Apache Jackrabbit Oak)**

   * 節點、屬性和工作階段等資料和內容抽象概念。

* **OSGi (Apache Felix)**

   * OSGi應用程式容器抽象概念，例如服務和(OSGi)元件。

## Java™ API偏好設定「經驗法則」

一般規則是依照下列順序偏好API/抽象化：

1. **AEM**
1. **Sling**
1. **JCR**
1. **osgi**

如果API是由AEM提供，則偏好使用 [!DNL Sling]、 JCR和OSGi。 如果AEM不提供API，則偏好使用 [!DNL Sling] 超過JCR和OSGi。

此順序為一般規則，表示存在例外。 可以接受脫離此規則的原因包括：

* 眾所周知的例外狀況，如下所述。
* 較高層級的API中沒有所需的功能。
* 在現有程式碼(自訂或AEM產品程式碼)的上下文中操作，而現有程式碼本身使用較不偏好的API，且移至新API的成本不合理。

   * 持續使用較低層級的API比建立混合更好。

## AEM API

* [**AEM API JavaDocs**](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/index.html)

AEM API提供產品化使用案例專用的抽象和功能。

例如，AEM [PageManager](https://developer.adobe.com/experience-manager/reference-materials/cloud-service/javadoc/com/day/cq/wcm/api/PageManager.html) 和 [頁面](https://developer.adobe.com/experience-manager/reference-materials/cloud-service/javadoc/com/day/cq/wcm/api/Page.html) API提供以下專案的抽象概念： `cq:Page` AEM中代表網頁的節點。

雖然這些節點可以透過 [!DNL Sling] API作為資源，JCR API作為節點，AEM API提供常見使用案例的抽象概念。 使用AEM API可確保AEM產品與AEM的自訂和擴充功能之間的一致行為。

### com.adobe.&#42; vs com.day.&#42; API

AEM API具有套件內偏好設定，依偏好設定順序由下列Java™套件識別：

1. `com.adobe.cq`
1. `com.adobe.granite`
1. `com.day.cq`

此 `com.adobe.cq` 套件支援產品使用案例，但 `com.adobe.granite` 支援跨產品平台使用案例，例如工作流程或任務(用於跨產品：AEM Assets、Sites等)。

此 `com.day.cq` 套件包含「原始」API。 這些API解決了Adobe收購之前和/或前後存在的核心抽象和功能 [!DNL Day CQ]. 這些API受到支援，應避免使用，除非 `com.adobe.cq` 或 `com.adobe.granite` 套件不提供（較新的）替代方案。

新的抽象概念，例如 [!DNL Content Fragments] 和 [!DNL Experience Fragments] 內建於 `com.adobe.cq` 空格而非 `com.day.cq` 如下所述。

### 查詢API

AEM支援多種查詢語言。 三種主要語言為 [JCR-SQL2](https://docs.jboss.org/jbossdna/0.7/manuals/reference/html/jcr-query-and-search.html)、 XPath和 [AEM Query Builder](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/query-builder/querybuilder-api.html).

最重要的考量是在程式碼庫中維持一致的查詢語言，以降低複雜性和理解成本。

所有查詢語言實際上都有相同的效能設定檔，例如 [!DNL Apache Oak] 將它們轉換到JCR-SQL2以便執行最終的查詢，並且與JCR-SQL2的查詢時間本身相比，轉換時間可以忽略不計。

偏好的API為 [AEM Query Builder](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/query-builder/querybuilder-api.html)，這是最高級別的抽象化，提供強大的API來建構、執行和擷取查詢的結果，並提供以下內容：

* 簡單、引數化的查詢建構（以Map模型化的查詢引數）
* 原生 [Java™ API和HTTP API](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions.html?lang=zh-Hans)
* [AEM查詢偵錯工具](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/query-builder/querybuilder-api.html)
* [AEM述詞](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/query-builder/querybuilder-predicate-reference.html) 支援常見查詢需求

* 可延伸的API，允許開發自訂 [查詢述詞](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions.html?lang=zh-Hans)
* JCR-SQL2和XPath可直接透過以下方式執行： [[!DNL Sling]](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html#findResources-java.lang.String-java.lang.String-) 和 [JCR API](https://developer.adobe.com/experience-manager/reference-materials/spec/javax.jcr/javadocs/jcr-2.0/index.html)，傳回結果 [[!DNL Sling] 資源](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/Resource.html) 或 [JCR節點](https://developer.adobe.com/experience-manager/reference-materials/spec/javax.jcr/javadocs/jcr-2.0/javax/jcr/Node.html)（分別）。

>[!CAUTION]
>
>AEM QueryBuilder API洩漏ResourceResolver物件。 若要減少此洩漏，請遵循以下步驟 [程式碼範例](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/core/src/main/java/com/adobe/acs/samples/search/querybuilder/impl/SampleQueryBuilder.java#L164).

## [!DNL Sling] API

* [**Apache [!DNL Sling] API JavaDocs**](https://sling.apache.org/apidocs/sling10/)

[Apache [!DNL Sling]](https://sling.apache.org/) 是支援AEM的RESTful Web架構。 [!DNL Sling] 提供HTTP請求路由、將JCR節點建模為資源、提供安全性上下文等等。

[!DNL Sling] API具有專為擴充功能建置的額外優點，這表示通常更容易、更安全地擴充使用建置的應用程式行為 [!DNL Sling] API比可擴充性較低的JCR API還多。

### 的常見用法 [!DNL Sling] API

* 存取JCR節點的方式 [[!DNL Sling Resources]](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/Resource.html) 並透過存取其資料 [ValueMap](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ValueMap.html).

* 透過提供安全性內容 [ResourceResolver](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html).
* 透過ResourceResolver建立和移除資源 [建立/移動/複製/刪除方法](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html).
* 透過更新屬性 [ModifiableValueMap](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ModifiableValueMap.html).
* 建置請求處理建置區塊

   * [Servlet](https://sling.apache.org/documentation/the-sling-engine/servlets.html)
   * [Servlet篩選器](https://sling.apache.org/documentation/the-sling-engine/filters.html)

* 非同步處理工作建置區塊

   * [事件與工作處理常式](https://sling.apache.org/documentation/bundles/apache-sling-eventing-and-job-handling.html)
   * [排程器](https://sling.apache.org/documentation/bundles/scheduler-service-commons-scheduler.html)
   * [Sling 模型](https://sling.apache.org/documentation/bundles/models.html)

* [服務使用者](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security-service-users.html)

## JCR API

* **[JCR 2.0 JavaDocs](https://developer.adobe.com/experience-manager/reference-materials/spec/javax.jcr/javadocs/jcr-2.0/index.html)**

此 [JCR (Java™ Content Repository) 2.0 API](https://developer.adobe.com/experience-manager/reference-materials/spec/javax.jcr/javadocs/jcr-2.0/index.html) 是JCR實施規格的一部分(在AEM的情況下， [Apache Jackrabbit Oak](https://jackrabbit.apache.org/oak/docs/))。 所有JCR實作都必須符合併實作這些API，因此，是與AEM內容互動的最低層級API。

JCR本身是階層式/樹狀結構的NoSQL資料存放區，AEM會將其用作內容存放庫。 JCR具有大量受支援的API，從內容CRUD到查詢內容。 雖然有這個強大的API，但很少比更高層級的AEM和 [!DNL Sling] 抽象化。

與Apache Jackrabbit Oak API相比，總是偏好JCR API。 JCR API用於 ***互動*** JCR存放庫，而Oak API則用於 ***實作*** jcr存放庫。

### JCR API的常見誤解

雖然JCR是AEM內容存放庫，但其API並非與內容互動的偏好方法。 相反地，他們偏好AEM API （Page、Assets、Tag等）或Sling Resource API，因為它們可提供更好的抽象概念。

>[!CAUTION]
>
>在AEM應用程式中，廣泛使用JCR API的工作階段和節點介面會導致程式碼異味。 確定 [!DNL Sling] 應該改用API。

### JCR API的常見用法

* [存取控制管理](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security-service-users.html)
* [可授權管理（使用者/群組）](https://jackrabbit.apache.org/api/2.12/org/apache/jackrabbit/api/security/user/package-summary.html)
* JCR觀察（接聽JCR事件）
* 建立深層節點結構

   * 雖然Sling API支援建立資源，但JCR API在中提供便利的方法 [JcrUtils](https://jackrabbit.apache.org/api/2.12/org/apache/jackrabbit/commons/JcrUtils.html) 和 [JcrUtil](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/day/cq/commons/jcr/JcrUtil.html) 可加快建立深層結構。

## OSGi API

* [**OSGi R6 JavaDocs**](https://docs.osgi.org/javadoc/r6/cmpn/index.html?overview-summary.html)
* **[OSGi Declarative Services 1.2元件註解JavaDocs](https://docs.osgi.org/javadoc/r6/cmpn/org/osgi/service/component/annotations/package-summary.html)**
* **[OSGi Declarative Services 1.2中繼型別註解JavaDocs](https://docs.osgi.org/javadoc/r6/cmpn/org/osgi/service/metatype/annotations/package-summary.html)**
* [**OSGi框架JavaDocs**](https://docs.osgi.org/javadoc/r6/core/org/osgi/framework/package-summary.html)

OSGi API和較高層級API (AEM， [!DNL Sling]和JCR)，很少需要使用OSGi API，且需要高水準的AEM開發專業知識。

### OSGi與Apache Felix API的比較

OSGi定義所有OSGi容器都必須實作並遵循的規格。 AEM OSGi實作Apache Felix也提供自己的幾個API。

* 偏好使用OSGi API (`org.osgi`) over Apache Felix API (`org.apache.felix`)。

### OSGi API的常見用法

* 宣告OSGi服務和元件的OSGi註解。

   * 偏好 [OSGi Declarative Services (DS) 1.2附註](https://docs.osgi.org/javadoc/r6/cmpn/org/osgi/service/component/annotations/package-summary.html) 超過 [Felix SCR註解](https://felix.apache.org/documentation/subprojects/apache-felix-maven-scr-plugin/scr-annotations.html) 用於宣告OSGi服務和元件

* 適用於動態程式碼的OSGi API [取消/註冊OSGi服務/元件](https://docs.osgi.org/javadoc/r6/core/org/osgi/framework/package-summary.html).

   * 當不需要條件式OSGi服務/元件管理（最常見的情況）時，偏好使用OSGi DS 1.2註解。

## 規則的例外

下列為上述定義規則的常見例外。

### OSGi API

處理低階OSGi抽象化時（例如在OSGi元件屬性中定義或讀取），較新的抽象化提供自 `org.osgi` 優先於較高層級的Sling抽象概念。 競爭的Sling抽象尚未標籤為 `@Deprecated` 並提出建議 `org.osgi` 替代方案。

另請注意OSGi設定節點定義偏好設定 `cfg.json` 超過 `sling:OsgiConfig` 格式。

### AEM資產API

* 偏好 [ `com.day.cq.dam.api`](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/day/cq/dam/api/package-summary.html) 超過 [ `com.adobe.granite.asset.api`](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/adobe/granite/asset/api/package-summary.html).

   * 而 `com.day.cq` Assets API為AEM資產管理使用案例提供更免費的工具。
   * Granite Assets API支援低階資產管理使用案例（版本、關係）。

### 查詢API

* AEM QueryBuilder不支援某些查詢函式，例如 [建議](https://jackrabbit.apache.org/oak/docs/query/query-engine.html#Suggestions)、拼字檢查，以及索引提示等不常見函式。 若要使用這些函式進行查詢，建議使用JCR-SQL2。

### [!DNL Sling] Servlet註冊 {#sling-servlet-registration}

* [!DNL Sling] servlet註冊，首選 [OSGi DS 1.2註解(含@SlingServletResourceTypes)](https://sling.apache.org/documentation/the-sling-engine/servlets.html) 超過 `@SlingServlet`

### [!DNL Sling] 篩選器註冊 {#sling-filter-registration}

* [!DNL Sling] 篩選器註冊，首選 [OSGi DS 1.2註解(含@SlingServletFilter)](https://sling.apache.org/documentation/the-sling-engine/filters.html) 超過 `@SlingFilter`

## 有用的程式碼片段

以下是有用的Java™程式碼片段，說明使用已討論API的常見使用案例的最佳實務。 這些片段也說明如何從較不偏好的API移至較偏好的API。

### JCR工作階段至 [!DNL Sling] ResourceResolver

#### 自動關閉Sling ResourceResolver

自AEM 6.2起， [!DNL Sling] ResourceResolver `AutoClosable` 在 [試用資源](https://docs.oracle.com/javase/tutorial/essential/exceptions/tryResourceClose.html) 陳述式。 使用此語法，明確呼叫 `resourceResolver .close()` 不需要。

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

#### 手動關閉Sling ResourceResolver

ResourceResolvers必須在以下位置手動關閉： `finally` 如果無法使用上述自動關閉技術，則會封鎖。

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

### JCR路徑： [!DNL Sling] [!DNL Resource]

```java
Resource resource = ResourceResolver.getResource("/path/to/the/resource");
```

### JCR節點至 [!DNL Sling] [!DNL Resource]

```java
Resource resource = resourceResolver.getResource(node.getPath());
```

### [!DNL Sling] [!DNL Resource] 至AEM資產

#### 建議做法

此 `DamUtil.resolveToAsset(..)` 函式解析下的任何資源 `dam:Asset` 視需要向上瀏覽樹狀結構，以移至Asset物件。

```java
Asset asset = DamUtil.resolveToAsset(resource);
```

#### 替代方法

將資源調整為適合資產需要資源本身 `dam:Asset` 節點。

```java
Asset asset = resource.adaptTo(Asset.class);
```

### [!DNL Sling] AEM資源頁面

#### 建議做法

`pageManager.getContainingPage(..)` 解析下的任何資源 `cq:Page` 視需要向上瀏覽樹狀結構來移至Page物件。

```java
PageManager pageManager = resourceResolver.adaptTo(PageManager.class);
Page page = pageManager.getContainingPage(resource);
Page page2 = pageManager.getContainingPage("/content/path/to/page/jcr:content/or/component");
```

#### 替代方法 {#alternative-approach-1}

將資源調整為符合頁面需要資源本身 `cq:Page` 節點。

```java
Page page = resource.adaptTo(Page.class);
```

### 讀取AEM頁面屬性

使用Page物件的getter來取得已知屬性(`getTitle()`， `getDescription()`、等等)和 `page.getProperties()` 以取得 `[cq:Page]/jcr:content` 用於擷取其他屬性的ValueMap。

```java
Page page = resource.adaptTo(Page.class);
String title = page.getTitle();
Calendar value = page.getProperties().get("cq:lastModified", Calendar.getInstance());
```

### 讀取AEM資產中繼資料屬性

Asset API提供便利的方法，讓您從 `[dam:Asset]/jcr:content/metadata` 節點。 這不是ValueMap，不支援第二個引數（預設值和自動型別轉換）。

```java
Asset asset = resource.adaptTo(Asset.class);
String title = asset.getMetadataValue("dc:title");
Calendar lastModified = (Calendar) asset.getMetadata("cq:lastModified");
```

### 讀取 [!DNL Sling] [!DNL Resource] 屬性 {#read-sling-resource-properties}

當屬性儲存在AEM API （頁面、資產）無法直接存取的位置（屬性或相關資源）時， [!DNL Sling] Resources和ValueMaps可用來取得資料。

```java
ValueMap properties = resource.getValueMap();
String value = properties.get("jcr:title", "Default title");
String relativeResourceValue = properties.get("relative/propertyName", "Default value");
```

在此情況下，可能必須將AEM物件轉換為 [!DNL Sling] [!DNL Resource] 以有效找到所需的屬性或子資源。

#### AEM頁面目標 [!DNL Sling] [!DNL Resource]

```java
Resource resource = page.adaptTo(Resource.class);
```

#### AEM資產目標 [!DNL Sling] [!DNL Resource]

```java
Resource resource = asset.adaptTo(Resource.class);
```

### 寫入屬性，使用 [!DNL Sling]的ModifiableValueMap

使用 [!DNL Sling]的 [ModifiableValueMap](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ModifiableValueMap.html) 將屬性寫入節點。 這只能寫入至直接節點（不支援相對屬性路徑）。

記下對的呼叫 `.adaptTo(ModifiableValueMap.class)` 需要資源的寫入許可權，否則會傳回null。

```java
ModifiableValueMap properties = resource.adaptTo(ModifiableValueMap.class);

properties.put("newPropertyName", "new value");
properties.put("propertyNameToUpdate", "updated value");
properties.remove("propertyToRemove");

resource.getResourceResolver().commit();
```

### 建立AEM頁面

您必須一律使用PageManager建立頁面，因為需要頁面範本，才能在AEM中正確定義和初始化頁面。

```java
String templatePath = "/conf/my-app/settings/wcm/templates/content-page";
boolean autoSave = true;

PageManager pageManager = resourceResolver.adaptTo(PageManager.class);
pageManager.create("/content/parent/path", "my-new-page", templatePath, "My New Page Title", autoSave);

if (!autoSave) { resourceResolver.commit(); }
```

### 建立 [!DNL Sling] 資源

ResourceResolver支援建立資源的基本作業。 建立較高層級的抽象概念(AEM Pages、Assets、Tags等)時，請使用其各自管理員所提供的方法。

```java
resourceResolver.create(parentResource, "my-node-name", new ImmutableMap.Builder<String, Object>()
           .put("jcr:primaryType", "nt:unstructured")
           .put("jcr:title", "Hello world")
           .put("propertyName", "Other initial properties")
           .build());

resourceResolver.commit();
```

### 刪除 [!DNL Sling] 資源

ResourceResolver支援移除資源。 建立較高層級的抽象概念(AEM Pages、Assets、Tags等)時，請使用其各自管理員所提供的方法。

```java
resourceResolver.delete(resource);

resourceResolver.commit();
```
