---
title: 在AEM Forms写定制
seo-title: 在AEM Forms写定制
description: 快速轻松地为自适应表单创建自己的自定义提交操作
seo-description: 快速轻松地为自适应表单创建自己的自定义提交操作
feature: adaptive-forms
topics: integrations
audience: developer
doc-type: article
activity: implement
version: 6.3,6.4,6.5
uuid: a26db0b9-7db4-4e80-813d-5c0438fabd1e
discoiquuid: 28611011-2ff9-477e-b654-e62e7374096a
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '227'
ht-degree: 0%

---


# 在AEM Forms编写自定义提交{#writing-a-custom-submit-in-aem-forms}

快速轻松地为自适应表单创建自己的自定义提交操作

本文将引导您完成创建自定义提交操作以处理自适应Forms提交所需的步骤。

* 登录crx
* 在应用程序下创建“sling :folder ”类型的节点。 我们将此节点命名为CustomSubmitHelpx。
* 保存新创建的节点。
* 将以下两个属性添加到新创建的节点
* 属性名称       |属性值
* guideComponentType | fd/af/components/guidesubmittype
* guideDataModel     | xfa,xsd,basic
* jcr:description   | CustomSubmitHelpx
* 保存更改
* 在CustomSubmitHelpx节点下新建一个名为post.POST.jsp的文件。提交自适应表单时，将调用此JSP。 您可以根据您的要求在此文件中编写JSP代码。 以下代码将请求转发到servlet。

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
* 向此文件添加以下代码

```java
    <%@include file="/libs/fd/af/components/guidesglobal.jsp"%>

    <%@page import="org.slf4j.LoggerFactory" %>

    <%@page import="org.slf4j.Logger" %>

    <input type="hidden" id="useSignedPdf" name="_useSignedPdf" value=""/>;
```

* 保存更改

现在您将开始在自适应表单的提交操作中看到“CustomSubmitHelpx”，如图所示。

![具有自定义提交的自适应表单](assets/capture-2.gif)

