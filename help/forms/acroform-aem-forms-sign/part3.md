---
title: 包含AEM Forms的Acroforms
description: 将Acroforms与AEM Forms集成的教程的第3部分。 在系统上测试工作流和自适应表单。
feature: adaptive-forms
doc-type: Tutorial
version: 6.5
badgeIntegration: label="集成" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
duration: 67
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '228'
ht-degree: 1%

---


# 在您的系统上测试此功能

[下载此包并将其导入AEM](assets/acro-form-aem-form.zip)
此包中包含示例工作流和html页面，您可以通过该页面从上传的Acroform创建架构。

## 配置工作流

1. [在编辑模式下打开工作流模型](http://localhost:4502/editor.html/conf/global/settings/workflow/models/MergeAcroformData.html).
2. 打开MergeAcroformData步骤的配置属性。
3. 单击“流程”选项卡。
4. 确保您传递的参数是服务器上的有效文件夹。
5. 保存更改。

## 创建自适应表单

1. 使用上一步中创建的架构创建自适应表单。
2. 将几个架构元素拖放到自适应表单上。
3. 配置自适应表单的提交操作以提交到AEM Workflow (MergeAcroformData)。
4. **确保将数据文件路径指定为“Data.xml”。 这一点非常重要，因为示例代码将在工作流有效载荷中查找名为Data.xml的文件。**
5. 预览自适应表单、填写表单并提交。
6. 您应会在配置工作流下看到与合并的数据一起保存到PDF步骤4中指定的文件夹中的数据

>[!NOTE]
>
>通过将数据与acroform合并而生成的pdf将保存为pdfdocument.pdf，位于工作流的有效负荷文件夹下。 然后，可以将此文档用作工作流的一部分进行进一步处理
