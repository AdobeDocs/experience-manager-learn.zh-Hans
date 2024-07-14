---
title: 在POST提交时打开代理UI
description: 这是为打印渠道创建第一个交互式通信文档的多步教程的第11部分。 在本部分中，我们将启动代理ui界面，用于在表单提交时创建临时通信。
feature: Interactive Communication
doc-type: Tutorial
version: 6.4,6.5
jira: KT-6168
thumbnail: 40122.jpg
topic: Development
role: Developer
level: Intermediate
exl-id: 509b4d0d-9f3c-46cb-8ef7-07e831775086
duration: 170
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '322'
ht-degree: 0%

---

# 在POST提交时打开代理UI

在本部分中，我们将启动代理ui界面，用于在表单提交时创建临时通信。

本文将引导您完成在提交表单时打开代理ui界面所涉及的步骤。 典型的使用案例是客户服务代理填写带有一些输入参数的表格，在表格提交代理界面上打开由表格数据模型预填充服务预填充的数据。表格数据模型预填充服务的输入参数是从表格提交中提取的。

以下视频展示了用例

>[!VIDEO](https://video.tv.adobe.com/v/40122?quality=12&learn=on)

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

第1行：从requestparameter获取accountnumber

第2-8行：创建参数映射并设置相应的键和值以反映documentId，Random。

第9-10行：创建另一个Map对象以保存表单数据模型中定义的输入参数。

第11行：设置slingRequest属性“paramMap”

第12-13行：将请求转发到servlet

要在您的服务器上测试此功能，请执行以下操作

* [使用包管理器导入并安装与本文相关的资源。](assets/launch-agent-ui.zip)
* [登录到configMgr](http://localhost:4502/system/console/configMgr)
* 搜索&#x200B;_AdobeGranite CSRF筛选器_
* 在排除的路径中添加&#x200B;_/content/getprintchannel_
* 保存更改。
* [打开POST.jsp](http://localhost:4502/apps/AEMForms/openprintchannel/POST.jsp)。 确保传递给FormFieldRequestParameter的字符串是有效的documentId。（第19行）。
* [打开网页](http://localhost:4502/content/OpenPrintChannel.html)，输入accountnumber并提交表单。
* 代理UI界面应会打开，并预先填充特定于在表单中输入的帐号的数据。

>[!NOTE]
>
>确保您的表单数据模型的Get操作的输入参数已绑定到名为“accountnumber”的请求属性，以便此操作正常工作。 如果将bindingvalue的名称更改为任何其他名称，请确保此更改反映在POST.jsp的第25行上
