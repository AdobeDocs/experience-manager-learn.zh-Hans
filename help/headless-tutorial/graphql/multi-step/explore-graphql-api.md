---
title: 探索GraphQL API - AEM Headless快速入門 — GraphQL
description: 開始使用Adobe Experience Manager (AEM)和GraphQL。 使用內建GrapiQL IDE來探索AEM GraphQL API。 瞭解AEM如何根據內容片段模式自動產生GraphQL結構描述。 嘗試使用GraphQL語法建構基本查詢。
version: Cloud Service
mini-toc-levels: 1
kt: 6714
thumbnail: KT-6714.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 508b0211-fa21-4a73-b8b4-c6c34e3ba696
source-git-commit: ae27cbc50fc5c4c2e8215d7946887b99d480d668
workflow-type: tm+mt
source-wordcount: '1454'
ht-degree: 2%

---

# 探索GraphQL API {#explore-graphql-apis}

AEM的GraphQL API提供強大的查詢語言，可向下游應用程式公開內容片段的資料。 內容片段模型會定義內容片段使用的資料結構。 每當建立或更新內容片段模型時，都會翻譯結構描述並將其新增到組成GraphQL API的「圖表」。

在本章中，讓我們來探索一些常見的GraphQL查詢，以使用名為的IDE收集內容 [GraphiQL](https://github.com/graphql/graphiql). GraphiQL IDE可讓您快速測試和調整傳回的查詢和資料。 它也能讓您輕鬆存取檔案，讓您輕鬆學習和瞭解有哪些方法可用。

## 前提条件 {#prerequisites}

此教學課程包含多個部分，並假設您已完成下列步驟： [製作內容片段](./author-content-fragments.md) 已完成。

## 目標 {#objectives}

* 瞭解如何使用GraphiQL工具來使用GraphQL語法建構查詢。
* 瞭解如何查詢內容片段清單和單一內容片段。
* 瞭解如何篩選和請求特定的資料屬性。
* 瞭解如何聯結多個內容片段模型的查詢
* 瞭解如何保留GraphQL查詢。

## 启用 GraphQL 端点 {#enable-graphql-endpoint}

必須設定GraphQL端點，才能啟用內容片段的GraphQL API查詢。

1. 從AEM開始畫面，瀏覽至 **工具** > **一般** > **GraphQL**.

   ![導覽至GraphQL端點](assets/explore-graphql-api/navigate-to-graphql-endpoint.png)

1. 點選 **建立** 在產生的對話方塊右上角，輸入下列值：

   * 名稱*： **我的專案端點**.
   * 使用…… *提供的GraphQL結構描述： **我的專案**

   ![建立GraphQL端點](assets/explore-graphql-api/create-graphql-endpoint.png)

   點選 **建立** 以儲存端點。

   根據專案設定建立的GraphQL端點只會針對屬於該專案的模型啟用查詢。 在此情況下，僅針對 **個人** 和 **團隊** 模型可供使用。

   >[!NOTE]
   >
   > 也可以建立全域端點，以啟用跨多個設定的模型查詢。 使用此選項時請務必謹慎，因為這麼做可能會使環境遭受其他安全性弱點，並增加AEM管理的整體複雜性。

1. 您現在應該會在環境中看到啟用一個GraphQL端點。

   ![啟用graphql端點](assets/explore-graphql-api/enabled-graphql-endpoints.png)

## 使用 GraphiQL IDE

此 [GraphiQL](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/graphiql-ide.html) 工具可讓開發人員針對目前AEM環境中的內容建立和測試查詢。 GraphiQL工具也可讓使用者 **保留或儲存** 供生產設定中的使用者端應用程式使用的查詢。

接下來，使用內建的GraphiQL IDE來探索AEM GraphQL API的強大功能。

1. 從AEM開始畫面，瀏覽至 **工具** > **一般** > **GraphQL查詢編輯器**.

   ![導覽至GraphiQL IDE](assets/explore-graphql-api/navigate-graphql-query-editor.png)

   >[!NOTE]
   >
   > 在中，舊版的AEM可能未內建GraphiQL IDE。 您可以在下列步驟之後手動安裝 [指示](#install-graphiql).

1. 在右上角，確認「端點」已設為 **我的專案端點**.

   ![設定GraphQL端點](assets/explore-graphql-api/set-my-project-endpoint.png)

這會將所有查詢的範圍設定為在中建立的模型 **我的專案** 專案。

### 查詢內容片段清單 {#query-list-cf}

常見需求是查詢多個內容片段。

1. 將下列查詢貼到主面板中（取代註解清單）：

   ```graphql
   query allTeams {
     teamList {
       items {
         _path
         title
       }
     }
   } 
   ```

1. 按下 **播放** 按鈕來執行查詢。 您應該會看到上一個章節的內容片段結果：

   ![個人清單結果](assets/explore-graphql-api/all-teams-list.png)

1. 將游標放置在 `title` 文字並輸入 **CTRL+空格鍵** 以觸發程式碼提示。 新增 `shortname` 和 `description` 至查詢。

   ![使用程式碼隱藏更新查詢](assets/explore-graphql-api/update-query-codehinting.png)

1. 按「 」以再次執行查詢 **播放** 按鈕，您應該會看到結果包含 `shortname` 和 `description`.

   ![簡短名稱和說明結果](assets/explore-graphql-api/updated-query-shortname-description.png)

   此 `shortname` 是簡單屬性，且 `description` 是多行文字欄位，而GraphQL API可讓我們為類似的結果選擇各種格式 `html`， `markdown`， `json`，或 `plaintext`.

### 查詢巢狀片段

接下來，嘗試查詢正在擷取巢狀片段，請記住 **團隊** 模型參照 **個人** 模型。

1. 更新查詢以包含 `teamMembers` 屬性。 記住，這是 **片段參考** 「個人模型」的欄位。 可傳回Person模型的屬性：

   ```graphql
   query allTeams {
       teamList {
           items {
               _path
               title
               shortName
               description {
                   plaintext
               }
               teamMembers {
                   fullName
                   occupation
               }
           }
       }
   }
   ```

   JSON回應：

   ```json
   {
       "data": {
           "teamList": {
           "items": [
               {
               "_path": "/content/dam/my-project/en/team-alpha",
               "title": "Team Alpha",
               "shortName": "team-alpha",
               "description": {
                   "plaintext": "This is a description of Team Alpha!"
               },
               "teamMembers": [
                   {
                   "fullName": "John Doe",
                   "occupation": [
                       "Artist",
                       "Influencer"
                   ]
                   },
                   {
                   "fullName": "Alison Smith",
                   "occupation": [
                       "Photographer"
                   ]
                   }
                 ]
           }
           ]
           }
       }
   }
   ```

   針對巢狀片段查詢的功能是AEM GraphQL API的強大功能。 在這個簡單範例中，巢狀僅深兩層。 不過，也可以進一步巢狀內嵌片段。 例如，如果有 **地址** 與關聯的模型 **個人** 在單一查詢中可傳回所有三個模型的資料。

### 篩選內容片段清單 {#filter-list-cf}

接下來，我們來看看如何根據屬性值，將結果篩選為內容片段子集。

1. 在GraphiQL UI中輸入以下查詢：

   ```graphql
   query personByName($name:String!){
     personList(
       filter:{
         fullName:{
           _expressions:[{
             value:$name
             _operator:EQUALS
           }]
         }
       }
     ){
       items{
         _path
         fullName
         occupation
       }
     }
   }  
   ```

   上述查詢會針對系統中的所有「人員」片段執行搜尋。 在查詢開頭新增的篩選器會對 `name` 欄位和變數字串 `$name`.

1. 在 **查詢變數** 面板輸入下列內容：

   ```json
   {"name": "John Doe"}
   ```

1. 執行查詢，預期只有 **人員** 傳回內容片段的值為 `John Doe`.

   ![使用查詢變數來篩選](assets/explore-graphql-api/using-query-variables-filter.png)

   篩選和建立複雜查詢有許多其他選項，請參閱 [瞭解如何將GraphQL與AEM搭配使用 — 範例內容和查詢](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/sample-queries.html).

1. 增強上述查詢以擷取設定檔圖片

   ```graphql
   query personByName($name:String!){
     personList(
       filter:{
         fullName:{
           _expressions:[{
             value:$name
             _operator:EQUALS
           }]
         }
       }
     ){
       items{  
         _path
         fullName
         occupation
         profilePicture{
           ... on ImageRef{
             _path
             _authorUrl
             _publishUrl
             height
             width
   
           }
         }
       }
     }
   } 
   ```

   此 `profilePicture` 是內容參照，且預期是影像，因此內建 `ImageRef` 物件已使用。 這可讓我們請求有關所參考影像的其他資料，例如 `width` 和 `height`.

### 查詢單一內容片段 {#query-single-cf}

也可以直接查詢單一內容片段。 AEM中的內容以階層式方式儲存，片段的唯一識別碼取決於片段的路徑。

1. 在GraphiQL編輯器中輸入以下查詢：

   ```graphql
   query personByPath($path: String!) {
       personByPath(_path: $path) {
           item {
           fullName
           occupation
           }
       }
   }
   ```

1. 輸入下列內容 **查詢變數**：

   ```json
   {"path": "/content/dam/my-project/en/alison-smith"}
   ```

1. 執行查詢並觀察是否傳回單一結果。

## 保留查詢 {#persist-queries}

一旦開發人員對查詢和查詢傳回的結果資料感到滿意，下一步就是將查詢儲存或儲存到AEM。 此 [持久查詢](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html) 是將GraphQL API公開給使用者端應用程式的慣用機制。 一旦查詢持續存在，就可以使用GET請求來請求它，並在Dispatcher和CDN層進行快取。 持續查詢的效能會好很多。 除了效能優勢外，持續查詢還能確保使用者端應用程式不會意外顯示額外資料。 更多關於以下專案的詳細資訊： [您可以在這裡找到持續查詢](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html).

接下來，保留兩個簡單查詢，它們會在下一個章節中使用。

1. 在GraphiQL IDE中輸入下列查詢：

   ```graphql
   query allTeams {
       teamList {
           items {
               _path
               title
               shortName
               description {
                   plaintext
               }
               teamMembers {
                   fullName
                   occupation
               }
           }
       }
   }
   ```

   驗證查詢是否有效。

1. 下一個點選 **另存為** 並輸入 `all-teams` 作為 **查詢名稱**.

   查詢應顯示在下方 **持久查詢** 在左側邊欄中。

   ![所有團隊持久查詢](assets/explore-graphql-api/all-teams-persisted-query.png)
1. 接著點選省略符號 **...** 在持續性查詢旁邊，然後點選 **複製URL** 以將路徑複製到剪貼簿。

   ![複製持續查詢URL](assets/explore-graphql-api/copy-persistent-query-url.png)

1. 開啟新索引標籤，並在瀏覽器中貼上複製的路徑：

   ```plain
   https://$YOUR-AEMasCS-INSTANCEID$.adobeaemcloud.com/graphql/execute.json/my-project/all-teams
   ```

   它應該看起來與上述路徑類似。 您應該會看到傳回的查詢JSON結果。

   劃分上述URL：

   | 名称 | 描述 |
   | ---------|---------- |
   | `/graphql/execute.json` | 持久查询端点 |
   | `/my-project` | 專案設定 `/conf/my-project` |
   | `/all-teams` | 持久查詢的名稱 |

1. 返回GraphiQL IDE並使用加號按鈕 **+** 以保留NEW查詢

   ```graphql
   query personByName($name: String!) {
     personList(
       filter: {
         fullName:{
           _expressions: [{
             value: $name
             _operator:EQUALS
           }]
         }
       }){
       items {
         _path
         fullName
         occupation
         biographyText {
           json
         }
         profilePicture {
           ... on ImageRef {
             _path
             _authorUrl
             _publishUrl
             width
             height
           }
         }
       }
     }
   }
   ```

1. 將查詢儲存為： `person-by-name`.
1. 您應該已儲存兩個持續查詢：

   ![最終持續查詢](assets/explore-graphql-api/final-persisted-queries.png)


## 發佈GraphQL端點與持續查詢

檢閱及驗證後，發佈 `GraphQL Endpoint` 和 `Persisted Queries`

1. 從AEM開始畫面，瀏覽至 **工具** > **一般** > **GraphQL**.

1. 點選「 」旁的核取方塊 **我的專案端點** 並點選 **發佈**

   ![發佈GraphQL端點](assets/explore-graphql-api/publish-graphql-endpoint.png)

1. 從AEM開始畫面，瀏覽至 **工具** > **一般** > **GraphQL查詢編輯器**

1. 點選 **所有團隊** 從「持續查詢」面板中查詢並點選 **發佈**

   ![發佈持續查詢](assets/explore-graphql-api/publish-persisted-query.png)

1. 重複上述步驟 `person-by-name` 查詢

## 解決方案檔案 {#solution-files}

下載在前三章中建立的內容、模型和持續查詢： [tutorial-solution-content.zip](assets/explore-graphql-api/tutorial-solution-content.zip)

## 其他资源

若要進一步瞭解GraphQL查詢，請前往 [瞭解如何將GraphQL與AEM搭配使用 — 範例內容和查詢](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/sample-queries.html).

## 恭喜！ {#congratulations}

恭喜，您已建立並執行數個GraphQL查詢！

## 后续步骤 {#next-steps}

在下一章中， [建置React應用程式](./graphql-and-react-app.md)，瞭解外部應用程式如何查詢AEM GraphQL端點，並使用這兩個持續查詢。 此外，還向您介紹GraphQL查詢執行期間的一些基本錯誤處理方式。

## 安裝GraphiQL工具（選用） {#install-graphiql}

在中，有些版本的AEM (6.X.X) GraphiQL IDE工具需要手動安裝，請使用 [從此處取得指示](../how-to/install-graphiql-aem-6-5.md).

