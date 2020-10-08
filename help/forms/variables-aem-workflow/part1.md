---
title: AEM Workflow[Part1]中的变量
seo-title: AEM Workflow[Part1]中的变量
description: 在aem工作流中使用xml、json、arraylist、文档类型的变量
seo-description: 在aem工作流中使用xml、json、arraylist、文档类型的变量
feature: workflow
topics: development
audience: developer
doc-type: tutorial
activity: understand
version: 6.5
translation-type: tm+mt
source-git-commit: 3a3832a05ed9598d970915adbc163254c6eb83f1
workflow-type: tm+mt
source-wordcount: '437'
ht-degree: 0%

---


# AEM工作流中的XML变量

当您具有基于XSD的自适应表单并且希望从工作流中的自适应表单提交中提取值时，通常使用XML类型的变量。

以下视频将指导您完成创建字符串和XML类型的变量并在工作流程中使用它们所需的步骤。

XML变量可用于预填充自适应表单或将自适应表单的提交数据存储在工作流中。

字符串变量可以由Xpathing填充到XML变量中。 然后，此字符串变量通常用于填充“发送电子邮件”组件中的电子邮件模板占位符

>[!NOTE]
>
>如果自适应表单未与XSD关联，则获取元素值的XPath将与
>
>**/afData/afUnbindData/data/submitterName**

自适应表单数据存储在数据元素下，如上所示。 **_在上面的XPath submitterName是自适应表单中文本字段的名称。_**

>[!NOTE]
>
>**AEM Forms6.5.0** —— 当您创建XML类型的变量以捕获工作流模型中提交的数据时，请不要将XSD与该变量关联。 这是因为提交基于XSD的自适应表单时，提交的数据不符合XSD。 XSD投诉数据包含在/afData/afBoundData/元素中。
>
>**AEM Forms6.5.1** —— 如果将XSD与XML变量关联，您可以浏览模式元素以进行变量映射。 您将无法访问未绑定到模式元素的表单数据。 如果您的用例是访问绑定到模式元素和未绑定数据的模式，那么不要在工作流中将与XML变量绑定。您必须使用适当的XPath表达式来获取所需数据

## 创建XML变量

>[!VIDEO](https://video.tv.adobe.com/v/26440?quality=12?autoplay=1)

### 将模式与XML变量结合使用

**用模式映射XML变量。 从AEM Forms6.5.1开始使用此功能**

>[!VIDEO](https://video.tv.adobe.com/v/28098?quality=9&learn=on)

#### 使用发送电子邮件中的变量

>[!VIDEO](https://video.tv.adobe.com/v/26441?quality=12&learn=on)

要使资产在您的系统上工作，请按照以下步骤操作：

* [使用包管理器下载资产并将其导入AEM](assets/xmlandstringvariable.zip)
* [浏览工作流模型](http://localhost:4502/editor.html/conf/global/settings/workflow/models/vacationrequest.html) ，了解工作流中使用的变量
* [配置电子邮件服务](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/notification.html#ConfiguringtheMailService)
* [打开自适应表单](http://localhost:4502/content/dam/formsanddocuments/applicationfortimeoff/jcr:content?wcmmode=disabled)
* 填写详细信息并提交表单。

