---
title: 包含AEM Forms的Acroforms
description: 将Acroforms与AEM Forms集成的教程的第3部分。 在系统上测试工作流和自适应表单。
feature: Adaptive Forms
doc-type: Tutorial
version: Experience Manager 6.5
badgeIntegration: label="集成" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
duration: 45
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '228'
ht-degree: 3%

---


# 在您的系统上测试此功能

[下载此包并将其导入AEM](assets/acro-form-aem-form.zip)
此包中包含示例工作流和html页面，您可以通过该页面从上传的Acroform创建架构。

## 配置工作流

1. [在编辑模式下打开工作流模型](http://localhost:4502/editor.html/conf/global/settings/workflow/models/MergeAcroformData.html)。
2. 打开MergeAcroformData步骤的配置属性。
3. 单击“流程”选项卡。
4. 确保您传递的参数是服务器上的有效文件夹。
5. 保存更改。

## 创建自适应表单

1. 使用上一步中创建的架构创建自适应表单。
2. 将几个架构元素拖放到自适应表单上。
3. 配置自适应表单的提交操作以提交到AEM工作流(MergeAcroformData)。
4. **请确保将数据文件路径指定为“Data.xml”。 这一点非常重要，因为示例代码将在工作流有效负载中查找名为Data.xml的文件。**
5. 预览自适应表单、填写表单并提交。
6. 您应该会在配置工作流下看到PDF，其中合并的数据将保存到步骤4中指定的文件夹

>[!NOTE]
>
>通过将数据与acroform合并而生成的pdf将保存为pdfdocument.pdf，位于工作流的有效负荷文件夹下。 然后，可以将此文档用作工作流的一部分进行进一步处理
