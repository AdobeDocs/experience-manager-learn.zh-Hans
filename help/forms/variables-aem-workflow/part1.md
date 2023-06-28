---
title: AEM工作流中的变量[Part1]
description: 在AEM工作流中使用XML、JSON、ArrayList、文档类型的变量
feature: Adaptive Forms, Workflow
version: 6.5
topic: Development
role: Developer
level: Beginner
exl-id: f9782684-3a74-4080-9680-589d3f901617
source-git-commit: eecc275e38390b9330464c8ac0750efa2c702c82
workflow-type: tm+mt
source-wordcount: '422'
ht-degree: 0%

---

# AEM Workflow中的XML变量

如果您具有基于XSD的自适应表单，并且希望从工作流中的自适应表单提交中提取值，则通常会使用XML类型的变量。

以下视频将指导您完成创建字符串和XML类型的变量并在工作流中使用它们所需的步骤。

XML变量可用于预填充自适应表单，或将自适应表单的提交数据存储到您的工作流中。

字符串变量可由Xpathing填充到XML变量中。 然后，通常使用此字符串变量来填充发送电子邮件组件中的电子邮件模板占位符

>[!NOTE]
>
>如果您的自适应表单未与XSD关联，则用于获取元素值的XPath将如下所示
>
>**/afData/afUnboundData/data/submitterName**

自适应表单数据存储在如上所示的数据元素下。 **_上述XPath submitterName是自适应表单中文本字段的名称。_**

>[!NOTE]
>
>**AEM Forms 6.5.0**  — 创建类型为XML的变量以在工作流模型中捕获提交的数据时，请勿将XSD与变量关联。 这是因为当您提交基于XSD的自适应表单时，提交的数据不符合XSD。 XSD投诉数据包含在/afData/afBoundData/元素中。
>
>**AEM Forms 6.5.1**  — 如果将XSD与XML变量关联，则可以浏览架构元素以执行变量映射。 您将无法访问未绑定到架构元素的表单数据。 如果您的用例是访问绑定到架构元素的数据以及未绑定的数据，则不要在工作流中绑定架构与XML变量。您必须使用相应的XPath表达式来获取您需要的数据

## 创建XML变量

>[!VIDEO](https://video.tv.adobe.com/v/26440?quality=12&learn=on)

### 将架构与XML变量结合使用

**使用架构映射XML变量。 在AEM Forms 6.5.1及更高版本中使用此功能**

>[!VIDEO](https://video.tv.adobe.com/v/28098?quality=12&learn=on)

#### 在发送电子邮件中使用变量

>[!VIDEO](https://video.tv.adobe.com/v/26441?quality=12&learn=on)

要使资源在您的系统上正常工作，请执行以下步骤：

* [使用包管理器下载资产并将其导入AEM](assets/xmlandstringvariable.zip)
* [浏览工作流模型](http://localhost:4502/editor.html/conf/global/settings/workflow/models/vacationrequest.html) 了解工作流中使用的变量
* [配置电子邮件服务](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/notification.html#ConfiguringtheMailService)
* [打开自适应表单](http://localhost:4502/content/dam/formsanddocuments/applicationfortimeoff/jcr:content?wcmmode=disabled)
* 填写详细信息并提交表单。
