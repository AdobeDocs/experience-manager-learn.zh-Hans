---
title: 提交POST时打开代理UI
seo-title: 提交POST时打开代理UI
description: 这是创建用于打印渠道的第一个交互式通信文档的多步教程的第11部分。 在本部分中，我们将启动代理用户界面，以便在提交表单时创建临时通信。
seo-description: 这是创建用于打印渠道的第一个交互式通信文档的多步教程的第11部分。 在本部分中，我们将启动代理用户界面，以便在提交表单时创建临时通信。
uuid: 96f34986-a5c3-400b-b51b-775da5d2cbd7
feature: 交互通信
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6168
thumbnail: 40122.jpg
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '365'
ht-degree: 1%

---


# 提交POST时打开代理UI

在本部分中，我们将启动代理用户界面，以便在提交表单时创建临时通信。

本文将引导您完成在提交表单时打开代理用户界面所涉及的步骤。 典型的用例是客户服务代理用一些输入参数填写表单，在表单提交代理用户界面上用从表单数据模型预填服务预填充的数据进行打开。表单数据模型预填服务的输入参数是从表单提交中提取的。

以下视频显示了用例

>[!VIDEO](https://video.tv.adobe.com/v/40122/?quality=9&learn=on)

```java
String accountNumber = request.getParameter("accountnumber"))
ParameterMap parameterMap = new ParameterMap();
RequestParameter icLetterId[] = new RequestParameter[1];
icLetterId[0] = new FormFieldRequestParameter("/content/dam/formsanddocuments/retirementstatementprint");
parameterMap.put("documentId", icLetterId);
RequestParameter Random[] = new RequestParameter[1];
Random[0] = new FormFieldRequestParameter("209457");
parameterMap.put("Random", Random);
Map map = new HashMap();
map.put("accountnumber",accountNumber);
slingRequest.setAttribute("paramMap",map);
CustomParameterRequest wrapperRequest = new CustomParameterRequest(slingRequest,parameterMap,"GET");
wrapperRequest.getRequestDispatcher("/aem/forms/createcorrespondence.html").include(wrapperRequest, response);
```

第1行：从request参数获取帐号

第2-8行：创建参数映射并设置相应的键和值以反映documentId，Random。

第9-10行：创建另一个Map对象，以保存在表单数据模型中定义的输入参数。

第11行：设置slingRequest属性“paramMap”

第12-13行：将请求转发到Servlet

在服务器上测试此功能

* [使用包管理器导入和安装与本文相关的资产。](assets/launch-agent-ui.zip)
* [登录到configMgr](http://localhost:4502/system/console/configMgr)
* 搜索&#x200B;_Adobe花岗岩CSRF滤镜_
* 在排除路径中添加&#x200B;_/content/getprintchannel_
* 保存更改。
* [打开POST.jsp](http://localhost:4502/apps/AEMForms/openprintchannel/POST.jsp)。确保传递到FormFieldRequestParameter的字符串是有效的documentId。（第19行）。
* [打开网](http://localhost:4502/content/OpenPrintChannel.html) 页，输入帐户号并提交表单。
* 代理UI界面应打开，其中特定于在表单中输入的帐号预填充的数据。

>[!NOTE]
>
>确保表单数据模型的“获取”操作的输入参数绑定到名为“accountnumber”的请求属性，以使其正常工作。 如果将绑定值的名称更改为任何其他名称，请确保所做的更改反映在POST.jsp的第25行

