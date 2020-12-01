---
title: AEM Forms
seo-title: 将自适应表单数据与Acroform合并
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4
translation-type: tm+mt
source-git-commit: defefc1451e2873e81cd81e3cccafa438aa062e3
workflow-type: tm+mt
source-wordcount: '218'
ht-degree: 1%

---


# 在系统上测试此功能

[下载此包并将其导入](assets/acro-form-aem-form.zip)
到AEMT其包中包含示例工作流和允许您从上传的Acrobat创建模式的html页。

## 配置工作流

1. [在编辑模式下打开工作流模型](http://localhost:4502/editor.html/conf/global/settings/workflow/models/MergeAcroformData.html)。
2. 打开MergeAcroformData步骤的配置属性。
3. 单击“进程”选项卡。
4. 确保您传递的参数是服务器上的有效文件夹。
5. 保存更改。

## 创建自适应表单

1. 使用在前一步骤中创建的模式创建自适应表单。
2. 将一些模式元素拖放到自适应表单中。
3. 配置自适应表单的提交操作以提交到AEM工作流(MergeAcroformData)。
4. **确保将数据文件路径指定为“Data.xml”。这很重要，因为示例代码在工作流有效负荷中查找名为Data.xml的文件。**
5. 预览自适应表单，填写表单并提交。
6. 您应当看到PDF，数据合并后保存到配置工作流程中步骤4中指定的文件夹

>[!NOTE]
>
>通过将数据与acroform合并而生成的pdf将保存为工作流的有效负荷文件夹下的pdfdocument.pdf。 此文档随后可用作工作流的一部分用于进一步处理
