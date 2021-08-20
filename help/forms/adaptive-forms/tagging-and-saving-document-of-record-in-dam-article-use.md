---
title: 在DAM中标记和存储AEM Forms DoR
description: 本文将介绍在AEM DAM中存储和标记AEM Forms生成的DoR的用例。 文档的标记是根据提交的表单数据完成的。
feature: 自适应表单
version: 6.4,6.5
topic: 开发
role: Developer
level: Experienced
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '616'
ht-degree: 0%

---


# 在DAM中标记和存储AEM Forms DoR {#tagging-and-storing-aem-forms-dor-in-dam}

本文将介绍在AEM DAM中存储和标记AEM Forms生成的DoR的用例。 文档的标记是根据提交的表单数据完成的。

客户的常见要求是在AEM DAM中存储并标记AEM Forms生成的记录文档(DoR)。 文档的标记必须基于自适应Forms提交的数据。 例如，如果提交数据中的雇佣状态为“已停用”，则我们希望使用“已停用”标记来标记文档，并将该文档存储在DAM中。

用例如下：

* 用户填写自适应表单。 在自适应表单中，捕获用户的婚姻状态（前单身）和就业状态（前已退休）。
* 提交表单时，会触发AEM工作流。 此工作流会使用婚姻状态（单个）和雇佣状态（已停用）标记文档，并将文档存储在DAM中。
* 文档存储到DAM中后，管理员应能够通过这些标记来搜索文档。 例如，在“单个”或“已停用”上搜索将获取相应的DoR。

为了满足此用例的要求，编写了自定义流程步骤。 在此步骤中，我们将从提交的数据中获取相应数据元素的值。 然后，使用此值构建标记拼贴。 例如，如果婚姻状态元素的值为“Single”，则标记标题将变为**Peak:EmploymentStatus/Single。 **使用TagManager API ，我们会找到标记并将标记应用到DoR。

以下代码片段显示了如何查找标记并将标记应用到文档。

```java
Tag tagFound = tagManager.resolveByTitle(tagTitle+xmlElement.getTextContent());
//tagTitle is "Peak:EmploymentStatus/" and the xmlElement.getTextContent() will return the value Single. So the tag title becomes Peak:EmploymentStatus/Single. Once the tag is found we put the tag in array and apply the tags to the resource as shown below
tagArray[i] = tagFound;
tagManager.setTags(metadata, tagArray, true);
```

要使此示例在您的系统上工作，请按照下面列出的步骤操作：
* [部署Developmingwithserviceuser包](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [下载并部署setvalue包](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)。这是自定义OSGI包，用于从提交的表单数据中设置标记。

* [下载示例自适应表单](assets/tag-and-store-in-dam-assets.zip)

* [转到Forms和文档](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)

* 单击Create |文件上传和上传示例adaptiveform.zip

* [使用AEM包管](assets/tag-and-store-in-dam-assets.zip) 理器导入文章资产
* 在预览模式下打开[示例表单](http://localhost:4502/content/dam/formsanddocuments/summit/peakform/jcr:content?wcmmode=disabled)。 填写人员部分并提交表单。
* [导航到DAM中的“峰值”文件夹](http://localhost:4502/assets.html/content/dam/Peak)。您应会在Peak文件夹中看到DoR。 检查文档的属性。 应正确标记。
恭喜你!! 您已成功在系统上安装示例

* 让我们来探索在提交表单时触发的[工作流](http://localhost:4502/editor.html/conf/global/settings/workflow/models/TagAndStoreDoRinDAM.html)。
* 工作流中的第一步是通过关联申请人姓名和居住县来创建唯一的文件名。
* 工作流的第二步传递标记层次结构和需要标记的表单字段元素。 流程步骤从提交的数据中提取值，并构建需要标记文档的标记标题。
* 如果要将DoR存储在DAM的其他文件夹中，请使用下面屏幕截图中指定的配置属性指定文件夹位置。

其他两个参数特定于“自适应表单提交”选项中指定的“DoR”和“数据文件路径”。 请确保此处指定的值与自适应表单提交选项中指定的值匹配。

![标记多尔](assets/tag_dor_service_configuration.gif)

