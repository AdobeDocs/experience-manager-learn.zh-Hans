---
title: 作者內容片段 — AEM Headless的進階概念 — GraphQL
description: 在本章Adobe Experience Manager (AEM) Headless的進階概念中，瞭解如何在內容片段中使用標籤、日期和時間、JSON物件和片段參考。 設定檔案夾原則以限制可包含的內容片段模型。
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Intermediate
exl-id: 998d3678-7aef-4872-bd62-0e6ea3ff7999
source-git-commit: a500c88091d87e34c12d4092c71241983b166af8
workflow-type: tm+mt
source-wordcount: '2911'
ht-degree: 1%

---

# 作者內容片段

在 [上一章](/help/headless-tutorial/graphql/advanced-graphql/create-content-fragment-models.md)，您已建立五個內容片段模式：人員、團隊、位置、地址和聯絡資訊。 本章將逐步引導您根據這些模型建立內容片段。 它也會探索如何建立檔案夾原則，以限制哪些內容片段模型可用於檔案夾。

## 前提条件 {#prerequisites}

本檔案是多部分教學課程的一部分。 請確保 [上一章](create-content-fragment-models.md) 已完成，再繼續本章節。

## 目標 {#objectives}

在本章中，瞭解如何：

* 使用資料夾原則建立資料夾並設定限制
* 直接從內容片段編輯器建立片段參考
* 使用標籤、日期和JSON物件資料型別
* 將內容和片段參考插入多行文字編輯器中
* 新增多個片段參考
* 巢狀內嵌內容片段

## 安裝範例內容 {#sample-content}

安裝包含數個資料夾和用於加速教學課程之範例影像的AEM套件。

1. 下載 [Advanced-GraphQL-Tutorial-Starter-Package-1.1.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/Advanced-GraphQL-Tutorial-Starter-Package-1.1.zip)
1. 在AEM中，導覽至 **工具** > **部署** > **套件** 存取 **封裝管理員**.
1. 上傳並安裝上一步驟中下載的套件（zip檔案）。

   ![透過封裝管理程式上傳的封裝](assets/author-content-fragments/install-starter-package.png)

## 使用資料夾原則建立資料夾並設定限制

在AEM首頁中，選取 **資產** > **檔案** > **WKND已共用** > **英文**. 您可以在此處檢視各種內容片段類別，包括冒險和投稿人。

### 创建文件夹 {#create-folders}

導覽至 **冒險** 資料夾。 您可以看到已建立Teams和Locations的資料夾來儲存Teams和Locations內容片段。

建立以人員內容片段模式為基礎的講師內容片段資料夾。

1. 從「冒險」頁面中，選取 **建立** > **資料夾** 右上角。

   ![创建文件夹](assets/author-content-fragments/create-folder.png)

1. 在出現的「建立資料夾」強制回應視窗中，於 **標題** 欄位。 記下結尾的&#39;s&#39;。 包含許多片段的資料夾標題必須是複數。 选择&#x200B;**创建**。

   ![建立資料夾強制回應視窗](assets/author-content-fragments/create-folder-modal.png)

   您現在已建立一個資料夾來儲存冒險導師。

### 使用檔案夾原則設定限制

AEM可讓您定義內容片段資料夾的許可權和原則。 透過使用許可權，您只能授予特定使用者（作者）或作者群組存取特定資料夾的許可權。 透過使用資料夾原則，您可以限製作者在這些資料夾中可以使用的內容片段模式。 在此範例中，我們將資料夾限製為「人員」和「聯絡人資訊」模型。 若要設定資料夾原則：

1. 選取 **講師** 您建立的資料夾，然後選取 **屬性** 從頂端導覽列取得。

   ![属性](assets/author-content-fragments/properties.png)

1. 選取 **原則** 標籤，然後取消選取 **繼承自/content/dam/wknd-shared**. 在 **依路徑允許的內容片段模型** 欄位中，選取資料夾圖示。

   ![文件夹图标](assets/author-content-fragments/folder-icon.png)

