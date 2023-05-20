---
title: 建立最適化表單
description: 建立並設定最適化表單以使用表單資料模型的預填服務
feature: Adaptive Forms
version: 6.4,6.5
kt: 5813
thumbnail: kt-5813.jpg
topic: Development
role: User
level: Beginner
exl-id: c8d4eed8-9e2b-458c-90d8-832fc9e0ad3f
source-git-commit: 4b47daf82e27f6bea4be30e3cdd132f497f4c609
workflow-type: tm+mt
source-wordcount: '606'
ht-degree: 1%

---

# 建立最適化表單

到目前為止，我們已建立下列專案

* 具有2個資料表的資料庫 —  `newhire` 和 `beneficiaries`
* 已設定的Apache Sling Connection Pooled DataSource
* 以RDBMS為基礎的表單資料模型

下一步是建立和設定最適化表單，以使用表單資料模型。  若要搶先一步，您可以 [下載和匯入](assets/fdm-demo-af.zip) 範例表單。 範例表單有一個區段可顯示員工詳細資訊，另一個區段可列出員工的受益人。

## 將表單與表單資料模型建立關聯

本課程隨附的範例表單未與任何表單資料模型建立關聯。 若要設定表單以使用表單資料模型，我們需要執行下列動作：

* 選取FDMDemo表單
* 按一下 _屬性_->_表單模型_
* 從下拉式清單中選取表單資料模型
* 搜尋並選取您在上一堂課中建立的表單資料模型。
* 按一下 _儲存並關閉_

## 設定預填服務

第一步是關聯表單的預填服務。 若要與預填服務建立關聯，請遵循下列步驟

* 選取 `FDMDemo` 表單
* 按一下 _編輯_ 以編輯模式開啟表單
* 在內容階層中選取「表單容器」，然後按一下扳手圖示以開啟其屬性工作表
* 選取 _表單資料模型預填服務_ 從「預填服務」下拉式清單
* 按一下藍色☑以儲存變更

* ![預填服務](assets/fdm-prefill.png)

## 設定員工詳細資訊

下一個步驟是將最適化表單的文字欄位繫結至表單資料模型元素。 您必須開啟下列欄位的屬性表，並設定其bindRef，如下所示


| 字段名 | 繫結參考 |
|------------|--------------------|
| 名字 | /newhire/FirstName |
| 姓氏 | /newhire/lastName |

>[!NOTE]
>
>您可以新增其他文字欄位，並將其繫結至適當的表單資料模型元素

## 設定受益人表格

下一步是以表格方式顯示員工的受益人。 提供的範例表單有一個包含4欄和單列的表格。 我們需要設定表格以隨著受益人人數而成長。

* 在編輯模式下開啟表單。
* 展開根面板 — >您的受益人 — >表格
* 選取「列1」，然後按一下扳手圖示以開啟其屬性表。
* 將繫結參考設定為 **/newhire/GetEmployeeRefineties**
* 將「重複設定 — 最小計數」設定為1，將「最大計數」設定為5。
* 您的Row1設定應該看起來像下面的熒幕擷圖
   ![row-configure](assets/configure-row.PNG)
* 按一下藍色☑鈕以儲存變更

## 繫結列儲存格

最後，我們需要將「列」儲存格繫結至「表單資料模型」元素。

* 展開根面板 — >您的受益人 — >表格 — >Row1
* 依照下表設定每個資料列儲存格的繫結參考

| 列儲存格 | Bind 引用 |
|------------|----------------------------------------------|
| 名字 | /newhire/GetEmployeeRefitures/firstname |
| 姓氏 | /newhire/GetEmployeeInfelieves/lastname |
| 關係 | /newhire/GetEmployeeRefineties/relation |
| 百分比 | /newhire/GetEmployeeRefitures/percentage |

* 按一下藍色☑鈕以儲存變更

## 測試您的表單

我們現在需要在URL中使用適當的empID開啟表單。 下列2個連結會使用資料庫中的資訊填入表單
[empID=207的表單](http://localhost:4502/content/dam/formsanddocuments/fdmdemo/jcr:content?wcmmode=disabled&amp;empID=207)
[empID=208的表單](http://localhost:4502/content/dam/formsanddocuments/fdmdemo/jcr:content?wcmmode=disabled&amp;empID=208)

## 疑难解答

我的表單是空白的，沒有任何資料

* 請確定表單資料模型傳回正確結果。
* 表單與正確的表單資料模型相關聯
* 檢查欄位繫結
* 請檢視stdout記錄檔。 您應該會看到正在寫入檔案的empID。如果您沒有看到此值，則表示您的表單可能未使用提供的自訂範本。

未填入表格

* 檢查Row1繫結
* 請確定Row1的重複設定已正確設定（最小值=1和最大值= 5或更多）
