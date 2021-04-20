---
title: 注册自定义资产类型
seo-title: 注册自定义资产类型
description: 启用自定义资源类型以在AEMForms门户中列出
seo-description: 启用自定义资源类型以在AEMForms门户中列出
uuid: eaf29eb0-a0f6-493e-b267-1c5c4ddbe6aa
feature: Adaptive Forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4,6.5
discoiquuid: 99944f44-0985-4320-b437-06c5adfc60a1
topic: Development
role: Developer
level: Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '671'
ht-degree: 2%

---


# 注册自定义资产类型{#registering-custom-asset-types}

启用自定义资源类型以在AEMForms门户中列出

>[!NOTE]
>
>确保已安装AEM 6.3(SP1)和相应的AEM Forms Add On。 此功能仅适用于AEM Forms 6.3 SP1及更高版本

## 指定基本路径{#specify-base-path}

基本路径是顶级存储库路径，其中包含用户可能希望在搜索和制表人组件中列表的所有资产。 如果需要，用户还可以从组件编辑对话框配置基本路径内的特定位置，以便在特定位置触发搜索而不是搜索基本路径内的所有节点。 默认情况下，除非用户从此位置配置一组特定路径，否则基本路径将用作提取资产的搜索路径条件。 要进行性能搜索，必须具有此路径的最佳值。 基本路径的默认值将保留为&#x200B;**_/content/dam/formsanddocuments_**，因为所有AEM Forms资产都驻留在&#x200B;**_/content/dam/formsanddocuments._**&#x200B;中

配置基本路径的步骤

1. 登录crx
1. 导航到&#x200B;**/libs/fd/fp/extensions/querybuilder/basepath**

1. 单击工具栏中的“叠加节点”
1. 确保叠加位置为“/apps/”
1. 单击确定
1. 单击“保存”
1. 导航到在&#x200B;**/apps/fd/fp/extensions/querybuilder/basepath**&#x200B;创建的新结构

1. 将路径属性的值更改为&#x200B;**&quot;/content/dam&quot;**
1. 单击“保存”

通过将path属性指定为&#x200B;**&quot;/content/dam&quot;**，您基本上将Base Path设置为/content/dam。 打开“搜索”和“制表人”组件即可验证这一点。

![basepath](assets/basepath.png)

## 注册自定义资源类型{#register-custom-asset-types}

我们在搜索和制表人组件中添加了一个新选项卡（资产列表）。 此选项卡将列表您配置的现成资产类型和其他资产类型。 默认情况下，会列出以下资产类型

1. 自适应表单
1. 表单模板
1. PDF forms
1. 文档（静态PDF）

**注册自定义资源类型的步骤**

1. 创建&#x200B;**/libs/fd/fp/extensions/querybuilder/assettypes**&#x200B;的叠加节点

1. 将叠加位置设置为“/apps”
1. 导航到在**/apps/fd/fp/extensions/querybuilder/assettypes **创建的新结构

1. 在此位置下，为要注册的类型创建一个“nt:unstructured”节点，将节点命名为&#x200B;**mp4文件。 向此mp4files节点**&#x200B;添加以下两个属性

   1. 添加jcr:title属性以指定资产类型的显示名称。 将jcr:title的值设置为“Mp4文件”。
   1. 添加“type”属性并将其值设置为“videos”。 这是我们在模板中用于列表视频类型资产的值。 保存更改。

1. 在mp4文件下创建“nt:unstructured”类型的节点。 将此节点命名为&quot;searchcriteria&quot;
1. 在搜索条件下添加一个或多个过滤器。 假设如果用户希望具有搜索过滤器来列表MIME类型为“video/mp4”的mp4Files，您可以在此处执行此操作
1. 在节点搜索条件下创建类型为“nt:unstructured”的节点。 将此节点命名为&quot;filetypes&quot;
1. 向此“filetypes”节点添加以下2个属性

   1. name: ./jcr:content/metadata/dc:format
   1. value:video/mp4

1. 这意味着，具有与video/mp4相等属性dc:format的资产将被视为资产类型“Mp4视频”。 您可以将“jcr:content/metadata”节点上列出的任何属性用作搜索条件

1. **确保保存您的工作**

执行上述步骤后，新的资产类型（Mp4文件）将开始显示在“搜索”和“制表人”组件的资产类型下拉列表中，如下所示

![mp4文件](assets/mp4files.png)

[如果在使其正常工作时遇到问题，可以导入以下包。](assets/assettypeskt1.zip) 包定义了两种自定义资产类型。Mp4文件和Word文档。 建议您查看&#x200B;**/apps/fd/fp/extensions/querybuilder/assettypes**

[安装customeportal包](assets/customportalpage.zip)。此包包含示例门户页面。 本页将在本教程的第2部分中使用

