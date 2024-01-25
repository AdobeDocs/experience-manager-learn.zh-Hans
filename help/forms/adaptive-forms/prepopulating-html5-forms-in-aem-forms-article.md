---
title: 使用数据属性预填充HTML5 Forms。
description: 通过从后端源获取数据来填充HTML5表单。
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: ab0f5282-383b-4be6-9c57-cded6ab37528
last-substantial-update: 2020-01-09T00:00:00Z
duration: 102
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '449'
ht-degree: 0%

---

# 使用数据属性预填充HTML5 Forms {#prepopulate-html-forms-using-data-attribute}


使用AEM Forms以HTML格式呈现的XDP模板称为HTML5或Mobile Forms。 一个常见用例是在呈现这些表单时预填充这些表单。

以HTML形式呈现数据时，有2种方法可将数据与xdp模板合并。

**dataRef**：您可以在URL中使用dataRef参数。 此参数指定与模板合并的数据文件的绝对路径。 此参数可以是一个指向rest服务的URL，该服务以XML格式返回数据。

**数据**：此参数指定与模板合并的UTF-8编码数据字节。 如果指定此参数，HTML5表单将忽略dataRef参数。 作为最佳实践，我们建议使用数据方法。

推荐的方法是，使用要预填充表单的数据来设置请求中的数据属性。

slingRequest.setAttribute(&quot;data&quot;， content)；

在本例中，我们使用内容设置数据属性。 内容表示要预填充表单的数据。 通常，您可以通过向内部服务进行REST调用来获取“内容”。

要实现此用例，您需要创建自定义用户档案。 有关创建自定义用户档案的详细信息，请参见 [此处提供AEM Forms文档](https://helpx.adobe.com/aem-forms/6/html5-forms/custom-profile.html).

创建自定义配置文件后，您将创建一个JSP文件，该文件将通过调用后端系统来提取数据。 获取数据后，您将使用slingRequest.setAttribute(&quot;data&quot;， content)；预填充表单

在渲染XDP时，您还可以将一些参数传递到xdp，并根据参数的值从后端系统中提取数据。

[例如，此url具有name参数](http://localhost:4502/content/dam/formsanddocuments/PrepopulateMobileForm.xdp/jcr:content?name=john)

您编写的JSP将有权通过request.getParameter(&quot;name&quot;)访问name参数。 然后，您可以将此参数的值传递给后端进程，以获取所需的数据。
要使此功能在您的系统上正常工作，请按照以下所述步骤操作：

* [使用包管理器下载资源并将其导入AEM](assets/prepopulatemobileform.zip)
该软件包将安装以下内容

   * 自定义配置文件
   * 示例XDP
   * 将返回数据以填充表单的示例POST端点

>[!NOTE]
>
>如果要通过调用Workbench进程填充表单，则可能需要在/apps/AEMFormsDemoListings/customprofiles/PrepopulateForm/html.jsp中包含callWorkbenchProcess.jsp，而不是setdata.jsp

* [将您最喜爱的浏览器指向此URL](http://localhost:4502/content/dam/formsanddocuments/PrepopulateMobileForm.xdp/jcr:content?name=Adobe%20Systems). 表单应预先填充name参数的值
