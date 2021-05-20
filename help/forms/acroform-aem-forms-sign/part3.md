---
title: 带有AEM Forms的Acroforms
seo-title: 将自适应表单数据与Acroform合并
description: 将Acroforms与AEM Forms集成的教程第3部分。 在系统上测试工作流和自适应表单。
feature: 自适应表单
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4
source-git-commit: 451ca39511b52e90a44bba25c6739280f49a0aac
workflow-type: tm+mt
source-wordcount: '237'
ht-degree: 1%

---


# 在系统上测试此功能

[下载此包并将其导入AEMT此](assets/acro-form-aem-form.zip)
包包含示例工作流和html页面，通过该页面，您可以从上传的Acroform创建架构。

## 配置工作流

1. [在编辑模式下打开工作流模型](http://localhost:4502/editor.html/conf/global/settings/workflow/models/MergeAcroformData.html)。
2. 打开MergeAcroformData步骤的配置属性。
3. 单击“流程”(Process)选项卡。
4. 确保您传递的参数是服务器上的有效文件夹。
5. 保存更改。

## 创建自适应表单

1. 使用前面步骤中创建的架构创建自适应表单。
2. 将几个架构元素拖放到自适应表单中。
3. 配置自适应表单的提交操作以提交到AEM工作流(MergeAcroformData)。
4. **确保将数据文件路径指定为“Data.xml”。当示例代码在工作流负载中查找名为Data.xml的文件时，这一点非常重要。**
5. 预览自适应表单，填写表单并提交。
6. 您应会看到PDF，其中数据已合并保存到配置工作流步骤4中指定的文件夹中

>[!NOTE]
>
>通过将数据与Acroform合并而生成的PDF将另存为pdfdocument.pdf，位于工作流的有效负荷文件夹下。 随后，可以将本文档用于作为工作流的一部分进行进一步处理
