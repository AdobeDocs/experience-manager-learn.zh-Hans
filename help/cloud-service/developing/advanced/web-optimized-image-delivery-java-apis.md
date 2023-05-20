---
title: 網頁最佳化的影像傳送Java&trade； API
description: 瞭解如何使用AEMas a Cloud Service的網頁最佳化影像傳送Java&trade； API來開發高效能的網頁體驗。
version: Cloud Service
feature: APIs, Sling Model, OSGI, HTL or HTML Template Language
topic: Performance, Development
role: Architect, Developer
level: Intermediate
doc-type: Code Sample
last-substantial-update: 2023-03-30T00:00:00Z
jira: KT-13014
thumbnail: KT-13014.jpeg
exl-id: c6bb9d6d-aef0-42d5-a189-f904bbbd7694
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '849'
ht-degree: 2%

---

# 網頁最佳化的影像傳送Java™ API

瞭解如何使用AEMas a Cloud Service的網頁最佳化影像傳送Java™ API來開發高效能的網頁體驗。

AEMas a Cloud Service支援 [網頁最佳化的影像傳遞](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/web-optimized-image-delivery.html) 會自動產生資產的最佳化影像Web轉譯。 網頁最佳化的影像傳送可以使用三種主要方法：

1. [使用AEM核心WCM元件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=zh-Hans)
2. 建立符合以下條件的自訂元件 [擴充AEM核心WCM元件影像元件](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/project-archetype/custom-component.html#tackling-the-image-problem)
3. 建立使用AssetDelivery Java™ API產生網頁最佳化影像URL的自訂元件。

本文探討如何在自訂元件中使用網頁最佳化的影像Java™ API，讓程式碼型可在AEMas a Cloud Service和AEM SDK上運作。

## Java™ API

此 [資產傳送API](https://javadoc.io/doc/com.adobe.aem/aem-sdk-api/latest/com/adobe/cq/wcm/spi/AssetDelivery.html) 是OSGi服務，可為影像資產產生網頁最佳化的傳送URL。 `AssetDelivery.getDeliveryURL(...)` 允許的選項為 [在此處記錄](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/web-optimized-image-delivery.html#can-i-use-web-optimized-image-delivery-with-my-own-component%3F).

此 `AssetDelivery` 只有在AEMas a Cloud Service中執行時，才符合OSGi服務。 在AEM SDK上，參考至 `AssetDelivery` OSGi服務返回 `null`. 在AEMas a Cloud Service上執行時，最好有條件地使用網頁最佳化的URL，並在AEM SDK上使用後援影像URL。 通常資產的網頁轉譯便是一個足夠的備援。


### OSGi服務中使用的API

標籤`AssetDelivery` 作為自訂OSGi服務中的選用參考，以便自訂OSGi服務在AEM SDK上仍然可用。

```java
import com.adobe.cq.wcm.spi.AssetDelivery;
...
@Reference(cardinality = ReferenceCardinality.OPTIONAL)
private volatile AssetDelivery assetDelivery;
```

### Sling模型中的API使用

標籤`AssetDelivery` 在自訂Sling模型中參考為選用，以便自訂Sling模型在AEM SDK上仍然可用。

```java
import com.adobe.cq.wcm.spi.AssetDelivery;
...
@OSGiService(injectionStrategy = InjectionStrategy.OPTIONAL)
private AssetDelivery assetDelivery;
```

### 有條件地使用API

有條件地傳回網頁最佳化的影像URL或備援URL，根據 `AssetDelivery` OSGi服務的可用性。 條件式使用可讓程式碼在AEM SDK上執行程式碼時運作。

```java
if (assetDelivery != null ) {
    // When running on AEM as a Cloud Service use the real web-optimized image URL.
    return assetDelivery.getDeliveryURL(...);
} else {
    // When running on AEM SDK, use some fallback image so the experience does not break.
    // What the fallback is up to you! 
    return getFallbackURL(...);
}
```

## 範常式式碼

下列程式碼會建立範例元件，使用Web最佳化的影像URL顯示影像資產清單。

當程式碼在AEMas a Cloud Service上執行時，會在自訂元件中使用網頁最佳化的Web影像轉譯。

![AEMas a Cloud Service上網頁最佳化的影像](./assets/web-optimized-image-delivery-java-apis/cloud-service.png)

_AEMas a Cloud Service支援AssetDelivery API，因此會使用網頁最佳化的Web轉譯_

程式碼在AEM SDK上執行時，會使用較不理想的靜態Web轉譯，讓元件在本機開發期間正常運作。

![AEM SDK上的Web最佳化後援影像](./assets/web-optimized-image-delivery-java-apis/aem-sdk.png)

_AEM SDK不支援AssetDelivery API，因此使用備援靜態網頁轉譯(PNG或JPEG)_

實作分為三個邏輯部分：

1. 此 `WebOptimizedImage` OSGi服務會成為AEM所提供之「智慧代理」 `AssetDelivery` 可處理AEMas a Cloud Service和AEM SDK中執行的OSGi服務。
2. 此 `ExampleWebOptimizedImages` Sling Model提供收集影像資產清單及其要顯示的網頁最佳化URL的商業邏輯。
3. 此 `example-web-optimized-images` AEM元件實作HTL以顯示Web最佳化影像清單。

下列範常式式碼可在程式碼庫中複製，並視需要更新。

### OSGi服務

此 `WebOptimizedImage` OSGi服務分割成可定址的公用介面(`WebOptimizedImage`)和內部實作(`WebOptimizedImageImpl`)。 此 `WebOptimizedImageImpl` 在AEMas a Cloud Service上執行時傳回Web最佳化的影像URL，並在AEM SDK上傳回靜態Web轉譯URL，讓元件在AEM SDK上維持正常運作。

#### 接口

介面會定義OSGi服務合約，其他程式碼（例如Sling模型）可與之互動。

```java
package com.adobe.aem.guides.wknd.core.images;

import org.apache.sling.api.resource.ResourceResolver;
import org.osgi.annotation.versioning.ProviderType;

import java.util.Map;

/**
 * OSGi Service that acts as a facade for the AssetDelivery API, such that a fallback can be automatically served on the AEM SDK.
 *
 * This service can be extended to provide additional functionality, such as srcsets, etc.
 */
@ProviderType
public interface WebOptimizedImage {
    /**
     * Returns the Web Optimized Image URL.
     * @param resourceResolver the user's resource resolver
     * @param path the path to the asset
     * @param options the options to pass to the AssetDelivery API
     * @return the Web Optimized Image URL
     */
    String getDeliveryURL(ResourceResolver resourceResolver, String path, Map<String, Object> options);
}
```

#### 实施

OSGi Service實作包含AEM的選用參考 `AssetDelivery` OSGi服務，以及後援邏輯，用於在下列情況下選取合適的影像URL： `AssetDelivery` 是 `null` 在AEM SDK上。 可以根據需求更新遞補邏輯。

```java
package com.adobe.aem.guides.wknd.core.images.impl;

import com.adobe.aem.guides.wknd.core.images.WebOptimizedImage;
import com.adobe.cq.wcm.spi.AssetDelivery;
import com.day.cq.dam.api.Asset;
import com.day.cq.dam.api.Rendition;
import com.day.cq.dam.api.RenditionPicker;
import com.day.cq.dam.commons.util.DamUtil;
import org.apache.commons.lang3.StringUtils;
import org.apache.sling.api.resource.Resource;
import org.apache.sling.api.resource.ResourceResolver;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.osgi.service.component.annotations.ReferenceCardinality;

import java.util.Map;
@Component
public class WebOptimizedImageImpl implements WebOptimizedImage {
    private static final String DEFAULT_FORMAT = "webp";
    @Reference(cardinality = ReferenceCardinality.OPTIONAL)
    private volatile AssetDelivery assetDelivery;

    /**
     * Returns the Web Optimized Image URL.
     * @param resourceResolver the user's resource resolver
     * @param path the path to the asset
     * @param options the options to pass to the AssetDelivery API
     * @return the Web Optimized Image URL
     */
    @Override
    public String getDeliveryURL(ResourceResolver resourceResolver, String path, Map<String, Object> options) {
        if (assetDelivery != null) {
            return getWebOptimizedUrl(resourceResolver, path, options);
        } else {
            return getFallbackUrl(resourceResolver, path);
        }
    }
    /**
     * Uses the AssetDelivery API to get the Web Optimized Image URL.
     * @param resourceResolver the user's resource resolver
     * @param path the path to the asset
     * @param options the options to pass to the AssetDelivery API
     * @return the Web Optimized Image URL
     */
    private String getWebOptimizedUrl(ResourceResolver resourceResolver, String path, Map<String, Object> options) {
        Resource resource = resourceResolver.getResource(path);
        Asset asset = DamUtil.resolveToAsset(resource);

        // These 3 options are required for the AssetDelivery API to work, else it will return null
        options.put("path", asset.getPath());
        options.put("format", StringUtils.defaultString((String) options.get("format"), DEFAULT_FORMAT));
        options.put("seoname", StringUtils.defaultString((String) options.get("seoname"), asset.getName()));

        // The resource only provides the security context into AEM so the asset's UUID can be looked up for the Web Optimized Image URL
        return assetDelivery.getDeliveryURL(resource, options);
    }

    /**
     * Fallback to the static web rendition if the AssetDelivery API is not available, meaning the code is running on the AEM SDK.
     * @param resourceResolver the user's resource resolver
     * @param path the path to the asset
     * @return the path to the web rendition
     */
    private String getFallbackUrl(ResourceResolver resourceResolver, String path) {
        Resource resource = resourceResolver.getResource(path);
        Asset asset = DamUtil.resolveToAsset(resource);

        return asset.getRendition(WebRenditionPicker).getPath();
    }

    /**
     * Picks the web rendition of the asset.
     */
    private static final RenditionPicker WebRenditionPicker = new RenditionPicker() {
        @Override
        public Rendition getRendition(Asset asset) {
            return asset.getRenditions().stream().filter(rendition -> StringUtils.startsWith(rendition.getName(), "cq5dam.web.")).findFirst().orElse(asset.getOriginal());
        }
    };
}
```

### Sling模型

此 `ExampleWebOptimizedImages` Sling模型會分割成可定址的公用介面(`ExampleWebOptimizedImages`)和內部實作(`ExampleWebOptimizedImagesImpl`)；

此 `ExampleWebOptimizedImagesImpl` Sling模型會收集要顯示的影像資產清單，並叫用自訂 `WebOptimizedImage` OSGi服務，用於取得Web最佳化的影像URL。 由於此Sling模型代表AEM元件，因此有以下常用方法 `isEmpty()`， `getId()`、和 `getData()` 不過，這些方法與使用Web最佳化的影像並不直接相關。

#### 接口

介面會定義Sling模型合約，其他程式碼（例如HTL）可以與之互動。

```java
package com.adobe.aem.guides.wknd.core.models;

import com.adobe.cq.wcm.core.components.models.datalayer.ComponentData;
import com.fasterxml.jackson.annotation.JsonProperty;

import java.util.List;

public interface ExampleWebOptimizedImages {

    /**
     * @return a list of web optimized images for the component to display. Each item in the list has necessary information to render the image.
     */
    List<Img> getImages();

    /**
     * @return true if this component has no images to display.
     */
    boolean isEmpty();

    /**
     * @return String representing the unique identifier of the ExampleWebOptimizedImages component on a page
     */
    String getId();

    /**
     * @return JSON data to populate the data layer
     */
    @JsonProperty("dataLayer")
    default ComponentData getData() {
        return null;
    }

    /**
     * Describes a web optimized image.
     */
    interface Img {
        /**
         * @return the URL to the web optimized rendition of the image.
         */
        String getSrc();

        /**
         * @return the alt text of the web optimized image.
         */
        String getAlt();

        /**
         * @return the height of the web optimized image.
         */
        String getHeight();
        /**
         * @return the width of the web optimized image.
         */
        String getWidth();
    }
}
```

#### 实施

Sling模型使用自訂 `WebOptimizeImage` OSGi服務，收集其元件顯示之影像資產的Web最佳化影像URL。

在此範例中，使用簡單查詢來收集影像資產。

```java
package com.adobe.aem.guides.wknd.core.models.impl;

import com.adobe.aem.guides.wknd.core.images.WebOptimizedImage;
import com.adobe.aem.guides.wknd.core.models.ExampleWebOptimizedImages;
import com.adobe.cq.wcm.core.components.models.datalayer.ComponentData;
import com.adobe.cq.wcm.core.components.models.datalayer.builder.DataLayerBuilder;
import com.adobe.cq.wcm.core.components.util.ComponentUtils;
import com.day.cq.dam.api.Asset;
import com.day.cq.dam.commons.util.DamUtil;
import com.day.cq.wcm.api.Page;
import com.day.cq.wcm.api.components.ComponentContext;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.resource.Resource;
import org.apache.sling.models.annotations.DefaultInjectionStrategy;
import org.apache.sling.models.annotations.Model;
import org.apache.sling.models.annotations.Required;
import org.apache.sling.models.annotations.injectorspecific.OSGiService;
import org.apache.sling.models.annotations.injectorspecific.ScriptVariable;
import org.apache.sling.models.annotations.injectorspecific.Self;

import java.util.*;

@Model(
        adaptables = {SlingHttpServletRequest.class},
        adapters = {ExampleWebOptimizedImages.class},
        resourceType = {ExampleWebOptimizedImagesImpl.RESOURCE_TYPE},
        defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL
)
public class ExampleWebOptimizedImagesImpl implements ExampleWebOptimizedImages {

    protected static final String RESOURCE_TYPE = "wknd/components/example-web-optimized-images";

    private static final int MAX_RESULTS = 10;

    @Self
    @Required
    private SlingHttpServletRequest request;

    @OSGiService
    private WebOptimizedImage webOptimizedImage;

    @ScriptVariable
    private Page currentPage;

    @ScriptVariable
    protected ComponentContext componentContext;

    private List<Img> images;

    // XPath query to find image assets to display
    private static final String XPATH_QUERY = "/jcr:root/content/dam/wknd-shared/en/adventures//element(*, dam:Asset) [ (jcr:contains(jcr:content/metadata/@dc:format, 'image/')) ]";
    @Override
    public List<Img> getImages() {

        if (images == null) {
            images = new ArrayList<>();

            // Set the AssetDelivery options to request a web-optimized rendition.
            // These options can be set as required by the implementation (Dialog, pass in from HTL via @RequestAttribute)
            final Map<String, Object> options = new HashMap<>();
            options.put("format", "webp");
            options.put("preferwebp", "true");
            options.put("width", "350");
            options.put("height", "350");

            final Iterator<Resource> results = request.getResourceResolver().findResources(XPATH_QUERY, "xpath");

            while (results.hasNext() && images.size() < MAX_RESULTS) {
                Resource resource = results.next();
                Asset asset = DamUtil.resolveToAsset(resource);

                // Get the image URL; the web-optimized rendition on AEM as a Cloud Service, or the static web rendition fallback on AEM SDK
                final String url = webOptimizedImage.getDeliveryURL(request.getResourceResolver(), resource.getPath(), options);

                // Add the image to the list that is passed to the HTL component to display
                // We'll add some extra attributes so that the HTL can display the image in a performant, SEO-friendly, and accessible way
                // ImgImpl can be extended to add additional attributes, such as srcset, etc.
                images.add(new ImgImpl(url, asset.getName(), (String) options.get("height"), (String) options.get("width")));
            }
        }

        return this.images;
    }

    @Override
    public boolean isEmpty() {
        return getImages().isEmpty();
    }

    @Override
    public String getId() {
        return ComponentUtils.getId(request.getResource(), currentPage, componentContext);
    }

    @Override
    public ComponentData getData() {
        if (ComponentUtils.isDataLayerEnabled(request.getResource())) {
            return DataLayerBuilder.forComponent()
                    .withId(() -> getId())
                    .withType(() -> RESOURCE_TYPE)
                    .build();
        }
        return null;
    }

    class ImgImpl implements Img {
        private final String url;
        private final String alt;
        private final int height;
        private final int width;

        public ImgImpl(String url, String alt, String height, String width) {
            this.url = url;
            this.alt = alt;
            this.height = Integer.parseInt(height);
            this.width = Integer.parseInt(width);
        }

        @Override
        public String getSrc() {
            return url;
        }

        @Override
        public String getAlt() {
            return alt;
        }

        @Override
        public String getHeight() {
            return height + "px";
        }

        @Override
        public String getWidth() {
            return width + "px";
        }
    }
}
```

### AEM元件

AEM元件繫結至的Sling資源型別 `WebOptimizedImagesImpl` Sling模型實作，並負責顯示影像清單。



元件會收到 `Img` 物件，透過 `getImages()` 包括在AEMas a Cloud Service上執行時最佳化的WEBP影像。 元件會收到 `Img` 物件，透過 `getImages()` 包括在AEM SDK上執行時的靜態PNG/JPEG網頁影像。

#### HTL

HTL使用 `WebOptimizedImages` Sling模型，並呈現  `Img` 物件傳回者 `getImages()`.

```html
<style>
    .cmp-example-web-optimized-images__list {
        width: 100%;
        list-style: none;
        padding: 0;
        display: flex;
        flex-wrap: wrap;
        justify-content: space-between;
        gap: 2rem;
    }

    .cmp-example-web-optimized-images-list__item {
        margin: 0;
        padding: 0;
    }
</style>

<div data-sly-use.exampleImages="com.adobe.aem.guides.wknd.core.models.ExampleWebOptimizedImages"
     data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
     data-sly-test.hasContent="${!exampleImages.empty}"
     data-cmp-data-layer="${exampleImages.data.json}">

    <h3>Example web-optimized images</h3>

    <ul class="cmp-example-web-optimized-images__list"
        data-sly-list.item="${exampleImages.images}">
        <li class="cmp-example-web-optimized-images-list__item">
            <img class="cmp-example-web-optimized-images__image"
                 src="${item.src}"
                 alt="${item.alt}"
                 width="${item.width}"/>
        </li>
    </ul>
</div>
<sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=!hasContent, classAppend='cmp-example-web-optimized-images'}"></sly>
```
