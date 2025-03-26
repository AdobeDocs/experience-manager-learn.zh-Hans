---
title: 在AEM Forms中编写自定义提交
description: 为自适应表单创建自定义提交操作的快速轻松方法
feature: Adaptive Forms
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
exl-id: 64b586a6-e9ef-4a3d-8528-55646ab03cc4
last-substantial-update: 2021-04-09T00:00:00Z
duration: 51
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '232'
ht-degree: 1%

---

# 在AEM Forms中编写自定义提交 {#writing-a-custom-submit-in-aem-forms}

为自适应表单创建自定义提交操作的快速轻松方法

>[!NOTE]
>本文中的代码不适用于基于核心组件的自适应表单。
>[此处提供了基于核心组件的自适应表单的等效文章](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/forms/custom-submit-headless-forms/custom-submit-service.html?lang=en)


本文将引导您完成创建自定义提交操作以处理自适应Forms提交所需的步骤。

* 登录crx
* 在应用程序下创建“sling ：folder”类型的节点。 让我们调用此节点CustomSubmitHelpx。
* 保存新创建的节点。
* 将以下三个属性添加到新创建的节点

| 属性名称 | 属性值 |
|----------------    | ---------------------------------|
| guideComponentType | fd/af/components/guidesubmittype |
| guideDataModel | xfa、xsd、basic |
| jcr ：description | CustomSubmitHelpx |


* 保存更改
* 在CustomSubmitHelpx节点下创建一个名为post.POST.jsp的新文件。提交自适应表单时，将调用此JSP。 您可以根据需要在此文件中编写JSP代码。 以下代码将请求转发到servlet。

```java
<%
%><%@include file="/libs/foundation/global.jsp"%>
<%@taglib prefix="cq" uri="http://www.day.com/taglibs/cq/1.0"%>
<%@ page import="org.apache.sling.api.request.RequestParameter,com.day.cq.wcm.api.WCMMode,com.adobe.forms.common.submitutils.CustomParameterRequest,com.adobe.aemds.guide.submitutils.*" %>

<%@ page import="org.apache.sling.api.request.RequestParameter,com.day.cq.wcm.api.WCMMode" %>
<%@page session="false" %>
<%

   com.adobe.aemds.guide.utils.GuideSubmitUtils.setForwardPath(slingRequest,"/bin/storeafsubmission",null,null);

%>
```

* 在CustomSubmitHelpx节点下创建名为addfields .jsp的文件。 此文件将允许您访问已签名的文档。
* 将以下代码添加到此文件

```java
    <%@include file="/libs/fd/af/components/guidesglobal.jsp"%>

    <%@page import="org.slf4j.LoggerFactory" %>

    <%@page import="org.slf4j.Logger" %>

    <input type="hidden" id="useSignedPdf" name="_useSignedPdf" value=""/>;
```

* 保存更改

现在，您将在自适应表单的提交操作中开始看到“CustomSubmitHelpx”，如本图所示。

![带有自定义提交的自适应表单](assets/capture-2.gif)
