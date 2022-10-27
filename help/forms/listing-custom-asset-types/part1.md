---
title: 注册自定义资产类型
seo-title: Registering Custom Asset Types
description: 在AEMForms门户中启用自定义资产类型以将其列出
seo-description: Enabling custom asset types for listing in AEMForms Portal
uuid: eaf29eb0-a0f6-493e-b267-1c5c4ddbe6aa
feature: Adaptive Forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 99944f44-0985-4320-b437-06c5adfc60a1
topic: Development
role: Developer
level: Experienced
exl-id: da613092-e03b-467c-9b9e-668142df4634
last-substantial-update: 2019-07-11T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '645'
ht-degree: 2%

---

# 注册自定义资产类型 {#registering-custom-asset-types}

在AEMForms门户中启用自定义资产类型以将其列出

>[!NOTE]
>
>确保已安装带有SP1的AEM 6.3和相应的AEM Forms Add On。 此功能仅适用于AEM Forms 6.3 SP1及更高版本

## 指定基本路径 {#specify-base-path}

基本路径是顶级存储库路径，其中包含用户可能希望在搜索和列表组件中列出的所有资产。 如果需要，用户还可以从组件编辑对话框配置基本路径内的特定位置，以便在特定位置上触发搜索，而不是搜索基本路径内的所有节点。 默认情况下，基本路径用作获取资产的搜索路径条件，除非用户从此位置配置一组特定路径。 要进行性能搜索，必须具有此路径的最佳值。 基本路径的默认值将保持为 **_/content/dam/formsanddocuments_** 因为所有AEM Forms资产都位于 **_/content/dam/formsanddocuments。_**

配置基本路径的步骤

1. 登录到crx
1. 导航到 **/libs/fd/fp/extensions/querybuilder/basepath**

1. 单击工具栏中的“覆盖节点”
1. 确保叠加位置为“/apps/”
1. 单击确定
1. 单击“保存”
1. 导航到在中创建的新结构 **/apps/fd/fp/extensions/querybuilder/basepath**

1. 将path属性的值更改为 **&quot;/content/dam&quot;**
1. 单击“保存”

通过指定路径属性 **&quot;/content/dam&quot;** 您基本上是在将基本路径设置为/content/dam。 可通过打开Search和Lister组件来验证这一点。

![basepath](assets/basepath.png)

## 注册自定义资产类型 {#register-custom-asset-types}

我们在搜索和列表组件中添加了新选项卡（资产列表）。 此选项卡将列出您配置的现成资产类型和其他资产类型。 默认情况下，会列出以下资产类型

1. 自适应表单
1. 表单模板
1. PDF forms
1. 文档(静态PDF)

**注册自定义资产类型的步骤**

1. 创建的叠加节点 **/libs/fd/fp/extensions/querybuilder/assettypes**

1. 将叠加位置设置为“/apps”
1. 导航到在中创建的新结构 `/apps/fd/fp/extensions/querybuilder/assettypes`

1. 在此位置下，为要注册的类型创建一个“nt:unstructured”节点，并命名该节点 **mp4文件。 将以下两个属性添加到此mp4files节点**

   1. 添加jcr:title属性以指定资产类型的显示名称。 将jcr:title的值设置为“Mp4文件”。
   1. 添加“type”属性，并将其值设置为“videos”。 这是我们在模板中用于列出视频类型资产的值。 保存更改。

1. 在mp4文件下创建“nt:unstructured”类型的节点。 将此节点命名为“searchcriteria”
1. 在搜索条件下添加一个或多个过滤器。 假设，如果用户希望有一个搜索过滤器来列出mime类型为“video/mp4”的mp4Files，您可以在此处执行此操作
1. 在节点搜索条件下创建类型为“nt:unstructured”的节点。 将此节点命名为“filetypes”
1. 将以下2个属性添加到此“filetypes”节点

   1. name: ./jcr:content/metadata/dc:format
   1. 值：video/mp4

1. 这意味着，属性dc:format等于video/mp4的资产被视为资产类型“Mp4视频”。 您可以将“jcr:content/metadata”节点上列出的任何属性用于搜索条件

1. **确保保存您的工作**

执行上述步骤后，新的资产类型（Mp4文件）将开始显示在搜索组件和制表人组件的资产类型下拉列表中，如下所示

![mp4文件](assets/mp4files.png)

[如果在使其正常工作时遇到问题，可以导入以下包。](assets/assettypeskt1.zip) 资源包定义了两种自定义资产类型。 Mp4文件和Worddocuments。 建议您查看 **/apps/fd/fp/extensions/querybuilder/assettypes**

[安装customportal包](assets/customportalpage.zip). 此包包含示例门户页面。 本教程第2部分将使用此页面
