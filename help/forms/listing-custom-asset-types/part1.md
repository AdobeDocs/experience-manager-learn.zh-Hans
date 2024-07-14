---
title: 注册自定义资源类型
description: 在AEMForms Portal中启用自定义资源类型以列出
feature: Adaptive Forms
doc-type: Tutorial
version: 6.4,6.5
discoiquuid: 99944f44-0985-4320-b437-06c5adfc60a1
topic: Development
role: Developer
level: Experienced
exl-id: da613092-e03b-467c-9b9e-668142df4634
last-substantial-update: 2019-07-11T00:00:00Z
duration: 129
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '644'
ht-degree: 0%

---

# 注册自定义资源类型 {#registering-custom-asset-types}

在AEMForms Portal中启用自定义资源类型以列出

>[!NOTE]
>
>确保您已安装带有SP1的AEM 6.3以及相应的AEM Forms附加组件。 此功能仅适用于AEM Forms 6.3 SP1及更高版本

## 指定基本路径 {#specify-base-path}

基本路径是顶级存储库路径，包含用户可能希望在搜索和列表程序组件中列出的所有资产。 如果需要，用户还可以通过组件编辑对话框配置基本路径内的特定位置，以便在特定位置触发搜索，而不是搜索基本路径内的所有节点。 默认情况下，将基本路径用作获取资产的搜索路径条件，除非用户从该位置中配置一组特定路径。 为了进行性能搜索，必须使此路径具有最优值。 基础路径的默认值将保留为&#x200B;**_/content/dam/formsanddocuments_**，因为所有AEM Forms资产都位于&#x200B;**_/content/dam/formsanddocuments._**

配置基本路径的步骤

1. 登录crx
1. 导航到&#x200B;**/libs/fd/fp/extensions/querybuilder/basepath**

1. 单击工具栏中的“覆盖节点”
1. 确保叠加位置为“/apps/”
1. 单击确定
1. 单击“保存”
1. 导航到&#x200B;**/apps/fd/fp/extensions/querybuilder/basepath**&#x200B;中创建的新结构

1. 将path属性的值更改为&#x200B;**&quot;/content/dam&quot;**
1. 单击“保存”

通过将path属性指定为&#x200B;**&quot;/content/dam&quot;**，您基本上是将Base Path设置为/content/dam。 可以通过打开“搜索和列表程序”组件来验证这一点。

![基本路径](assets/basepath.png)

## 注册自定义资源类型 {#register-custom-asset-types}

我们在搜索和列表程序组件中添加了一个新选项卡（资产列表）。 此选项卡将列出现成的资源类型以及您配置的其他资源类型。 默认情况下，将列出以下资源类型

1. 自适应表单
1. 表单模板
1. PDF forms
1. 文档(静态PDF)

**注册自定义资源类型的步骤**

1. 创建&#x200B;**/libs/fd/fp/extensions/querybuilder/assettypes**&#x200B;的覆盖节点

1. 将覆盖位置设置为“/apps”
1. 导航到在`/apps/fd/fp/extensions/querybuilder/assettypes`创建的新结构

1. 在此位置下，为要注册的类型创建一个“nt：unstructured”节点，为节点&#x200B;**mp4files命名。 将以下两个属性添加到此mp4files节点**

   1. 添加jcr：title属性以指定资源类型的显示名称。 将jcr：title的值设置为“Mp4文件”。
   1. 添加“type”属性并将其值设置为“videos”。 我们在模板中使用此值来列出视频类型的资产。 保存更改。

1. 在mp4files下创建“nt：unstructured”类型的节点。 将此节点命名为“searchcriteria”
1. 在搜索条件下添加一个或多个筛选器。 假设，如果用户希望有一个搜索过滤器来列出mime类型为“video/mp4”的mp4文件，则可以在此处进行列出
1. 在节点搜索标准下创建“nt：unstructured”类型的节点。 将此节点命名为“文件类型”
1. 将以下2个属性添加到此“文件类型”节点

   1. 名称： 。/jcr：content/metadata/dc：format
   1. 值： video/mp4

1. 这意味着属性dc：format等于video/mp4的资源被视为资源类型“Mp4视频”。 您可以使用“jcr：content/metadata”节点中列出的任何属性作为搜索条件

1. **确保保存您所做的工作**

执行上述步骤后，新的资源类型（Mp4文件）将开始显示在“搜索和列表程序”组件的资源类型下拉列表中，如下所示

![mp4files](assets/mp4files.png)

[如果您在使此功能生效时遇到问题，可以导入以下包。](assets/assettypeskt1.zip)包定义了两种自定义资源类型。 Mp4文件和Worddocuments。 建议您查看&#x200B;**/apps/fd/fp/extensions/querybuilder/assettypes**

[安装customeportal包](assets/customportalpage.zip)。 此包中包含示例门户页面。 本教程的第2部分将使用此页面
