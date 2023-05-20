---
title: 第7章 — 從行動應用程式使用AEM Content Services — 內容服務
description: 教學課程的第7章會執行Android行動應用程式，以使用來自AEM Content Services的編寫內容。
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: d6b6d425-842a-43a9-9041-edf78e51d962
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '1391'
ht-degree: 0%

---

# 第7章 — 從行動應用程式使用AEM內容服務

教學課程的第7章使用原生Android行動應用程式來使用AEM Content Services的內容。

## Android行動應用程式

本教學課程使用 **簡易原生Android行動應用程式** 使用及顯示AEM Content Services公開的事件內容。

使用 [Android](https://developer.android.com/) 這在很大程度上並不重要，而且耗用的行動應用程式可以在任何行動平台的任何框架中撰寫，例如iOS。

Android之所以用於教學課程，是因為其可在Windows、macOs和Linux上執行Android模擬器、其受歡迎程度以及可寫成Java (AEM開發人員完全瞭解的語言)。

*本教學課程的Android行動應用程式為&#x200B;**not**旨在指示如何建置Android行動應用程式或傳達Android開發最佳實務，而非說明如何從行動應用程式使用AEM Content Services。*

### AEM Content Services如何推動行動應用程式體驗

![行動應用程式與內容服務的對應](assets/chapter-7/content-services-mapping.png)

1. 此 **標誌** 如下列專案所定義： [!DNL Events API] 頁面的 **影像元件**.
1. 此 **標籤行** 如 [!DNL Events API] 頁面的 **文字元件**.
1. 此 **事件清單** 衍生自事件內容片段的序列化，透過設定的 **內容片段清單元件**.

## 行動應用程式示範

>[!VIDEO](https://video.tv.adobe.com/v/28345?quality=12&learn=on)

### 設定行動應用程式以供非localhost使用

如果AEM Publish未在上執行 **http://localhost:4503** 主機和連線埠可在行動應用程式的 [!DNL Settings] 指向屬性AEM發佈主機/連線埠。

>[!VIDEO](https://video.tv.adobe.com/v/28344?quality=12&learn=on)

## 在本機執行行動應用程式

1. 下載並安裝 [Android Studio](https://developer.android.com/studio/install) 安裝Android模擬器。
1. **下載** Android [!DNL APK] 檔案 [GitHub >資產> wknd-mobile.x.x.xapk](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
1. 開啟 **Android Studio**
   * Android Studio初次啟動時，系統會提示您安裝 [!DNL Android SDK] 將會出現。 接受預設值並完成安裝。
1. 開啟Android Studio並選取 **設定檔或偵錯APK**
1. 選取APK檔案(**wknd-mobile.x.x.x.apk**)下載，然後按一下 **確定**
   * 如果系統提示 **建立新資料夾**，或 **使用現有**，選取 **使用現有**.
1. 初次啟動Android Studio時，以滑鼠右鍵按一下 **wknd-mobile.x.x.x** ，然後選取「 」 **開啟模組設定**.
   * 下 **模組> wknd-mobile.x.x.x >相依性索引標籤**，選取 **Android API 29平台**. 點選「確定」以關閉並儲存變更。
   * 如果不這麼做，當您嘗試啟動模擬器時，會看到「請選取Android SDK」錯誤。
1. 開啟 **AVD管理員** 藉由選取 **「工具」>「AVD管理員」** 或點選 **AVD管理員** 圖示加以檢視。
1. 在 **AVD管理員** 視窗，按一下 **+建立虛擬裝置……** 如果您尚未註冊裝置。
   1. 在左側，選取 **電話** 類別。
   1. 選取 **畫素2**.
   1. 按一下 **下一個** 按鈕。
   1. 選取 **Q** 替換為 **API Level 29**.
      * AVD Manager初次啟動時，系統會要求您下載已建立版本的API。 按一下「Q」版旁的「下載」連結，完成下載和安裝。
   1. 按一下 **下一個** 按鈕。
   1. 按一下 **完成** 按鈕。
1. 關閉 **AVD管理員** 視窗。
1. 在頂端功能表列中，選取 **wknd-mobile.x.x.x** 從 **執行/編輯設定** 下拉式清單。
1. 點選 **執行** 所選專案旁的按鈕 **執行/編輯設定**
1. 在快顯視窗中，選取新建的 **[!DNL Pixel 2 API 29]** 虛擬裝置和點選 **確定**
1. 如果 [!DNL WKND Mobile] 應用程式未立即載入、尋找並點選 **[!DNL WKND]** 圖示加以檢視（位於模擬器的Android首頁畫面中）。
   * 如果模擬器啟動，但模擬器的畫面仍保持黑色，請點選 **power** 「模擬器」視窗旁的「工具」視窗中的按鈕。
   * 若要在虛擬裝置內捲動，請按住並拖曳。
   * 若要從AEM重新整理內容，請從上方下拉直到「重新整理」圖示顯示，然後釋放。

>[!VIDEO](https://video.tv.adobe.com/v/28341?quality=12&learn=on)

## 行動應用程式程式碼

本節重點說明互動最多、且取決於AEM Content Services及其JSON輸出的Android行動應用程式程式碼。

載入時，行動應用程式會執行 `HTTP GET` 至 `/content/wknd-mobile/en/api/events.model.json` 這是設定為提供內容以驅動行動應用程式的AEM Content Services端點。

因為Events API的可編輯範本(`/content/wknd-mobile/en/api/events.model.json`)已鎖定，行動應用程式可經過編碼，以在JSON回應中的特定位置中尋找特定資訊。

### 高階程式碼流程

1. 開啟 [!DNL WKND Mobile] 應用程式會叫用 `HTTP GET` 請求發佈至AEM Publish `/content/wknd-mobile/en/api/events.model.json` 收集內容以填入行動應用程式的UI。
2. 收到來自AEM的內容後，行動應用程式的三個檢視元素中的每一個都會在 **標誌、標籤行與活動清單**，會以AEM中的內容初始化。
   * 若要將AEM內容繫結至行動應用程式的檢視元素，代表每個AEM元件的JSON是對映至Java POJO的物件，而該Java POJO又繫結至Android檢視元素。
      * 影像元件JSON →標誌POJO →標誌影像檢視
      * 文字元件JSON → TagLine POJO →文字影像檢視
      * 內容片段清單JSON →事件POJO →事件RecyclerView
   * *行動應用程式程式碼可將JSON對應至POJO，因為較大JSON回應中有知名的位置。 請記住，「image」、「text」和「contentfragmentlist」的JSON索引鍵是由後援AEM元件的節點名稱所指定。 如果這些節點名稱變更，行動應用程式將會中斷，因為它不知道如何從JSON資料取得必要內容。*

#### 叫用AEM Content Services端點

以下為行動應用程式程式碼中的 `MainActivity` 負責叫用AEM Content Services來收集推動行動應用程式體驗的內容。

```
protected void onCreate(Bundle savedInstanceState) {
    ...
    // Create the custom objects that will map the JSON -> POJO -> View elements
    final List<ViewBinder> viewBinders = new ArrayList<>();

    viewBinders.add(new LogoViewBinder(this, getAemHost(), (ImageView) findViewById(R.id.logo)));
    viewBinders.add(new TagLineViewBinder(this, (TextView) findViewById(R.id.tagLine)));
    viewBinders.add(new EventsViewBinder(this, getAemHost(), (RecyclerView) findViewById(R.id.eventsList)));
    ...
    initApp(viewBinders);
}

private void initApp(final List<ViewBinder> viewBinders) {
    final String aemUrl = getAemUrl(); // -> http://localhost:4503/content/wknd-mobile/en/api/events.mobile.json
    JsonObjectRequest request = new JsonObjectRequest(aemUrl, null,
        new Response.Listener<JSONObject>() {
            @Override
            public void onResponse(JSONObject response) {
                for (final ViewBinder viewBinder : viewBinders) {
                    viewBinder.bind(response);
                }
            }
        },
        ...
    );
}
```

`onCreate(..)` 是行動應用程式的初始化掛接，並註冊3個自訂 `ViewBinders` 負責剖析JSON並將值繫結至 `View` 元素。

`initApp(...)` 接著會呼叫，以向AEM Publish上的AEM Content Services端點發出HTTPGET請求來收集內容。 收到有效的JSON回應後，JSON回應會傳遞至每個 `ViewBinder` 負責剖析JSON並將其繫結至行動裝置 `View` 元素。

#### 剖析JSON回應

接下來，我們將審視 `LogoViewBinder`，很簡單，但會強調幾個重要考量。

```
public class LogoViewBinder implements ViewBinder {
    ...
    public void bind(JSONObject jsonResponse) throws JSONException, IOException {
        final JSONObject components = jsonResponse.getJSONObject(":items").getJSONObject("root").getJSONObject(":items");

        if (components.has("image")) {
            final Image image = objectMapper.readValue(components.getJSONObject("image").toString(), Image.class);

            final String imageUrl = aemHost + image.getSrc();
            Glide.with(context).load(imageUrl).into(logo);
        } else {
            Log.d("WKND", "Missing Logo");
        }
    }
}
```

第一行 `bind(...)` 透過索引鍵向下導覽JSON回應 **：items → root → ：items** 代表新增元件的AEM配置容器。

從此處，會對名為的金鑰進行檢查 **影像**，代表影像元件(再次強調，此節點名稱→JSON索引鍵穩定非常重要)。 如果此物件存在，則會讀取並對應至 [自訂影像POJO](#image-pojo) 透過Jackson `ObjectMapper` 資料庫。 以下說明影像POJO。

最後，圖志的 `src` 會使用載入到Android ImageView中 [!DNL Glide] 協助程式庫。

請注意，我們必須提供AEM結構描述、主機和連線埠(透過 `aemHost`)至AEM Publish執行個體，因為AEM Content Services只會提供JCR路徑(即 `/content/dam/wknd-mobile/images/wknd-logo.png`)至參考的內容。

#### 影像POJO{#image-pojo}

雖然可選用，但使用 [Jackson ObjectMapper](https://fasterxml.github.io/jackson-databind/javadoc/2.9/com/fasterxml/jackson/databind/ObjectMapper.html) 或由其他程式庫（例如Gson）提供的類似功能，有助於將複雜的JSON結構對應到Java POJO，而不必費心直接處理原生JSON物件本身。 在這個簡單的案例中，我們會將 `src` 索引鍵來自 `image` JSON物件，至 `src` 屬性直接透過 `@JSONProperty` 註解。

```
package com.adobe.aem.guides.wknd.mobile.android.models;

import com.fasterxml.jackson.annotation.JsonProperty;

public class Image {
    @JsonProperty(value = "src")
    private String src;

    public String getSrc() {
        return src;
    }
}
```

事件POJO需要從JSON物件選取更多資料點，比起簡單的影像(我們只想要 `src`.

## 探索行動應用程式體驗

現在您已瞭解AEM Content Services如何推動原生行動體驗，請使用您已學到的知識執行以下步驟，並檢視您的變更已反映在行動應用程式中。

在每個步驟後，提取以重新整理行動應用程式，並驗證行動體驗的更新。

1. 建立並發佈 **新 [!DNL Event] 內容片段**
1. 取消發佈 **現有 [!DNL Event] 內容片段**
1. 將更新發佈至 **標籤行**

## 恭喜

**您已完成AEM Headless教學課程！**

若要進一步瞭解AEM Content Services和AEM as a Headless CMS，請造訪Adobe的其他檔案和啟用資料：

* [使用內容片段](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/content-fragments/understand-content-fragments-and-experience-fragments.html)
* [AEM WCM Core Components使用手冊](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=zh-Hans)
* [AEM WCM核心元件程式庫](https://opensource.adobe.com/aem-core-wcm-components/library.html)
* [AEM WCM核心元件GitHub專案](https://github.com/adobe/aem-core-wcm-components)
* [元件匯出程式的程式碼範例](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/core/src/main/java/com/adobe/acs/samples/models/SampleComponentExporter.java)
