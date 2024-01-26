---
title: CRXDE Lite
description: CRXDE Lite是用于调试AEMas a Cloud Service开发人员环境的经典但功能强大的工具。 CRXDE Lite提供了一套功能，可帮助进行调试，从而检查所有资源和属性、处理JCR的可变部分以及调查权限。
feature: Developer Tools
version: Cloud Service
doc-type: Tutorial
kt: KT-5481
thumbnail: kt-5481.jpg
topic: Development
role: Developer
level: Beginner
exl-id: f3f2c89f-6ec1-49d3-91c7-10a42b897780
duration: 157
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '613'
ht-degree: 0%

---

# 使用CRXDE Lite调试AEMas a Cloud Service

CRXDE Lite为 __仅__ 在AEMas a Cloud Service开发环境(以及本地AEM SDK)上可用。

## 访问AEM Author上的CRXDE Lite

CRXDE Lite为 __仅限__ 可在AEMas a Cloud Service开发环境中访问，并且 __非__ 在暂存或生产环境中可用。

要在AEM Author中访问CRXDE Lite，请执行以下操作：

1. 登录到AEMas a Cloud ServiceAEM Author服务。
1. 导航到工具>常规>CRXDE Lite

这将使用用于登录AEM Author的凭据和权限打开CRXDE Lite。

## 调试内容

CRXDE Lite提供对JCR的直接访问。 通过CRXDE Lite显示的内容受授予用户的权限限制，这意味着根据您的访问权限，您可能无法查看或修改JCR中的所有内容。

请注意 `/apps`， `/libs` 和 `/oak:index` 不可变，这意味着任何用户都不能在运行时更改它们。 JCR中的这些位置只能通过代码部署进行修改。

+ 使用左侧导航窗格导航和操作JCR结构
+ 在左侧导航窗格中选择节点，将显示底部窗格中的node属性。
   + 可以从窗格中添加、删除或更改属性
+ 双击左侧导航中的文件节点，会在右上窗格中打开文件的内容
+ 点按左上角的“全部保存”按钮以保留所做的更改，或者点按“全部保存”旁边的向下箭头以还原任何未保存的更改。

![CRXDE Lite — 调试内容](./assets/crxde-lite/debugging-content.png)

在运行时通过CRXDE Lite对AEMas a Cloud Service开发环境中的可变内容进行更改必须谨慎完成。
直接通过CRXDE Lite对AEM所做的任何更改可能难以跟踪和治理。 根据需要，确保通过CRXDE Lite所做的更改能够返回到AEM项目的可变内容包(`ui.content`)，并将其提交到Git，以确保问题得到解决。 理想情况下，所有应用程序内容更改都源自代码库并通过部署流入AEM，而不是通过CRXDE Lite直接对AEM进行更改。

### 调试访问控制

CRXDE Lite提供了一种在特定节点上测试和评估特定用户或组（亦即主体）的访问控制的方法。

要访问CRXDE Lite中的“Test Access Control（测试访问控制）”控制台，请导航至：

+ CRXDE Lite>工具>测试访问控制……

![CRXDE Lite — 测试访问控制](./assets/crxde-lite/permissions__test-access-control.png)

1. 使用路径字段，选择要计算的JCR路径
1. 使用“承担者”字段，选择要作为路径评估依据的用户或组
1. 点按测试按钮

结果显示如下：

+ __路径__ 重申已评估的路径
+ __主体__ 重申评估路径的用户或组
+ __主体__ 列出所选主体所属的所有主体。
   + 这有助于了解可通过继承提供权限的可传递组成员资格
+ __路径上的权限__ 列出所选承担者对所评估路径拥有的所有JCR权限

### 不支持的调试活动

以下调试活动可以 __非__ 将在CRXDE Lite中执行。

### 调试OSGi配置

无法通过CRXDE Lite查看部署的OSGi配置。 OSGi配置在AEM项目的 `ui.apps` 代码包位于 `/apps/example/config.xxx`但是，部署到AEMas a Cloud Service环境后，OSGi配置资源不会保留到JCR，因此不会通过CRXDE Lite显示。

请改用 [“开发人员控制台”>“配置”](./developer-console.md#configurations) 查看已部署的OSGi配置。
