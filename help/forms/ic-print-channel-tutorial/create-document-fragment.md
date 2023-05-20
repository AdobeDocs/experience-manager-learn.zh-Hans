---
title: 建立檔案片段
description: 這是建立第一個互動式通訊檔案的多步驟教學課程的第5部分。 在本部分中，我們將建立檔案片段來儲存收件者名稱和地址。
seo-description: This is part 5 of a multi-step tutorial for creating your first interactive communications document. In this part, we will create document fragment to hold the recipient name and address.
uuid: 7fd8a0f2-a921-4e70-91c9-908dae9aeab2
feature: Interactive Communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 47d3aa97-0bff-48e0-8a65-55e5332f811b
kt: 5958
thumbnail: 22350.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 2fe3f950-bc2a-4e91-8d91-00438691727a
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '229'
ht-degree: 0%

---

# 建立檔案片段

在本部分中，我們將建立檔案片段來儲存收件者名稱和地址。

>[!VIDEO](https://video.tv.adobe.com/v/22350?quality=12&learn=on)

檔案片段包含互動式通訊檔案的文字內容。 此文字內容可以是靜態文字，或從基礎資料模型元素值插入。 例如 **親愛的 _{name}_**，其中Dear是靜態文字，name是表單資料模型元素名稱。 在執行階段，這將會解析為&#x200B;**親愛的Gloria Rios**或&#x200B;**親愛的約翰·雅各布斯**取決於name元素的值。

RTF編輯器足夠直覺，讓業務使用者可以編寫文字並插入表單資料元素。 檔案片段編輯器能夠格式化文字、指定字型型別和樣式、插入特殊字元和建立超連結。

檔案片段編輯器也可以在文字中插入內嵌條件，如下所述 [視訊](https://helpx.adobe.com/experience-manager/kt/forms/using/editing-improvements-correspondence-mgmt-feature-video-use.html)

>[!NOTE]
>
>請確定您插入檔案片段中的表單資料模型元素是根元素的子代。 例如，在此使用案例中，請確定您選取的使用者物件元素是餘額物件的子項

## 后续步骤

[建立列印管道檔案](./create-print-channel-document.md)
