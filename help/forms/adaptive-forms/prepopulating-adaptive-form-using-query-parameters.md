---
title: 使用查詢引數填入最適化Forms。
description: 使用查詢引數的資料填入最適化Forms。
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Experienced
kt: 11470
last-substantial-update: 2020-11-12T00:00:00Z
exl-id: 14ac6ff9-36b4-415e-a878-1b01ff9d3888
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '221'
ht-degree: 0%

---

# 使用查詢引數預先填入最適化Forms

我們其中一位客戶要求使用查詢引數填入最適化表單。 例如，在以下url中，最適化表單中的FirstName和LastName欄位分別設定為John和Doe

```html
https://forms.enablementadobe.com/content/forms/af/testingxml.html?FirstName=John&LastName=Doe
```

為了完成此使用案例，已建立新的最適化表單範本並與頁面元件建立關聯。 在此頁面元件中，我們有jsp可取得查詢引數，並建立可用來填入調適型表單的xml結構。

建立新最適化表單範本和頁面元件的詳細資訊如下 [本影片說明。](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/storing-and-retrieving-form-data/part5.html?lang=en)

以下是jsp頁面中使用的程式碼

```java
java.util.Enumeration enumeration = request.getParameterNames();
String dataXml = "<afData><afUnboundData><data>";
while (enumeration.hasMoreElements())
{
   String parameterName = (String) enumeration.nextElement();
   dataXml = dataXml + "<" + parameterName + ">" + request.getParameter(parameterName) + "</" + parameterName + ">";

}

dataXml = dataXml + "</data></afUnboundData></afData>";
//System.out.println("The data xml is "+dataXml);
slingRequest.setAttribute("data", dataXml);
```

>[!NOTE]
>
>如果您的表單使用結構描述，則xml的結構將會不同，您必須據以建置xml。


## 在您的系統上部署資產

* [使用封裝管理程式下載並安裝最適化表單範本](assets/populate-with-xml.zip)
* [下載並安裝最適化表單範例](assets/populate-af-with-query-paramters-form.zip)

* [預覽最適化表單](http://localhost:4502/content/dam/formsanddocuments/testingxml/jcr:content?wcmmode=disabled&amp;FirstName=John&amp;LastName=Doe)
您應該會看到最適化表單填入值John和Doe
