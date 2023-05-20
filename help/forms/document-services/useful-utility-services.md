---
title: 實用公用程式服務
description: AEM Forms開發人員的一些實用工具服務
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: add06b73-18bb-4963-b91f-d8e1eb144842
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '155'
ht-degree: 0%

---

# 實用公用程式服務

此範例套件提供有用的公用程式服務，可供AEM Forms開發人員使用。 可使用下列服務。


```java
package aemformsutilityfunctions.core;
import java.util.Map;
import com.adobe.aemfd.docmanager.Document;
public interface AemFormsUtilities
{
public abstract com.adobe.aemfd.docmanager.Document createDDXFromMapOfDocuments(Map<String, com.adobe.aemfd.docmanager.Document> paramMap);
public abstract org.w3c.dom.Document w3cDocumentFromStrng(String xmlString);
public abstract com.adobe.aemfd.docmanager.Document orgw3cDocumentToAEMFDDocument(org.w3c.dom.Document xmlDocument);
public abstract String saveDocumentInCrx(String jcrPath,String fileExtension, Document documentToSave);

}
```

範例組合可以是 [已從此處下載](assets/aemformsutilityfunctions.aemformsutilityfunctions.core-1.0-SNAPSHOT.jar)

## 使用公用程式服務的範常式式碼

以下是在JSP頁面中用來從字串建立org.w3c.dom.Document，並轉換檔案並將其儲存在CRX存放庫中的程式碼，如下列程式碼片段所示。

```java
 aemformsutilityfunctions.core.AemFormsUtilities aemFormsUtilities = sling.getService(aemformsutilityfunctions.core.AemFormsUtilities.class);
com.adobe.aemfd.docmanager.Document xmlStringDoc = aemFormsUtilities.orgw3cDocumentToAEMFDDocument(aemFormsUtilities.w3cDocumentFromStrng("<data><fname>Girish</fname></data>"));
aemFormsUtilities.saveDocumentInCrx("/content/xmlfiles",".xml",xmlStringDoc);
```

## 前提条件


您需要部署 [DevelopingWithServiceUserBundle](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/DevelopingWithServiceUser.jar) 並啟動套件組合。


如果您要使用這些公用程式服務將檔案儲存在CRX存放庫中，請遵循 [使用服務使用者文章開發](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/adaptive-forms/service-user-tutorial-develop.html?lang=en#adaptive-forms). 請務必提供 [必要許可權](http://localhost:4502/useradmin) 將適當的CRX資料夾上傳給fd-service使用者。
