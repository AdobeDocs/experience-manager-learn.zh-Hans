---
title: CRXDE Lite
description: CRXDE Lite是用于调试AEM as a Cloud Service开发人员环境的经典但功能强大的工具。 CRXDE Lite提供了一套功能，可帮助进行调试，从而检查所有资源和属性、处理JCR的可变部分以及调查权限。
feature: Developer Tools
version: Cloud Service
doc-type: Tutorial
kt: KT-5481
thumbnail: kt-5481.jpg
topic: Development
role: Developer
level: Beginner
exl-id: f3f2c89f-6ec1-49d3-91c7-10a42b897780
duration: 125
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '613'
ht-degree: 0%

---

# 使用CRXDE Lite调试AEM as a Cloud Service

CRXDE Lite为&#x200B;__仅__&#x200B;在AEM as a Cloud Service开发环境(以及本地AEM SDK)中可用。

## 访问AEM Author上的CRXDE Lite

CRXDE Lite __仅限__&#x200B;可在AEM as a Cloud Service开发环境中访问，并且&#x200B;__不__&#x200B;在暂存或生产环境中可用。

要在AEM Author中访问CRXDE Lite，请执行以下操作：

1. 登录到AEM as a Cloud Service AEM Author服务。
1. 导航到工具>常规>CRXDE Lite

这将使用用于登录AEM Author的凭据和权限打开CRXDE Lite。

## 调试内容

CRXDE Lite提供对JCR的直接访问。 通过CRXDE Lite显示的内容受授予用户的权限限制，这意味着根据您的访问权限，您可能无法查看或修改JCR中的所有内容。

请注意，`/apps`、`/libs`和`/oak:index`是不可变的，这意味着它们不能由任何用户在运行时更改。 JCR中的这些位置只能通过代码部署进行修改。

+ 使用左侧导航窗格导航和操作JCR结构
+ 在左侧导航窗格中选择节点，将显示底部窗格中的node属性。
   + 可以从窗格中添加、删除或更改属性
+ 双击左侧导航中的文件节点，会在右上窗格中打开文件的内容
+ 点按左上角的“全部保存”按钮以保留所做的更改，或者点按“全部保存”旁边的向下箭头以还原任何未保存的更改。

![CRXDE Lite — 正在调试内容](./assets/crxde-lite/debugging-content.png)

在运行时通过CRXDE Lite对AEM as a Cloud Service开发环境中的可变内容进行更改必须谨慎完成。
直接通过CRXDE Lite对AEM所做的任何更改可能难以跟踪和治理。 根据需要，确保通过CRXDE Lite所做的更改能够返回到AEM项目的可变内容包(`ui.content`)并提交到Git，以确保问题得到解决。 理想情况下，所有应用程序内容更改都源自代码库并通过部署流入AEM，而不是通过CRXDE Lite直接对AEM进行更改。

### 调试访问控制

CRXDE Lite提供了一种在特定节点上测试和评估特定用户或组（亦即主体）的访问控制的方法。

要访问CRXDE Lite中的“Test Access Control（测试访问控制）”控制台，请导航至：

+ CRXDE Lite>工具>测试访问控制……

![CRXDE Lite — 测试访问控制](./assets/crxde-lite/permissions__test-access-control.png)

1. 使用路径字段，选择要计算的JCR路径
1. 使用“承担者”字段，选择要作为路径评估依据的用户或组
1. 点按测试按钮

结果显示如下：

+ __Path__&#x200B;重申已计算的路径
+ __主体__&#x200B;重申评估路径的用户或组
+ __承担者__&#x200B;列出了所选承担者所属的所有承担者。
   + 这有助于了解可通过继承提供权限的可传递组成员资格
+ 路径&#x200B;__上的__&#x200B;权限列出了所选主体在评估路径上的所有JCR权限

### 不支持的调试活动

以下是CRXDE Lite活动，该活动无法&#x200B;__在Debugger中执行__。

### 调试OSGi配置

无法通过CRXDE Lite查看部署的OSGi配置。 OSGi配置在AEM项目的`ui.apps`代码包的`/apps/example/config.xxx`处进行维护，但是在部署到AEM as a Cloud Service环境时，OSGi配置资源不会保留到JCR，因此不会通过CRXDE Lite显示。

请改用[Developer Console >配置](./developer-console.md#configurations)查看已部署的OSGi配置。
