---
title: 在AEM Forms中编写自定义提交
seo-title: 在AEM Forms中编写自定义提交
description: 快速、简便地为自适应表单创建自己的自定义提交操作
seo-description: 快速、简便地为自适应表单创建自己的自定义提交操作
feature: 自适应表单
topics: integrations
audience: developer
doc-type: article
activity: implement
version: 6.3,6.4,6.5
uuid: a26db0b9-7db4-4e80-813d-5c0438fabd1e
discoiquuid: 28611011-2ff9-477e-b654-e62e7374096a
topic: 开发
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '230'
ht-degree: 1%

---


# 在AEM Forms中编写自定义提交{#writing-a-custom-submit-in-aem-forms}

快速、简便地为自适应表单创建自己的自定义提交操作

本文将指导您完成创建自定义提交操作以处理自适应Forms提交所需的步骤。

* 登录到crx
* 在应用程序下创建类型为“sling :folder ”的节点。 让我们将此节点命名为CustomSubmitHelpx。
* 保存新创建的节点。
* 将以下两个属性添加到新创建的节点
* 属性名称       |属性值
* guideComponentType | fd/af/components/guidesubmittype
* guideDataModel     | xfa，xsd，基本
* jcr:description   | CustomSubmitHelpx
* 保存更改
* 在CustomSubmitHelpx节点下创建一个名为post.POST.jsp的新文件。提交自适应表单时，将调用此JSP。 您可以根据您在此文件中的要求编写JSP代码。 以下代码会将请求转发到Servlet。

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

现在，您将在自适应表单的提交操作中看到“CustomSubmitHelpx”，如此图像所示。

![具有自定义提交的自适应表单](assets/capture-2.gif)

