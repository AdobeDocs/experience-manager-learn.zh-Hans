---
title: AEM 收件箱
description: 根據工作流程資料新增欄，以自訂收件匣
feature: Adaptive Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5
kt: 5830
topic: Development
role: Developer
level: Experienced
exl-id: 3e1d86ab-e0c4-45d4-b998-75a44a7e4a3f
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: bd41cd9d64253413e793479b5ba900c8e01c0eab
workflow-type: tm+mt
source-wordcount: '206'
ht-degree: 5%

---

# AEM 收件箱

AEM收件匣可整合來自各種AEM元件(包括Forms工作流程)的通知和工作。 觸發包含「指派」任務步驟的表單工作流程時，相關聯的應用程式會在受指派人的「收件匣」中列為任務。

收件匣使用者介面提供清單和行事曆檢視來檢視任務。 您也可以配置檢視設定。 您可以根據各種引數篩選任務。

您可以自訂Experience Manager收件匣以變更欄的預設標題、重新排序欄的位置，以及根據工作流程的資料顯示其他欄。

>[!NOTE]
>
>您必須是管理員或工作流程管理員的成員才能自訂收件匣欄

## 欄自訂

[啟動AEM收件匣](http://localhost:4502/aem/inbox)
按一下「 」，開啟「管理員控制項」 _清單檢視_ 圖示，然後選取 _管理員控制_ 如下方熒幕擷圖所示

![admin-control](assets/open-customization.png)

在欄自訂UI中，您可以執行下列操作

* 刪除欄
* 重新排序欄
* 重新命名欄

## 品牌化自定义

在品牌自訂中，您可以執行下列動作

* 新增您的組織標誌
* 自定义标题文本
* 自訂說明連結
* 隱藏導覽選項

![收件匣品牌化](assets/branding-customization.PNG)

## 后续步骤

[新增已婚欄](./add-married-column.md)
