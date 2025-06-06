---
title: 使用查询参数填充自适应Forms。
description: 使用查询参数中的数据填充自适应Forms。
feature: Adaptive Forms
version: Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
jira: KT-11470
last-substantial-update: 2020-11-12T00:00:00Z
exl-id: 14ac6ff9-36b4-415e-a878-1b01ff9d3888
duration: 49
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '199'
ht-degree: 0%

---

# 使用查询参数预填充自适应Forms

我们的一位客户要求使用查询参数填充自适应表单。 例如，在以下url中，自适应表单中的FirstName和LastName字段分别设置为John和Doe

```html
https://forms.enablementadobe.com/content/forms/af/testingxml.html?FirstName=John&LastName=Doe
```

要完成此用例，需创建新的自适应表单模板并将其与页面组件关联。 在此页面组件中，我们使用jsp获取查询参数并创建可用于填充自适应表单的xml结构。

有关创建新的自适应表单模板和页面组件的详细信息[在此视频中进行了说明。](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/storing-and-retrieving-form-data/part5.html?lang=zh-Hans)

以下是在jsp页面中使用的代码

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
>如果您的表单使用架构，则xml的结构将不同，您必须相应地构建xml。


## 在系统上部署资源

* [使用包管理器下载并安装自适应表单模板](assets/populate-with-xml.zip)
* [下载并安装自适应表单示例](assets/populate-af-with-query-paramters-form.zip)

* [预览自适应表单](http://localhost:4502/content/dam/formsanddocuments/testingxml/jcr:content?wcmmode=disabled&amp;FirstName=John&amp;LastName=Doe)
您应该会看到自适应表单中填充了值John和Doe
