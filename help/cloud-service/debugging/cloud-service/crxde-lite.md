---
title: CRXDE Lite
description: CRXDE Lite是一款经典但功能强大的工具，可用于将AEM作为Cloud Service开发人员环境进行调试。 CRXDE Lite提供了一套功能，可帮助调试人员检查所有资源和属性、处理JCR的可变部分并调查权限。
feature: Developer Tools
topics: development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
kt: KT-5481
thumbnail: kt-5481.jpg
topic: Development
role: Developer
level: Beginner
exl-id: f3f2c89f-6ec1-49d3-91c7-10a42b897780
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '613'
ht-degree: 0%

---

# 将AEM作为Cloud Service进行CRXDE Lite

CRXDE Lite __仅__&#x200B;可在AEM作为Cloud Service开发环境(以及本地AEM SDK)中使用。

## 访问AEM创作CRXDE Lite

CRXDE Lite __仅__&#x200B;可在AEM作为Cloud Service开发环境访问，并且&#x200B;__不__&#x200B;可在暂存或生产环境中使用。

要访问AEM创作CRXDE Lite，请执行以下操作：

1. 以Cloud ServiceAEM创作服务的身份登录AEM。
1. 导航至工具>常规>CRXDE Lite

这将使用用于登录AEM作者的凭据和权限打开CRXDE Lite。

## 调试内容

CRXDE Lite提供对JCR的直接访问。 通过CRXDE Lite显示的内容受授予用户的权限的限制，这意味着您可能无法查看或修改JCR中的所有内容，具体取决于您的访问权限。

请注意，`/apps`、`/libs`和`/oak:index`不可更改，这意味着任何用户在运行时都无法更改它们。 JCR中的这些位置只能通过代码部署进行修改。

+ 使用左侧导航窗格导航和操作JCR结构
+ 在左侧导航窗格中选择节点时，会在底部窗格中显示节点属性。
   + 可以从窗格中添加、删除或更改属性
+ 双击左侧导航中的文件节点，在右上方窗格中打开文件的内容
+ 点按左上角的全部保存按钮以保留已更改，或点按全部保存旁边的向下箭头以还原任何未保存的更改。

![CRXDE Lite — 调试内容](./assets/crxde-lite/debugging-content.png)

在AEM as a Cloud Service开发环境中，通过CRXDE Lite在运行时对可变内容进行更改时，必须小心操作。
通过CRXDE Lite直接对AEM所做的任何更改都可能难以跟踪和管理。 根据需要，确保通过CRXDE Lite所做的更改可返回到AEM项目的可变内容包(`ui.content`)并提交到Git，以确保问题得以解决。 理想情况下，所有应用程序内容更改都源自代码库，并通过部署流入AEM，而不是通过CRXDE Lite直接对AEM进行更改。

### 调试访问控制

CRXDE Lite提供了一种方法来测试和评估特定用户或组（即主体）在特定节点上的访问控制。

要在CRXDE Lite中访问测试访问控制控制台，请导航到：

+ CRXDE Lite>工具>测试访问控制……

![CRXDE Lite — 测试访问控制](./assets/crxde-lite/permissions__test-access-control.png)

1. 使用“路径”字段，选择要评估的JCR路径
1. 使用“主体”(Principal)字段，选择用户或组以根据
1. 点按测试按钮

结果显示如下：

+ ____ 路径重申评估的路径
+ ____ Principal重申已评估路径的用户或组
+ ____ 主体将所选主体所属的所有主体全部分离。
   + 这有助于了解可通过继承提供权限的传递组成员关系
+ __路径上的__ 权限列出了所选主体在评估路径上拥有的所有JCR权限

### 不受支持的调试活动

以下是可以在CRXDE Lite中执行&#x200B;__不__&#x200B;的调试活动。

### 调试OSGi配置

已部署的OSGi配置无法通过CRXDE Lite进行审核。 OSGi配置在AEM Project的`ui.apps`代码包的`/apps/example/config.xxx`中进行维护，但是，在作为Cloud Service环境部署到AEM时，OSGi配置资源不会持久保留到JCR，因此不会通过CRXDE Lite显示。

请改为使用[开发人员控制台>配置](./developer-console.md#configurations)来查看已部署的OSGi配置。
