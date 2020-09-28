---
title: 使用Batch API生成交互式通信文档
description: 使用批处理API生成打印渠道文档的示例资产
feature: interactive-communication
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5
translation-type: tm+mt
source-git-commit: 3dc1bd3f2f7b6324c53640f01a263fa0728d439c
workflow-type: tm+mt
source-wordcount: '416'
ht-degree: 0%

---


# 批处理API

您可以使用批处理API从模板生成多个交互式通信。 模板是无任何数据的交互式通信。 批处理API将数据与模板相结合以生成交互式通信。 API在大规模制作交互式通信中非常有用。 例如，电话单、多个客户的信用卡对帐单。

[进一步了解Batch Generation API](https://docs.adobe.com/content/help/en/experience-manager-65/forms/interactive-communications/generate-multiple-interactive-communication-using-batch-api.html)

本文提供了使用Batch API生成Interactive Communications文档的示例资源。

## 使用监视文件夹进行批量生成

* 将交互 [式通信模板导入您](assets/Beneficiaries-confirmation.zip) 的AEM Forms服务器。
* 导入监视 [的文件夹配置](assets/batch-generation-api.zip)。 这将在C驱动器中创 `batchAPI` 建一个名为的文件夹。

**如果您正在非windows操作系统上运行AEM Forms，请遵循以下3个步骤：**

1. [打开监视文件夹](http://localhost:4502/libs/fd/core/WatchfolderUI/content/UI.html)
2. 选择BatchAPIWatchedFolder，然后单击“编辑”。
3. 更改路径以匹配您的操作系统。

![路径](assets/watched-folder-batch-api-basic.PNG)

* 下载并解压zip文 [件的内容](assets/jsonfile.zip)。 zip文件包含名为的包 `jsonfile` 含文件的 `beneficiaries.json` 文件夹。 此文件具有生成3个文档的数据。

* 将该文 `jsonfile` 件夹放入监视文件夹的输入文件夹。
* 在选取文件夹进行处理后，检查已监视文件夹的结果文件夹。 您应当看到生成的3个PDF文件

## 使用REST请求生成批

您可以通过REST [请求调用](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/index.html) “批处理API”。 您可以为其他应用程序公开REST端点，以调用API以生成文档。
提供的示例资源公开了用于生成交互式通信文档的REST端点。 Servlet接受以下参数：

* fileName —文件系统上数据文件的位置。
* templatePath - IC模板路径
* saveLocation —— 用于在文件系统上保存生成的文档的位置
* channelType —— 打印、Web或两者
* recordId —— 用于设置交互式通信名称的元素的JSON路径

以下屏幕截图显示了参数及其值示例![请求。](assets/generate-ic-batch-servlet.PNG)

## 在服务器上部署示例资源

* 使用 [包管理器](assets/ICTemplate.zip) 导 [入ICT模板](http://localhost:4502/crx/packmgr/index.jsp)
* 使用包 [管理器导入自定义](assets/BatchAPICustomSubmit.zip) 提交 [处理程序](http://localhost:4502/crx/packmgr/index.jsp)
* 使用 [Forms](assets/BatchGenerationAPIAF.zip) 和文档 [界面导入自适应表单](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* 使用Felix Web控 [制台部署和开始自](assets/batchgenerationapi.batchgenerationapi.core-1.0-SNAPSHOT.jar) 定 [义OSGI捆绑包](http://localhost:4502/system/console/bundles)
* [通过提交表单触发批生成](http://localhost:4502/content/dam/formsanddocuments/batchgenerationapi/jcr:content?wcmmode=disabled)
