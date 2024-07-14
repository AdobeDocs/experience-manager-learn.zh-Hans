---
title: 将自适应表单提交到外部服务器
description: 将自适应表单提交到外部服务器上运行的REST端点
feature: Adaptive Forms
doc-type: article
version: 6.4,6.5
discoiquuid: 9e936885-4e10-4c05-b572-b8da56fcac73
topic: Development
role: Developer
level: Beginner
exl-id: 5363c3f7-9006-4430-b647-f3283a366a64
last-substantial-update: 2020-07-07T00:00:00Z
duration: 78
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '340'
ht-degree: 12%

---

# 将自适应表单提交到外部服务器 {#submitting-adaptive-form-to-external-server}

使用提交到REST端点操作将提交的数据发布到REST URL。 该 URL 可以属于内部服务器（呈现表单的服务器）或外部服务器。

通常，客户希望将表单数据提交到外部服务器以供进一步处理。

要将数据发布到内部服务器，请提供资源的路径。 数据将发布到资源的路径。例如， &lt;/content/restEndPoint> 。 对于此类post请求，使用提交请求的认证信息。

要将数据发布到外部服务器，请提供 URL。URL 的格式为 <http://host:port/path_to_rest_end_point>。确保已配置匿名处理POST请求的路径。

出于本文的目的，我编写了一个简单的war文件，可部署到您的tomcat实例上。 假定您的tomcat在端口8080上运行，则POSTURL将为

<http://localhost:8080/AemFormsEnablement/HandleFormSubmission>

将自适应表单配置为提交到此端点时，表单数据和附件（如果有）可通过以下代码在servlet中提取

```java
System.out.println("form was submitted");
Part attachment = request.getPart("attachments");
if(attachment!=null)
{
    System.out.println("The content type of the attachment added is "+attachment.getContentType());
}
Enumeration<String> params = request.getParameterNames();
while(params.hasMoreElements())
{
String paramName = params.nextElement();
System.out.println("The param Name is "+paramName);
String data = request.getParameter(paramName);System.out.println("The data  is "+data);
}
```

![formsubmission](assets/formsubmission.gif)
要在您的服务器上对此进行测试，请执行以下操作

1. 安装Tomcat（如果尚未安装）。 [此处提供了安装tomcat的说明](https://helpx.adobe.com/experience-manager/kt/forms/using/preparing-datasource-for-form-data-model-tutorial-use.html)
1. 下载与此文章关联的[zip文件](assets/aemformsenablement.zip)。 解压缩文件以获取war文件。
1. 在tomcat服务器中部署war文件。
1. 创建一个带有文件附件组件的简单自适应表单，并配置其提交操作，如上面的屏幕快照所示。 POSTURL为<http://localhost:8080/AemFormsEnablement/HandleFormSubmission>。 如果您的AEM和tomcat未在localhost上运行，请相应地更改URL。
1. 要启用将多部分表单数据提交到tomcat的功能，请将以下属性添加到&lt;tomcatInstallDir>\conf\context.xml的上下文元素中，然后重新启动Tomcat服务器。
1. **&lt;Context allowCasualMultipartParsing=&quot;true&quot;>**
1. 预览自适应表单、添加附件并提交。 在tomcat控制台窗口中查看消息。
