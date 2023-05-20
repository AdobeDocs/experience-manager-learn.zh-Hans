---
title: 建立檔案片段以保留收件者名稱和地址
seo-title: Creating Document Fragments to hold the recipient name and address
description: 這是建立第一個互動式通訊檔案的多步驟教學課程的第5部分。 在本部分中，我們將建立檔案片段來儲存收件者名稱和地址。
seo-description: This is part 5 of a multi-step tutorial for creating your first interactive communications document. In this part, we will create document fragment to hold the recipient name and address.
uuid: 689931e4-a026-4e62-9acd-552918180819
feature: Interactive Communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 404eed65-ec55-492a-85b5-59773896b217
topic: Development
role: Developer
level: Beginner
exl-id: 1d7093a8-3765-46ec-912a-b5a5503fd5af
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '244'
ht-degree: 0%

---

# 建立檔案片段以保留收件者名稱和地址 {#creating-document-fragments-to-hold-the-recipient-name-and-address}

在本部分中，我們將建立檔案片段來儲存收件者名稱和地址。

>[!VIDEO](https://video.tv.adobe.com/v/22350?quality=12&learn=on)

檔案片段包含互動式通訊檔案的文字內容。 此文字內容可以是靜態文字，或從基礎資料模型元素值插入。 例如Dear {name}，其中Dear是靜態文字，{name}是表單資料元素名稱。 在執行階段，這將根據name元素的值解析為Dear Gloria Rios或Dear John Jacobs。

RTF編輯器足夠直覺，讓業務使用者可以編寫文字並插入表單資料元素。 檔案片段編輯器能夠格式化文字、指定字型型別和樣式、插入特殊字元和建立超連結。

檔案片段編輯器也可以在文字中插入內嵌條件，如下所述 [視訊](https://helpx.adobe.com/experience-manager/kt/forms/using/editing-improvements-correspondence-mgmt-feature-video-use.html)

>[!NOTE]
>
>請確定您插入檔案片段中的表單資料模型元素是根元素的子代。 例如，在此使用案例中，請確定您選取的使用者物件元素是餘額物件的子項

## 后续步骤

[建立互動式通訊檔案](./partsix.md)