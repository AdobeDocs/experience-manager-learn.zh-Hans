---
title: 使用AEM Forms的Acroform
seo-title: Merge Adaptive Form data with Acroform
description: 將Acroforms與AEM Forms整合的第1部分。 使用Acroform建立最適化表單並合併資料以取得PDF。
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '218'
ht-degree: 0%

---


# 建立Acroform

Acroform是使用Acrobat建立的表單。 您可以使用Acrobat從頭開始建立新表單，或採用在Microsoft Word中建立的現有表單，並使用Acrobat將其轉換為Acroform。 若要將在Microsoft Word中建立的表單轉換為Acroform，請依照下列步驟操作。

* 使用Acrobat開啟Word檔案
* 使用Acrobat準備表單工具來識別表單上的表單欄位。
* 儲存pdf。 請確定檔案名稱中沒有任何空格。


>[!VIDEO](https://video.tv.adobe.com/v/22575?quality=12&learn=on)

>[!NOTE]
>
>如果您想要傳送可填寫的Acroform以供使用Acrobat Sign簽署，請為欄位命名相應。 例如，您可以為欄位命名 **Sig_es_:signer1:簽名**. 這是Acrobat Sign瞭解的語法。

>[!NOTE]
>
>如果您要傳送以XFA為基礎的檔案，您必須將檔案平面化，且Acrobat Sign簽名標籤必須顯示為檔案中的靜態文字。

[Acrobat Sign文字標籤檔案](https://helpx.adobe.com/sign/using/text-tag.html)

>[!NOTE]
>
>請確定Acroform檔案名稱中沒有空格。 目前的範常式式碼不處理空格。
>
>表單欄位名稱只能包含下列內容：
>
>* 單一空間
>* 單底線
>* 英數字元

