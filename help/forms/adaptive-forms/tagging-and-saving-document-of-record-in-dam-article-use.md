---
title: 在DAM中标记和存储AEM FormsDoR
seo-title: 在DAM中标记和存储AEM FormsDoR
description: 本文将介绍AEM Forms在AEM DAM中生成的DoR的存储和标记用例。 文档的标记基于提交的表单数据完成。
seo-description: 本文将介绍AEM Forms在AEM DAM中生成的DoR的存储和标记用例。 文档的标记基于提交的表单数据完成。
uuid: b9ba13ed-52d5-4389-a7d5-bf85e58fea49
feature: adaptive-forms,workflow
topics: developing
audience: implementer
doc-type: article
activity: develop
version: 6.4,6.5
discoiquuid: 53961454-633b-4cd8-aef7-e64ab4e528e4
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '655'
ht-degree: 0%

---


# 在DAM {#tagging-and-storing-aem-forms-dor-in-dam}中标记和存储AEM FormsDoR

本文将介绍AEM Forms在AEM DAM中生成的DoR的存储和标记用例。 文档的标记基于提交的表单数据完成。

客户的常见要求是存储和标记AEM Forms在AEM DAM中生成的记录文档(DoR)。 文档的标记必须基于Forms自适应数据。 例如，如果提交数据中的雇佣状态为“已停用”，则我们要用“已停用”标签标记文档，并将文档存储在DAM中。

用例如下：

* 用户填写自适应表单。 在自适应形式中，捕获用户的婚姻状态（例如单人）和就业状态（例如已退休）。
* 提交表单时，将触发AEM工作流。 此工作流将文档与婚姻状态（单一）和雇佣状态（退休）标记，并将文档存储在DAM中。
* 文档存储在DAM中后，管理员应能够按这些标记搜索文档。 例如，在“单个”或“已停用”上搜索将获取相应的DoR。

为了满足此用例，编写了自定义流程步骤。 在此步骤中，我们从提交的数据中提取相应数据元素的值。 然后，我们使用此值构建标记拼贴。 例如，如果婚姻状态元素的值为“Single”，则标记标题将变为**Peak:EmploymentStatus/Single。 **使用TagManager API，我们找到标记并将标记应用于DoR。

下面的代码片断向您展示如何查找标记并将标记应用到文档。

```java
Tag tagFound = tagManager.resolveByTitle(tagTitle+xmlElement.getTextContent());
//tagTitle is "Peak:EmploymentStatus/" and the xmlElement.getTextContent() will return the value Single. So the tag title becomes Peak:EmploymentStatus/Single. Once the tag is found we put the tag in array and apply the tags to the resource as shown below
tagArray[i] = tagFound;
tagManager.setTags(metadata, tagArray, true);
```

要使此范例在您的系统上工作，请按照下面列出的步骤操作：
* [使用服务用户包部署开发](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [下载并部署setvalue捆绑包](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)。这是自定义OSGI捆绑包，用于从提交的表单数据设置标记。

* [下载示例自适应表单](assets/tag-and-store-in-dam-assets.zip)

* [转到Forms和文档](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)

* 单击创建 |文件上传并上传示例adaptiveform.zip

* [使用AEM包管理器](assets/tag-and-store-in-dam-assets.zip) 导入文章资源
* 以预览模式](http://localhost:4502/content/dam/formsanddocuments/summit/peakform/jcr:content?wcmmode=disabled)打开[示例表单。 填写“人员”部分并提交表单。
* [导航到DAM中的峰值文件夹](http://localhost:4502/assets.html/content/dam/Peak)。您应在峰值文件夹中看到DoR。 检查文档的属性。 它应该被正确标记。
祝贺你！! 您已成功在系统上安装示例

* 让我们浏览在提交表单时触发的[工作流](http://localhost:4502/editor.html/conf/global/settings/workflow/models/TagAndStoreDoRinDAM.html)。
* 工作流中的第一步是通过连接申请人姓名和居住地县来创建唯一的文件名。
* 工作流的第二步传递标记层次结构和需要标记的表单字段元素。 处理步骤从提交的数据中提取值并构建需要标记文档的标记标题。
* 如果要将DoR存储在DAM中的其他文件夹中，请使用以下屏幕截图中指定的配置属性指定文件夹位置。

其他两个参数特定于DoR和数据文件路径，如自适应表单提交选项中指定。 请确保在此处指定的值与您在自适应表单提交选项中指定的值相匹配。

![标记多](assets/tag_dor_service_configuration.gif)

