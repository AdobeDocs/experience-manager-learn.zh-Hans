---
title: 有用的实用工具服务
description: AEM Forms开发人员的一些实用工具服务
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

# 有用的实用工具服务

此示例捆绑包提供了可供AEM Forms开发人员使用的有用实用程序服务。 可以使用以下服务。


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

示例捆绑包可以是 [已从此处下载](assets/aemformsutilityfunctions.aemformsutilityfunctions.core-1.0-SNAPSHOT.jar)

## 使用实用程序服务的示例代码

以下代码是在JSP页面中用于从字符串创建org.w3c.dom.Document并转换文档的代码，然后将其存储在CRX存储库中，如以下代码片段所示。

```java
 aemformsutilityfunctions.core.AemFormsUtilities aemFormsUtilities = sling.getService(aemformsutilityfunctions.core.AemFormsUtilities.class);
com.adobe.aemfd.docmanager.Document xmlStringDoc = aemFormsUtilities.orgw3cDocumentToAEMFDDocument(aemFormsUtilities.w3cDocumentFromStrng("<data><fname>Girish</fname></data>"));
aemFormsUtilities.saveDocumentInCrx("/content/xmlfiles",".xml",xmlStringDoc);
```

## 前提条件


您需要部署 [DevelopingWithServiceUserBundle](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/DevelopingWithServiceUser.jar) 然后启动捆绑包。


如果要使用这些实用程序服务将文档保存在CRX存储库中，请按照 [使用服务用户文章进行开发](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/adaptive-forms/service-user-tutorial-develop.html?lang=en#adaptive-forms). 确保您提供 [所需权限](http://localhost:4502/useradmin) 将相应的CRX文件夹添加到fd-service用户。
