---
title: 使用Watched資料夾產生列印管道檔案
seo-title: Generating Print Channel Documents Using Watched Folder
description: 這是多步驟教學課程的第10部分，說明如何為列印頻道建立您的第一個互動式通訊檔案。 在本部分中，我們將使用watched資料夾機制產生列印管道檔案。
seo-description: This is part 10 of multistep tutorial for creating your first interactive communications document for the print channel. In this part, we will generate print channel documents using the watched folder mechanism.
uuid: 9e39f4e3-1053-4839-9338-09961ac54f81
feature: Interactive Communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
contentOwner: gbedekar
discoiquuid: 23fbada3-d776-4b77-b381-22d3ec716ae9
topic: Development
role: Developer
level: Beginner
exl-id: 9bb05c94-2a7b-4149-b567-186eb08b1c66
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '348'
ht-degree: 0%

---

# 使用Watched資料夾產生列印管道檔案

在本部分中，我們將使用watched資料夾機制產生列印管道檔案。

在建立及測試您的列印管道檔案後，我們需要一種機制，以批次模式或依需求產生這些檔案。 通常，這些型別的檔案會以批次模式產生，最常見的機制是使用watched資料夾。

當您在AEM中設定Watched資料夾時，您會建立ECMA指令碼或Java程式碼的關聯，當檔案被放入Watched資料夾時就會執行。 在本文中，我們將重點介紹ECMA指令碼，它會產生列印通道檔案並將其儲存至檔案系統。

watched資料夾設定和ECMA指令碼是您於匯入的資產的一部分 [本教學課程的開頭](introduction.md)

放入watched資料夾的輸入檔案具有下列結構。 ECMA指令碼會讀取帳號，並產生每個帳號的列印管道檔案。

如需有關產生檔案的ECMA指令碼的詳細資訊， [請參閱本文](/help/forms/interactive-communications/generating-interactive-communications-print-document-using-api-tutorial-use.md)

```xml
<accountnumbers>
 <accountnumber>509840</accountnumber>
 <accountnumber>948576</accountnumber>
 <accountnumber>398762</accountnumber>
 <accountnumber>291723</accountnumber>
 <accountnumber>291724</accountnumber>
 <accountnumber>291725</accountnumber>
 <accountnumber>291726</accountnumber>
 <accountnumber>291727</accountnumber>
</accountnumbers>
```

若要使用watched資料夾機制產生列印管道檔案，請遵循下列步驟：

* [請依照本檔案提及的步驟操作](/help/forms/adaptive-forms/service-user-tutorial-develop.md)

* 登入crx並導覽至/etc/fd/watchfolder/scripts/PrintPDF.ecma

* 請確定interactiveCommunicationsDocument的路徑指向您要列印的正確檔案。（第1行）
* 記下saveLocation （第2行）。您可以視需要加以變更。
* 請確定表單資料模型的輸入引數已繫結至要求屬性，且其繫結值已設為「accountnumber」。 請參閱下面的熒幕擷圖。
   ![請求](assets/requestattributeprintchannel.gif)

* 建立包含以下內容的accountnumbers.xml檔案

```xml
<accountnumbers>
<accountnumber>1</accountnumber>
<accountnumber>100</accountnumber>
<accountnumber>101</accountnumber>
<accountnumber>1009</accountnumber>
<accountnumber>10009</accountnumber>
<accountnumber>11990</accountnumber>
</accountnumbers>
```

* 將xml檔案拖放到C:\RenderPrintChannel\input中

* 依照ECMA指令碼中的指定，檢查儲存位置中的pdf檔案。

## 后续步骤

[在表單提交時開啟代理程式ui](./opening-agent-ui-on-form-submission.md)