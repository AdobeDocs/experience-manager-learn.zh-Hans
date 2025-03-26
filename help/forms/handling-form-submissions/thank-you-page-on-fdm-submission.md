---
title: 在表单提交上显示提交ID
description: 在感谢页面中显示表单数据模型提交的响应
feature: Adaptive Forms
doc-type: article
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-13900
last-substantial-update: 2023-09-09T00:00:00Z
exl-id: 18648914-91cc-470d-8f27-30b750eb2f32
duration: 72
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '330'
ht-degree: 0%

---

# 自定义感谢页面

向REST端点提交自适应表单时，您会希望显示确认消息，以告知用户其表单提交成功。 POST响应包含有关提交的详细信息（如提交ID），而设计良好的确认消息包含有助于改善用户体验的提交ID。 此响应可显示在配置有自适应表单的感谢页面中。

以下屏幕快照显示正在使用表单数据模型提交操作提交的表单，并配置了感谢页面

![感谢页面](./assets/thank-you-page-fdm-submit.png)

表单数据模型的POST将始终在响应中返回JSON对象。 此JSON在感谢页面URL中作为名为&#x200B;_fdmSubmitResult_的查询参数提供。 您可以解析此查询参数，并在感谢页面中显示JSON元素。
以下示例代码解析JSON响应以提取数字字段的值。 然后，系统会构建适当的xml，并在slingRequest中传递它以填充表单。 此代码通常编写在与自适应表单模板关联的页面组件的jsp中。

```java
if(request.getParameter("fdmSubmitResult")!=null)
{
    String fdmSubmitResult =  request.getParameter("fdmSubmitResult");
    String status = request.getParameter("status");
    com.google.gson.JsonObject jsonObject = com.google.gson.JsonParser.parseString(fdmSubmitResult).getAsJsonObject();
    String caseNumber = jsonObject.get("result").getAsJsonObject().get("number").getAsString();
    slingRequest.setAttribute("data","<afData><afUnboundData><data><caseNumber>"+caseNumber+"</caseNumber><status>"+status+"</status></data></afUnboundData></afData>");
}
```

建议您将感谢页面基于新的自适应表单模板，通过模板可编写自定义代码以从查询参数中提取响应。

## 测试解决方案

创建自适应表单并将其配置为使用表单数据模型提交操作提交表单。
[部署示例自适应表单模板](assets/thank-you-page-template.zip)
基于此模板创建感谢表单
将此感谢页面与主表单关联
修改[createXml.jsp](http://localhost:4502/apps/thank-you-page-template/component/page/thankyoupage/createxml.jsp)中的jsp代码以生成预填充自适应表单所需的xml。
预览并提交您的自适应表单。
此时将显示感谢页面，并预填充在XML中指定的数据
