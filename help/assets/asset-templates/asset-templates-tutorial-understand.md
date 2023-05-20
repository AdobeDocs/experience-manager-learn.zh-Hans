---
title: 瞭解AEM Assets中的InDesign檔案和資產範本
description: 本影片教學課程會逐步解說如何定義AEM Assets資產範本功能中使用的InDesign檔案，以及隨附的所有考量事項。
version: 6.4, 6.5
topic: Content Management
role: User
level: Intermediate
exl-id: c418e94a-b18e-429a-b41c-2bf32e158598
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '507'
ht-degree: 1%

---

# 瞭解AEM Assets中的InDesign檔案和資產範本 {#understanding-indesign-files-and-asset-templates-in-aem-assets}

本影片教學課程會逐步解說如何定義AEM Assets資產範本功能中使用的InDesign檔案，以及隨附的所有考量事項。

## 建構InDesign範本檔案 {#constructing-the-indesign-template-file}

>[!VIDEO](https://video.tv.adobe.com/v/19293?quality=12&learn=on)

1. 下載並開啟 [**InDesign檔案範本**](assets/asset-templates-tutorial-video--supporting-files.zip)
2. **開啟「標籤」面板，** 請檢閱標籤命名慣例，並注意InDesign檔案中的可作者元素已加上標籤。 請記住，在AEM中只能編輯已標籤的元素。

   * **「視窗>公用程式>標籤」**

3. 在頁面上，新增文字元素、提供文字「Header」並套用 **標題** 段落樣式。

   * **視窗>樣式>段落樣式**

   然後，建立並套用名為的新標籤 **Page2Heading。**

4. 新增FPO標誌影像([提供於zip中](assets/asset-templates-tutorial-video--supporting-files.zip))至主版頁面上的Logo元素。

   * **按一下右鍵**&#x200B;並選取&#x200B;**「符合」>「框架符合選項……」>「內容符合」>「按比例填滿框架」**
   [深入瞭解框架彎管頭選項](https://helpx.adobe.com/indesign/using/frames-objects.html#fitting_objects_to_frames)，，而此設定適用於您的使用案例。

5. 透過「原地貼上」，從「頁面」和「頁面」的主版範本中向下複製頁首（標誌和公司名稱）。

   * 在第1頁上，按住Shift鍵並按一下macOS或Shift鍵並按一下Windows，以選取從「主版」頁面公開的「頁首」，然後將其刪除。
   * 從「主版」頁面，透過「就地貼上」將頁首複製到第1頁
   * 對第2頁重複這些步驟

6. 開啟「結構」面板，連按兩下每個面板，確定所有結構元素都對應到InDesign檔案中的真實元素。 移除任何未使用或不需要的元素。 請確認所有標籤皆為語意，且元素已正確標籤。

   >[!NOTE]
   >
   >請記住，構造不良的InDesign檔案是AEM資產範本出現問題的最常見原因，因此請確保標籤和結構乾淨正確。

## 在AEM Assets中建立和編寫資產範本 {#creating-and-authoring-an-asset-template-in-aem-assets}

>[!VIDEO](https://video.tv.adobe.com/v/19294?quality=12&learn=on)

1. **開始InDesign Server** 在連線埠8080上。
2. 確保 **AEM作者執行個體已設定為與您的InDesign Server互動**（反之亦然）。

   * [IDS背景工作Cloud Service設定](http://localhost:4502/etc/cloudservices/proxy/ids.html)
   * [雲端ProxyCloud Service設定](http://localhost:4502/etc/cloudservices/proxy.html)
   * [AEM Externalizer OSGi設定](http://localhost:4502/system/console/configMgr)

3. **已將InDesign檔案上傳至AEM Assets** 並允許AEM工作流程和InDesign Server完全處理資產。
4. **建立新範本** 在 **資產>範本** 並在Step #4中選取上傳至AEM的InDesign檔案。
5. **編輯資產範本** 建立於步驟#5，並編寫可編輯欄位。
6. 按一下 **完成** 以產生「資產範本」的最終高保真度轉譯。
7. 按一下「資產範本」卡片以開啟，並檢閱「資產轉譯」以下載高保真度轉譯。

## 其他资源 {#additional-resources}

InDesign範本檔案和支援的影像

下載 [InDesign範本檔案和支援的影像](assets/asset-templates-tutorial-video--supporting-files-1.zip)

* [InDesignCC試用版下載](https://creative.adobe.com/products/download/indesign)
* InDesign Server試用版可從以下網址下載： [Adobe發行前網站](https://www.adobeprerelease.com/) 或 [CC Enterprise客戶可以聯絡其客戶主管，要求InDesign Server試用版授權](https://www.adobe.com/products/indesignserver/faq.html)
