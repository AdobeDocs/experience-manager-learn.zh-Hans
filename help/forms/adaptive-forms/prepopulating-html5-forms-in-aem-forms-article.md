---
title: 使用数据属性预填充HTML5 Forms。
seo-title: 使用数据属性预填充HTML5 Forms。
description: 通过从后端源获取数据来填充HTML5表单。
seo-description: 通过从后端源获取数据来填充HTML5表单。
feature: 集成
topics: mobile-forms
audience: developer
doc-type: article
activity: implement
version: 6.3,6.4,6.5.
uuid: 889d2cd5-fcf2-4854-928b-0c2c0db9dbc2
discoiquuid: 3aa645c9-941e-4b27-a538-cca13574b21c
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '511'
ht-degree: 0%

---


# 使用数据属性{#prepopulate-html-forms-using-data-attribute}预填充HTML5 Forms

请访问[AEM Forms示例](https://forms.enablementadobe.com/content/samples/samples.html?query=0)页面，获取此功能的实时演示的链接。

使用AEM Forms以HTML格式呈现的XDP模板称为HTML5或Mobile Forms。 一个常见用例是在呈现这些表单时预填充它们。

当将数据呈现为HTML时，有两种方法可将数据与xdp模板合并。

**dataRef**:可以在URL中使用dataRef参数。此参数指定与模板合并的数据文件的绝对路径。 此参数可以是返回XML格式数据的rest服务的URL。

**数据**:此参数指定与模板合并的UTF-8编码数据字节。如果指定此参数，则HTML5表单将忽略dataRef参数。 作为最佳实践，我们建议使用数据方法。

建议的方法是在请求中设置数据属性，并使用要预填充表单的数据。

slingRequest.setAttribute(&quot;data&quot;, content);

在此示例中，我们将设置内容的数据属性。 内容表示要预填充表单的数据。 通常，您会通过对内部服务发出REST调用来获取“内容”。

要实现此用例，您需要创建自定义用户档案。 有关创建自定义用户档案的详细信息，请在[此处的AEM Forms文档中详细说明。](https://helpx.adobe.com/aem-forms/6/html5-forms/custom-profile.html)

创建自定义用户档案后，您将创建JSP文件，该文件将通过向后端系统发出调用来获取数据。 获取数据后，您将使用slingRequest.setAttribute(&quot;data&quot;, content);以预填充表单

呈现XDP时，您还可以将某些参数传递到xdp，并根据参数的值可以从后端系统获取数据。

[例如，此url包含name参数](http://localhost:4502/content/dam/formsanddocuments/PrepopulateMobileForm.xdp/jcr:content?name=john)

您编写的JSP将有权通过request.getParameter(&quot;name&quot;)访问name参数。 然后，您可以将此参数的值传递给后端进程以获取所需数据。
要使此功能在您的系统上正常工作，请按照以下步骤操作：

* [下载资产并使用包管理器将资](assets/prepopulatemobileform.zip)
产导入AEM包将安装以下内容

   * CustomProfile
   * 示例XDP
   * 将返回POST以填充表单的示例

>[!NOTE]
>
>如果要通过调用Workbench Process填充表单，则可能希望在/apps/AEMFormsDemoListings/customprofiles/PrepopulateForm/html.jsp文件夹中包含callWorkbenchProcess.jsp，而不是setdata.jsp

* [将您最喜爱的浏览器指向此URL](http://localhost:4502/content/dam/formsanddocuments/PrepopulateMobileForm.xdp/jcr:content?name=Adobe%20Systems)。表单应预填充name参数的值
