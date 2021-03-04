---
title: 在DAM中标记和存储AEM Forms DoR
seo-title: 在DAM中标记和存储AEM Forms DoR
description: 本文将介绍在AEM DAM中存储和标记AEM Forms生成的DoR的用例。 文档的标记是根据提交的表单数据完成的。
seo-description: 本文将介绍在AEM DAM中存储和标记AEM Forms生成的DoR的用例。 文档的标记是根据提交的表单数据完成的。
uuid: b9ba13ed-52d5-4389-a7d5-bf85e58fea49
feature: 自适应Forms，工作流
topics: developing
audience: implementer
doc-type: article
activity: develop
version: 6.4,6.5
discoiquuid: 53961454-633b-4cd8-aef7-e64ab4e528e4
topic: 开发
role: 开发人员
level: 富有经验
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '661'
ht-degree: 0%

---


# 在DAM {#tagging-and-storing-aem-forms-dor-in-dam}中标记和存储AEM Forms DoR

本文将介绍在AEM DAM中存储和标记AEM Forms生成的DoR的用例。 文档的标记是根据提交的表单数据完成的。

客户的常见要求是存储和标记AEM Forms在AEM DAM中生成的记录(DoR)文档。 文档的标记必须基于自适应Forms提交的数据。 例如，如果提交数据中的雇佣状态为“已退休”，则我们要用“已退休”标记文档，并将文档存储在DAM中。

用例如下：

* 用户填写自适应表单。 在自适应表单中，捕获用户的婚姻状态(ex Single)和就业状态(Ex Reliated)。
* 提交表单时，将触发AEM工作流。 此工作流将文档与婚姻状态（单一）和雇佣状态（退休）标记，并将文档存储在DAM中。
* 文档存储到DAM后，管理员应能够通过这些标记搜索文档。 例如，在“单个”或“已停用”上搜索将获取相应的DoR。

为满足此用例，已编写一个自定义进程步骤。 在此步骤中，我们从提交的数据中提取相应数据元素的值。 然后，我们使用此值构建标记拼贴。 例如，如果婚姻状态元素的值为“Single”，则标记标题将变为**Peak:EmploymentStatus/Single。 **使用TagManager API，我们找到标签并将标签应用到DoR。

下面的代码片断向您展示如何查找标记并将标记应用到文档。

```java
Tag tagFound = tagManager.resolveByTitle(tagTitle+xmlElement.getTextContent());
//tagTitle is "Peak:EmploymentStatus/" and the xmlElement.getTextContent() will return the value Single. So the tag title becomes Peak:EmploymentStatus/Single. Once the tag is found we put the tag in array and apply the tags to the resource as shown below
tagArray[i] = tagFound;
tagManager.setTags(metadata, tagArray, true);
```

要使此示例在您的系统上工作，请按照以下列出的步骤操作：
* [使用服务用户包部署开发](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [下载并部署setvalue包](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)。这是自定义OSGI捆绑包，用于从提交的表单数据设置标记。

* [下载示例自适应表单](assets/tag-and-store-in-dam-assets.zip)

* [转到Forms和文档](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)

* 单击创建 |文件上传并上传示例adaptiveform.zip

* [使用AEM包管](assets/tag-and-store-in-dam-assets.zip) 理器导入文章资源
* 以预览模式](http://localhost:4502/content/dam/formsanddocuments/summit/peakform/jcr:content?wcmmode=disabled)打开[示例表单。 填写“人员”部分并提交表单。
* [导航到DAM中的Peak文件夹](http://localhost:4502/assets.html/content/dam/Peak)。您应在Peak文件夹中看到DoR。 检查文档的属性。 它应当被正确标记。
祝贺你！! 您已成功在系统上安装该示例

* 让我们了解表单提交时触发的[工作流](http://localhost:4502/editor.html/conf/global/settings/workflow/models/TagAndStoreDoRinDAM.html)。
* 工作流中的第一步是通过连接申请人姓名和居住县来创建唯一的文件名。
* 工作流的第二步传递了标记层次结构和需要标记的表单字段元素。 该处理步骤从提交的数据中提取值并构建需要标记文档的标记标题。
* 如果要将DoR存储在DAM中的其他文件夹中，请使用以下屏幕快照中指定的配置属性指定文件夹位置。

另外两个参数特定于DoR和Data File Path（在自适应表单提交选项中指定）。 请确保在此处指定的值与您在自适应表单提交选项中指定的值相匹配。

![标记多](assets/tag_dor_service_configuration.gif)

