---
title: 使用批处理API生成交互式通信文档
description: 使用批处理API生成打印渠道文档的示例资产
feature: 交互式通信
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5
topic: 开发
role: Developer
level: Intermediate
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '417'
ht-degree: 3%

---


# 批处理API

您可以使用批处理API从模板生成多个交互式通信。 模板是一种无任何数据的交互式通信。 批处理API将数据与模板组合在一起，以生成交互式通信。 该API在大规模生产交互式通信中非常有用。 例如，电话单、多个客户的信用卡对帐单。

[了解有关批量生成API的更多信息](https://experienceleague.adobe.com/docs/experience-manager-65/forms/interactive-communications/generate-multiple-interactive-communication-using-batch-api.html)

本文提供了使用批处理API生成交互式通信文档的示例资产。

## 使用监视文件夹批量生成

* 将[交互式通信模板](assets/Beneficiaries-confirmation.zip)导入AEM Forms服务器。
* 导入[监视的文件夹配置](assets/batch-generation-api.zip)。 这将在C驱动器中创建一个名为`batchAPI`的文件夹。

**如果您在非windows操作系统上运行AEM Forms，请按照以下3个步骤操作：**

1. [打开监视文件夹](http://localhost:4502/libs/fd/core/WatchfolderUI/content/UI.html)
2. 选择BatchAPIWatchedFolder并单击“编辑”。
3. 更改路径以匹配您的操作系统。

![路径](assets/watched-folder-batch-api-basic.PNG)

* 下载并解压缩[zip文件](assets/jsonfile.zip)的内容。 zip文件包含名为`jsonfile`的文件夹，其中包含`beneficiaries.json`文件。 此文件具有用于生成3个文档的数据。

* 将`jsonfile`文件夹拖放到监视文件夹的输入文件夹中。
* 提取文件夹进行处理后，请检查监视文件夹的结果文件夹。 您应会看到生成了3个PDF文件

## 使用REST请求生成批量

您可以通过REST请求调用[批量API](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/index.html)。 您可以为其他应用程序公开REST端点以调用API以生成文档。
提供的示例资产公开了用于生成交互式通信文档的REST端点。 Servlet接受以下参数：

* fileName — 数据文件在文件系统上的位置。
* templatePath - IC模板路径
* saveLocation — 在文件系统上保存生成文档的位置
* channelType — 打印、Web或两者
* recordId — 用于设置交互式通信名称的元素的JSON路径

以下屏幕截图显示了参数及其值
![示例请求](assets/generate-ic-batch-servlet.PNG)

## 在服务器上部署示例资产

* 使用[包管理器](http://localhost:4502/crx/packmgr/index.jsp)导入[ICTemplate](assets/ICTemplate.zip)
* 使用[包管理器](http://localhost:4502/crx/packmgr/index.jsp)导入[自定义提交处理程序](assets/BatchAPICustomSubmit.zip)
* 使用[Forms和Document界面](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)导入[自适应表单](assets/BatchGenerationAPIAF.zip)
* 使用[Felix Web控制台](http://localhost:4502/system/console/bundles)部署和启动[自定义OSGi包](assets/batchgenerationapi.batchgenerationapi.core-1.0-SNAPSHOT.jar)
* [通过提交表单触发批生成](http://localhost:4502/content/dam/formsanddocuments/batchgenerationapi/jcr:content?wcmmode=disabled)
