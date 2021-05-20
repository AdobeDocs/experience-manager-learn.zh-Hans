---
title: 将自适应表单提交到外部服务器
seo-title: 将自适应表单提交到外部服务器
description: 将自适应表单提交到在外部服务器上运行的REST端点
seo-description: 将自适应表单提交到在外部服务器上运行的REST端点
uuid: 1a46e206-6188-4096-816a-d59e9fb43263
feature: 自适应表单
topics: developing
audience: implementer
doc-type: article
activity: setup
version: 6.3,6.4,6.5
discoiquuid: 9e936885-4e10-4c05-b572-b8da56fcac73
topic: 开发
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '365'
ht-degree: 0%

---


# 将自适应表单提交到外部服务器{#submitting-adaptive-form-to-external-server}

使用Submit to REST Endpoint操作将提交的数据发布到REST URL。 URL可以是内部服务器（呈现表单的服务器）或外部服务器。

通常，客户会希望将表单数据提交到外部服务器以进一步处理。

要将数据发布到内部服务器，请提供资源的路径。 数据将发布到资源的路径中。 例如， &lt;/content/restEndPoint> 。 对于这种帖子请求，使用提交请求的验证信息。

要将数据发布到外部服务器，请提供URL。 URL的格式为<http://host:port/path_to_rest_end_point>。 确保您已配置路径以匿名处理POST请求。

为了撰写本文，我编写了一个简单的战争文件，该文件可以部署在您的tomcat实例上。 假设您的tomcat在端口8080上运行，则POSTurl将为

<http://localhost:8080/AemFormsEnablement/HandleFormSubmission>

将自适应表单配置为提交到此端点时，表单数据和附件（如果有）可通过以下代码在Servlet中提取

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

![](assets/formsubmission.gif)
formsubmission要在您的服务器上测试它，请执行以下操作

1. 安装Tomcat（如果尚未安装）。 [此处提供了安装tomcat的说明](https://helpx.adobe.com/experience-manager/kt/forms/using/preparing-datasource-for-form-data-model-tutorial-use.html)
1. 下载与本文关联的[zip文件](assets/aemformsenablement.zip)。 解压缩文件以获取战争文件。
1. 在tomcat服务器中部署war文件。
1. 创建带有文件附件组件的简单自适应表单，并配置其提交操作，如上面的屏幕截图所示。 POSTURL为<http://localhost:8080/AemFormsEnablement/HandleFormSubmission>。 如果您的AEM和tomcat未在localhost上运行，请相应地更改URL。
1. 要启用向tomcat提交多部分表单数据的功能，请将以下属性添加到&lt;tomcatInstallDir>\conf\context.xml的上下文元素中，然后重新启动Tomcat服务器。
1. **&lt;context allowCasualMultipartParsing=&quot;true&quot;>**
1. 预览自适应表单，添加附件并提交。 检查tomcat控制台窗口中的消息。

