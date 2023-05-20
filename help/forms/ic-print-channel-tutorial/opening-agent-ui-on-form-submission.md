---
title: 在POST提交時開啟代理程式UI
seo-title: Opening Agent UI On POST Submission
description: 這是多步驟教學課程的第11部分，說明如何為列印頻道建立您的第一個互動式通訊檔案。 在本部分中，我們將啟動代理程式ui介面，以在表單提交時建立隨選通訊。
seo-description: This is part 11 of multistep tutorial for creating your first interactive communications document for the print channel. In this part, we will launch the agent ui interface for creating ad-hoc correspondence on form submission.
uuid: 96f34986-a5c3-400b-b51b-775da5d2cbd7
feature: Interactive Communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6168
thumbnail: 40122.jpg
topic: Development
role: Developer
level: Intermediate
exl-id: 509b4d0d-9f3c-46cb-8ef7-07e831775086
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '324'
ht-degree: 1%

---

# 在POST提交時開啟代理程式UI

在本部分中，我們將啟動代理程式ui介面，以在表單提交時建立隨選通訊。

本文將逐步引導您完成在提交表單時開啟代理程式ui介面的步驟。 典型的使用案例是客戶服務代理使用一些輸入引數填寫表單，並且在表單提交代理程式ui上開啟了預先填入來自表單資料模型預填服務的資料。表單資料模型預填服務的輸入引數提取自表單提交。

以下影片說明使用案例

>[!VIDEO](https://video.tv.adobe.com/v/40122?quality=12&learn=on)

```java
String accountNumber = request.getParameter("accountnumber"))
ParameterMap parameterMap = new ParameterMap();
RequestParameter icLetterId[] = new RequestParameter[1];
icLetterId[0] = new FormFieldRequestParameter("/content/dam/formsanddocuments/retirementstatementprint");
parameterMap.put("documentId", icLetterId);
RequestParameter Random[] = new RequestParameter[1];
Random[0] = new FormFieldRequestParameter("209457");
parameterMap.put("Random", Random);
Map map = new HashMap();
map.put("accountnumber",accountNumber);
slingRequest.setAttribute("paramMap",map);
CustomParameterRequest wrapperRequest = new CustomParameterRequest(slingRequest,parameterMap,"GET");
wrapperRequest.getRequestDispatcher("/aem/forms/createcorrespondence.html").include(wrapperRequest, response);
```

第1行：從requestparameter取得accountnumber

第2-8行：建立引數對映並設定適當的索引鍵和值，以反映documentId，Random。

第9-10行：建立另一個Map物件以保留表單資料模型中定義的輸入引數。

第11行：設定slingRequest屬性&quot;paramMap&quot;

第12-13行：將請求轉送至servlet

若要在您的伺服器上測試此功能

* [使用封裝管理程式匯入和安裝與本文相關的資產。](assets/launch-agent-ui.zip)
* [登入configMgr](http://localhost:4502/system/console/configMgr)
* 搜尋 _AdobeGranite CSRF篩選器_
* 新增 _/content/getprintchannel_ 在排除的路徑中
* 儲存您的變更。
* [開啟POST.jsp](http://localhost:4502/apps/AEMForms/openprintchannel/POST.jsp). 請確定傳遞至FormFieldRequestParameter的字串是有效的documentId。（第19行）。
* [開啟網頁](http://localhost:4502/content/OpenPrintChannel.html) 並輸入accountnumber並提交表單。
* Agent UI介面應會開啟，並預先填入表單中輸入accountnumber的特定資料。

>[!NOTE]
>
>請確定您的表單資料模型的Get作業輸入引數已繫結至名為「accountnumber」的請求屬性，這樣才能運作。 如果您將bindingvalue的名稱變更為任何其他名稱，請確定變更反映在POST.jsp的第25行上
