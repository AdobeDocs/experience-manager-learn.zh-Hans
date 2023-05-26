---
title: 使用批处理API生成交互式通信文档
description: 使用批处理API生成打印渠道文档的示例资产
feature: Interactive Communication
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 2cdf37e6-42ad-469a-a6e4-a693ab2ca908
last-substantial-update: 2019-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '414'
ht-degree: 2%

---

# 批处理API

您可以使用批处理API从模板生成多个交互式通信。 模板是一种没有任何数据的交互式通信。 批处理API将数据与模板结合起来以产生交互式通信。 该API在交互式通信的大量生产中很有用。 例如，电话帐单、多个客户的信用卡对帐单。

[了解有关批量生成API的更多信息](https://experienceleague.adobe.com/docs/experience-manager-65/forms/interactive-communications/generate-multiple-interactive-communication-using-batch-api.html)

本文提供了使用批处理API生成Interactive Communications文档的示例资源。

## 使用Watched文件夹批量生成

* 导入 [交互式通信模板](assets/Beneficiaries-confirmation.zip) 到AEM Forms服务器。
* 导入 [观察文件夹配置](assets/batch-generation-api.zip). 这将创建一个名为的文件夹 `batchAPI` 在你的C盘里。

**如果您在非Windows操作系统上运行AEM Forms，请按照下面提到的3个步骤操作：**

1. [打开观察文件夹](http://localhost:4502/libs/fd/core/WatchfolderUI/content/UI.html)
2. 选择BatchAPIWatchedFolder并单击“编辑”。
3. 更改路径以匹配您的操作系统。

![路径](assets/watched-folder-batch-api-basic.PNG)

* 下载并解压缩的内容 [zip文件](assets/jsonfile.zip). zip文件包含名为的文件夹 `jsonfile` 其中包含 `beneficiaries.json` 文件。 此文件包含生成3个文档的数据。

* 放下 `jsonfile` 文件夹中的Watched文件夹的输入文件夹。
* 提取文件夹以进行处理后，检查观察文件夹的结果文件夹。 您应会看到生成的3个PDF文件

## 使用REST请求批量生成

您可以调用 [批处理API](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/index.html) 通过REST请求。 您可以为其他应用程序公开REST端点，以调用API来生成文档。
提供的示例资产显示用于生成交互式通信文档的REST端点。 此servlet接受以下参数：

* fileName — 数据文件在文件系统中的位置。
* templatePath - IC模板路径
* saveLocation — 在文件系统中保存生成的文档的位置
* channelType — 打印、Web或两者
* recordId — 用于设置交互式通信名称的元素的JSON路径

以下屏幕截图显示了参数及其值
![示例请求](assets/generate-ic-batch-servlet.PNG)

## 在服务器上部署示例资产

* 导入 [信息通信技术模板](assets/ICTemplate.zip) 使用 [包管理器](http://localhost:4502/crx/packmgr/index.jsp)
* 导入 [自定义提交处理程序](assets/BatchAPICustomSubmit.zip) 使用 [包管理器](http://localhost:4502/crx/packmgr/index.jsp)
* 导入 [自适应表单](assets/BatchGenerationAPIAF.zip) 使用 [Forms和Document界面](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* 部署和启动 [自定义OSGI捆绑包](assets/batchgenerationapi.batchgenerationapi.core-1.0-SNAPSHOT.jar) 使用 [Felix Web控制台](http://localhost:4502/system/console/bundles)
* [通过提交表单触发批量生成](http://localhost:4502/content/dam/formsanddocuments/batchgenerationapi/jcr:content?wcmmode=disabled)