1. 在開啟的「選取路徑」對話方塊中，依路徑操作 **conf** > **WKND已共用**. 在上一章建立的個人內容片段模式，包含對聯絡資訊內容片段模式的參考。 「講師」資料夾中必須同時允許「人員」和「聯絡資訊」模式，才能建立「講師」內容片段。 選取 **個人** 和 **連絡資訊**，然後按 **選取** 以關閉對話方塊。

   ![選取路徑](assets/author-content-fragments/select-path.png)

1. 選取 **儲存並關閉** 並選取 **確定** 在出現的成功對話方塊中。

1. 您現在已為「講師」資料夾設定了資料夾原則。 導覽至 **講師** 資料夾並選取 **建立** > **內容片段**. 您現在只能選取以下模型 **個人** 和 **連絡資訊**.

   ![檔案夾原則](assets/author-content-fragments/folder-policies.png)

## 為講師製作內容片段

導覽至 **講師** 資料夾。 從這裡，讓我們建立巢狀資料夾，以儲存講師的聯絡資訊。

請遵循以下章節中概述的步驟： [建立資料夾](#create-folders) 建立名為「聯絡人資訊」的資料夾。 巢狀資料夾繼承父資料夾的資料夾原則。 請隨意設定更具體的原則，以便新建立的資料夾僅允許使用聯絡資訊模型。

### 建立講師內容片段

讓我們建立四個可以新增到冒險講師團隊的人員。

1. 在「講師」資料夾中，根據人員內容片段模式建立內容片段，並將其標題命名為「Jacob Wester」。

   新建立的內容片段如下所示：

   ![個人內容片段](assets/author-content-fragments/person-content-fragment.png)

1. 在欄位中輸入下列內容：

   * **全名**：雅各布·韋斯特
   * **履歷**：雅各布·韋斯特當了10年的遠足教練，喜歡這幾乎每一分鐘！ 雅各布是一名探險家，具有攀巖和揹包操作的天賦。 Jacob是登山比賽的贏家，包括海灣之戰攀巖比賽。 Jacob目前住在加州。
   * **講師經驗層級**：專家
   * **技能**：攀巖、衝浪、揹包
   * **管理員詳細資訊**：雅各布·韋斯特三年來一直在協調揹包行動。

1. 在 **個人資料圖片** 欄位，新增影像的內容參照。 瀏覽至 **WKND已共用** > **英文** > **貢獻者** > **jacob_wester.jpg** 以建立影像的路徑。

### 從內容片段編輯器建立片段參考 {#fragment-reference-from-editor}

AEM可讓您直接從內容片段編輯器建立片段參考。 讓我們建立Jacob聯絡資訊的參考。

1. 選取 **新內容片段** 在 **連絡資訊** 欄位。

   ![新内容片段](assets/author-content-fragments/new-content-fragment.png)

1. 新內容片段模式隨即開啟。 在「選取目的地」標籤下方，依路徑操作 **冒險** > **講師** 並選取「 」旁邊的核取方塊 **連絡資訊** 資料夾。 選取 **下一個** 以繼續前往屬性標籤。

   ![新內容片段強制回應視窗](assets/author-content-fragments/new-content-fragment-modal.png)

1. 在「屬性」標籤下方，輸入「Jacob Wester聯絡資訊」於 **標題** 欄位。 選取 **建立**，然後按 **開啟** 在出現的成功對話方塊中。

   ![“属性”选项卡](assets/author-content-fragments/properties-tab.png)

   新欄位會出現，讓您編輯聯絡資訊內容片段。

   ![聯絡資訊內容片段](assets/author-content-fragments/contact-info-content-fragment.png)

1. 在欄位中輸入下列內容：

   * **電話**：209-888-0000
   * **電子郵件**： jwester@wknd.com

   完成後，選取 **儲存**. 您現在已建立連絡人資訊內容片段。

1. 若要導覽回講師內容片段，請選取 **雅各布·韋斯特** 編輯器的左上角。

   ![導覽回原始內容片段](assets/author-content-fragments/back-to-jacob-wester.png)

   此 **連絡資訊** 欄位現在包含參考的聯絡資訊片段的路徑。 這是巢狀片段參考。 完成的講師內容片段如下所示：

   ![Jacob Wester內容片段](assets/author-content-fragments/jacob-wester-content-fragment.png)

1. 選取 **儲存並關閉** 以儲存內容片段。 您現在有新的講師內容片段。

### 建立其他片段

遵循中概述的相同程式 [上一節](#fragment-reference-from-editor) 再為這些講師建立三個講師內容片段和三個聯絡資訊內容片段。 在講師片段中新增下列內容：

**Stacey Roswells**

| 字段 | 值 |
| --- | --- |
| 內容片段標題 | Stacey Roswells |
| 完整名称 | Stacey Roswells |
| 联系信息 | /content/dam/wknd-shared/en/adventures/instructers/contact-info/stacey-roswells-contact-info |
| 個人資料圖片 | /content/dam/wknd-shared/en/contributors/stacey-roswells.jpg |
| 履歷 | Stacey Roswells是一位成功的攀巖家和高山冒險家。 斯泰西出生於馬里蘭州巴爾的摩，是六個孩子中的老幺。 Stacey的父親是美國海軍的中校，母親則是現代舞蹈教練。 Stacey的家人經常因父親的職務任務而搬家，並在父親駐紮泰國時拍攝了第一張照片。 這也是Stacey學會攀巖的地方。 |
| 講師經驗層級 | 高级 |
| 技能 | 攀巖 |滑雪 |揹包 |

**Kumar Selvaraj**

| 字段 | 值 |
| --- | --- |
| 內容片段標題 | Kumar Selvaraj |
| 完整名称 | Kumar Selvaraj |
| 联系信息 | /content/dam/wknd-shared/en/adventures/instructors/contact-info/kumar-selvaraj-contact-info |
| 個人資料圖片 | /content/dam/wknd-shared/en/contributors/kumar-selvaraj.jpg |
| 履歷 | Kumar Selvaraj是一名經驗豐富的AMGA認證專業講師，其主要目標是幫助學生提高攀爬和登山技能。 |
| 講師經驗層級 | 高级 |
| 技能 | 攀巖 |揹包 |

**Ayo Ogunseinde**

| 字段 | 值 |
| --- | --- |
| 內容片段標題 | Ayo Ogunseinde |
| 完整名称 | Ayo Ogunseinde |
| 联系信息 | /content/dam/wknd-shared/en/adventures/instructors/contact-info/ayo-ogunseinde-contact-info |
| 個人資料圖片 | /content/dam/wknd-shared/en/contributors/ayo-ogunseinde-237739.jpg |
| 履歷 | Ayo Ogunseinde是一名職業登山者和揹包教練，居住在中加州Fresno。 Ayo的目標是引導徒步旅行者進行他們最壯觀的國家公園冒險。 |
| 講師經驗層級 | 高级 |
| 技能 | 攀巖 |騎腳踏車 |揹包 |

離開 **其他資訊** 欄位空白。

在聯絡資訊片段中新增下列資訊：

| 內容片段標題 | 手机 | 电子邮件 |
| ------- | -------- | -------- |
| Stacey Roswells聯絡資訊 | 209-888-0011 | sroswells@wknd.com |
| Kumar Selvaraj聯絡資訊 | 209-888-0002 | kselvaraj@wknd.com |
| Ayo Ogunseinde聯絡資訊 | 209-888-0304 | aogunseinde@wknd.com |

您現在已準備好建立團隊！

## 為位置製作內容片段

導覽至 **位置** 資料夾。 在這裡，您會看到兩個已建立的巢狀資料夾：約塞米蒂國家公園和約塞米蒂山谷旅館。

![位置資料夾](assets/author-content-fragments/locations-folder.png)

暫時忽略Yosemite Valley Lodge資料夾。 在本節稍後，當我們建立一個位置，作為我們講師團隊的Home Base時，我們會再回到它。

導覽至 **約塞米蒂國家公園** 資料夾。 目前只包含約塞米蒂國家公園的圖片。 讓我們使用位置內容片段模式建立內容片段，並將其命名為「約塞米蒂國家公園」。

### 索引標籤預留位置

AEM可讓您使用索引標籤預留位置來分組不同型別的內容，並讓您的內容片段更易於閱讀和管理。 在上一章中，您將定位字元預留位置新增至位置模型。 因此，位置內容片段現在有兩個索引標籤區段： **位置詳細資料** 和 **位置地址**.

![索引標籤預留位置](assets/author-content-fragments/tabs.png)

此 **位置詳細資料** 索引標籤包含 **名稱**， **說明**， **連絡資訊**， **位置影像**、和 **依季節的天氣** 欄位，而 **位置地址** 索引標籤包含對地址內容片段的引用。 這些標籤清楚說明必須填寫哪些型別的內容，因此製作內容更易於管理。

### JSON物件資料型別

此 **依季節的天氣** 欄位是JSON物件資料型別，這表示它接受JSON格式的資料。 此資料型別具有彈性，可用於您要包含在內容中的任何資料。

您可以將滑鼠移至欄位右側的資訊圖示上，檢視在上一章建立的欄位說明。

![JSON物件資訊圖示](assets/author-content-fragments/json-object-info.png)

在此情況下，我們需要提供位置的平均氣候。 輸入下列資料：

```json
{
    "summer": "81 / 89°F",
    "fall": "56 / 83°F",
    "winter": "46 / 51°F",
    "spring": "57 / 71°F"
}
```

此 **依季節的天氣** 欄位現在應如下所示：

![JSON 对象](assets/author-content-fragments/json-object.png)

### 新增內容

讓我們將其餘內容新增至位置內容片段，以便在下一章中使用GraphQL查詢資訊。

1. 在 **位置詳細資料** 標籤中，在欄位中輸入下列資訊：

   * **名稱**：約塞米蒂國家公園
   * **說明**：約塞米蒂國家公園位於加利福尼亞的內華達山脈。 它以華麗的瀑布、巨大的紅杉樹和標誌性的埃爾卡皮坦半圓頂懸崖景緻而聞名。 徒步旅行和露營是體驗約塞米蒂的最佳方式。 許多小徑提供無窮無盡的冒險和探索機會。

1. 從 **連絡資訊** 欄位，根據聯絡資訊模式建立內容片段，並將其標題命名為「約塞米蒂國家公園聯絡資訊」。 遵循上一節中概述的相同程式，於 [從編輯器建立片段參考](#fragment-reference-from-editor) 並在欄位中輸入下列資料：

   * **電話**：209-999-0000
   * **電子郵件**： yosemite@wknd.com

1. 從 **位置影像** 欄位，瀏覽至 **冒險** > **位置** > **約塞米蒂國家公園** > **約塞米蒂 — 國家公園.jpeg** 以建立影像的路徑。

   請記住，在上一章中，您設定了影像驗證，因此Location影像的尺寸必須小於2560 x 1800，而其檔案大小必須小於3 MB。

1. 新增所有資訊後， **位置詳細資料** 標籤現在看起來像這樣：

   ![位置詳細資訊索引標籤已完成](assets/author-content-fragments/location-details-tab-completed.png)

1. 導覽至 **位置地址** 標籤。 從 **地址** 欄位，使用您在上一章中建立的位址內容片段模式，建立名為「約塞米蒂國家公園位址」的內容片段。 遵循以下章節中概述的相同程式： [從編輯器建立片段參考](#fragment-reference-from-editor) 並在欄位中輸入下列資料：

   * **街道地址**：咖哩村通道9010號
   * **城市**：約塞米蒂谷
   * **州**： CA
   * **郵遞區號**：95389
   * **國家**：美國

1. 已完成 **位置地址** 約塞米蒂國家公園片段的標籤看起來像這樣：

   ![「位置地址」索引標籤已完成](assets/author-content-fragments/location-address-tab-completed.png)

1. 选择&#x200B;**保存并关闭**。

### 再建立一個片段

1. 導覽至 **約塞米蒂山谷旅館** 資料夾。 使用位置內容片段模式建立內容片段，並將其標題命名為「Yosemite Valley Lodge」。

1. 在 **位置詳細資料** 標籤中，在欄位中輸入下列資訊：

   * **名稱**：約塞米蒂山谷旅館
   * **說明**： Yosemite Valley Lodge是群組會議和各種活動的中心，例如購物、用餐、釣魚、遠足等。

1. 從 **連絡資訊** 欄位，根據聯絡資訊模型建立內容片段，並將其標題命名為「Yosemite Valley Lodge聯絡資訊」。 遵循以下章節中概述的相同程式： [從編輯器建立片段參考](#fragment-reference-from-editor) 並在新內容片段的欄位中輸入以下資料：

   * **電話**：209-992-0000
   * **電子郵件**： yosemitelodge@wknd.com

   儲存新建立的內容片段。

1. 導覽回至 **約塞米蒂山谷旅館** 並前往 **位置地址** 標籤。 從 **地址** 欄位，使用您在上一章中建立的位址內容片段模式，建立名為「Yosemite Valley Lodge Address」的內容片段。 遵循以下章節中概述的相同程式： [從編輯器建立片段參考](#fragment-reference-from-editor) 並在欄位中輸入下列資料：

   * **街道地址**： 9006 Yosemite Lodge Drive
   * **城市**：約塞米蒂國家公園
   * **州**： CA
   * **郵遞區號**：95389
   * **國家**：美國

   儲存新建立的內容片段。

1. 導覽回至 **約塞米蒂山谷旅館**，然後選取 **儲存並關閉**. 此 **約塞米蒂山谷旅館** 資料夾現在包含三個內容片段：Yosemite Valley Lodge、Yosemite Valley Lodge Contact Info和Yosemite Valley Lodge Address。

   ![Yosemite Valley Lodge資料夾](assets/author-content-fragments/yosemite-valley-lodge-folder.png)

## 編寫團隊內容片段

瀏覽資料夾至 **團隊** > **Yosemite團隊**. 您可以看到Yosemite Team資料夾目前僅包含團隊標誌。

![Yosemite團隊資料夾](assets/author-content-fragments/yosemite-team-folder.png)

讓我們使用團隊內容片段模式建立內容片段，並將其標題命名為「Yosemite團隊」。

### 多行文字編輯器中的內容和片段參考

AEM可讓您直接將內容和片段參考新增到多行文字編輯器中，並在稍後使用GraphQL查詢來擷取它們。 讓我們將內容和片段參考新增到 **說明** 欄位。

1. 首先，將下列文字新增至 **說明** 欄位： 「在約塞米蒂國家公園工作的專業冒險家和登山指導員團隊。」

1. 若要新增內容參照，請選取 **插入資產** 圖示來標籤多行文字編輯器。

   ![插入資產圖示](assets/author-content-fragments/insert-asset-icon.png)

1. 在出現的強制回應視窗中，選取 **team-yosemite-logo.png** 並按下 **選取**.

   ![選取影像](assets/author-content-fragments/select-image.png)

   內容參考現在已新增至 **說明** 欄位。

請記住，在上一章中，您允許將片段參考新增到 **說明** 欄位。 讓我們在這裡新增一個。

1. 選取 **插入內容片段** 圖示來標籤多行文字編輯器。

   ![「插入內容片段」圖示](assets/author-content-fragments/insert-content-fragment-icon.png)

1. 瀏覽至 **WKND已共用** > **英文** > **冒險** > **位置** > **約塞米蒂山谷旅館** > **約塞米蒂山谷旅館**. 按下 **選取** 以插入內容片段。

   ![插入內容片段強制回應視窗](assets/author-content-fragments/insert-content-fragment-modal.png)

   此 **說明** 欄位現在看起來像這樣：

   ![說明欄位](assets/author-content-fragments/description-field.png)

您現在已將內容和片段參考直接新增到多行文字編輯器中。

### 日期和時間資料型別

讓我們來看一下「日期」和「時間」資料型別。 選取 **行事曆** 圖示加以選取 **團隊建立日期** 欄位以開啟行事曆檢視。

![日期行事曆檢視](assets/author-content-fragments/date-calendar-view.png)

過去或未來的日期可以使用月份任一側的向前和向後箭頭來設定。 假設Yosemite團隊於2016年5月24日成立，因此我們將設定該日的日期。

### 新增多個片段參考

讓我們將講師新增到團隊成員片段參考。

1. 選取 **新增** 在 **團隊成員** 欄位。

   ![“添加”按钮](assets/author-content-fragments/add-button.png)

1. 在出現的新欄位中，選取資料夾圖示以開啟「選取路徑」強制回應視窗。 瀏覽資料夾至 **WKND已共用** > **英文** > **冒險** > **講師**，然後選取旁邊的核取方塊 **jacob-wester**. 按下 **選取** 以儲存路徑。

   ![片段參考路徑](assets/author-content-fragments/fragment-reference-path.png)

1. 選取 **新增** 再按三次。 使用新欄位將其餘三名講師新增至團隊。 此 **團隊成員** 欄位現在看起來像這樣：

   ![團隊成員欄位](assets/author-content-fragments/team-members-field.png)

1. 選取 **儲存並關閉** 以儲存團隊內容片段。

### 新增片段參考至冒險內容片段

最後，我們將新建立的內容片段新增到冒險中。

1. 導覽至 **冒險** > **Yosemite揹包** 並開啟Yosemite揹包內容片段。 在表單底部，您可以看到在上一章中建立的三個欄位： **位置**， **講師團隊**、和 **管理員**.

1. 在中新增片段參考 **位置** 欄位。 位置路徑應參考您建立的Yosemite國家公園內容片段： `/content/dam/wknd-shared/en/adventures/locations/yosemite-national-park/yosemite-national-park`.

1. 在中新增片段參考 **講師團隊** 欄位。 團隊路徑應參考您建立的Yosemite團隊內容片段： `/content/dam/wknd-shared/en/adventures/teams/yosemite-team/yosemite-team`. 這是巢狀片段參考。 Team內容片段包含參考連絡人資訊和地址模式之人員模式的參考。 因此，您會將巢狀內容片段向下調整到三個層級。

1. 在中新增片段參考 **管理員** 欄位。 假設雅各布·韋斯特是Yosemite Backpacking Adventure的管理員。 路徑應該會指向Jacob Wester內容片段，並顯示如下： `/content/dam/wknd-shared/en/adventures/instructors/jacob-wester`.

1. 您現在已將三個片段參考新增到冒險內容片段。 欄位看起來像這樣：

   ![冒險片段參考](assets/author-content-fragments/adventure-fragment-references.png)

1. 選取 **儲存並關閉** 以儲存您的內容。

## 恭喜！

恭喜！您現在已根據上一章建立的進階內容片段模型建立內容片段。 您也可以建立檔案夾原則，以限制在檔案夾中可以選取的內容片段模式。

## 后续步骤

在 [下一個章節](/help/headless-tutorial/graphql/advanced-graphql/explore-graphql-api.md)，您將瞭解如何使用GraphiQL整合式開發環境(IDE)傳送進階GraphQL查詢。 這些查詢可讓我們檢視在本章中建立的資料，並最終將這些查詢新增到WKND應用程式。
