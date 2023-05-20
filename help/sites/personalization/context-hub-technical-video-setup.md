---
title: 使用AEM Sites設定ContextHub以進行個人化
description: ContextHub是一種用於儲存、操控和呈現內容資料的架構。 ContextHub Javascript API可讓您存取存放區，以視需要建立、更新和刪除資料。 因此，ContextHub代表頁面上的資料層。 本頁面說明如何將Context Hub新增至AEM網站頁面。
feature: Context Hub
topics: personalization
audience: developer, architect
doc-type: technical video
activity: setup
version: 6.4, 6.5
topic: Personalization
role: Developer
level: Intermediate
exl-id: 89308dd3-a7e5-4fec-bffb-5f0974125c0a
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '424'
ht-degree: 3%

---

# 設定ContextHub以進行個人化 {#set-up-contexthub}

ContextHub是一種用於儲存、操控和呈現內容資料的架構。 ContextHub Javascript API可讓您存取存放區，以視需要建立、更新和刪除資料。 因此，ContextHub代表頁面上的資料層。 本頁面說明如何將Context Hub新增至AEM網站頁面。

>[!VIDEO](https://video.tv.adobe.com/v/23765?quality=12&learn=on)

>[!NOTE]
>
>我們對此影片使用WKND參考網站，它不是AEM版本的一部分。 您可以下載 [此處為最新版本](https://github.com/adobe/aem-guides-wknd/releases).

將ContextHub新增至您的頁面，以啟用ContextHub功能並連結至ContextHub JavaScript資料庫。 ContextHub JavaScript API可讓您存取ContextHub管理的內容資料。

## 將ContextHub新增至頁面元件 {#adding-contexthub-to-a-page-component}

若要啟用ContextHub功能並連結至ContextHub JavaScript程式庫，請包含 `contexthub` 中的元件 `<head>` 區段。 頁面元件的HTL程式碼類似於以下範例：

```java
<!--/* Include Context Hub */-->
<sly data-sly-resource="${'contexthub' @ resourceType='granite/contexthub/components/contexthub'}"/>
```

## 網站設定和ContextHub區段 {#site-configuration-and-contexthub-segments}

ContextHub包含區段引擎，可管理區段並決定針對目前內容解析哪些區段。 已定義數個區段。 您可以使用Javascript API來 [決定已解析的區段](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/ch-adding.html#DeterminingResolvedContextHubSegments). 在底下啟用網站的ContextHub區段 [[!UICONTROL 設定瀏覽器]](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/configurations.html).

## 建立區段 {#create-segments}

建立作為Teaser規則的AEM區段。 也就是說，它們定義了Teaser中的內容何時出現在網頁上。 接著，即可根據訪客的相符區段，針對其需求和興趣特別鎖定內容。

## 指派雲端設定、區段路徑和ContextHub路徑至您的網站 {#assigning-cloud-configuration-segment-path-and-contexthub-path-to-your-site}

將雲端設定路徑、分段路徑和ContextHub路徑指派至您的網站根節點，好讓您可以為對象建立個人化體驗。 使用ContextHub，您可以操作內容資料並測試已解析的區段。

![CRXDE Lite](assets/crx-de-properties.png)

您可以閱讀以下有關ContextHub和區段的更多資訊：

* [ContextHub](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/contexthub.html)
* [將Context Hub新增至頁面並存取存放區](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/ch-adding.html)
* [了解分段](https://helpx.adobe.com/experience-manager/6-5/sites/classic-ui-authoring/using/classic-personalization-campaigns-segmentation.html)
* [使用 ContextHub 配置分段](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/segmentation.html)
