---
title: CRXDE Lite
description: 'CRXDE Lite是调试AEM作为Cloud Service开发者环境的经典而强大的工具。 CRXDE Lite提供一套功能，有助于调试检查所有资源和属性、处理JCR的可变部分和调查权限。 '
feature: null
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: KT-5481
thumbnail: kt-5481.jpg
translation-type: tm+mt
source-git-commit: 10d0970c1b0debfec7cf94cc106bcae414fb55f6
workflow-type: tm+mt
source-wordcount: '613'
ht-degree: 0%

---


# 将AEM作为Cloud Service进行CRXDE Lite

CRXDE Lite是&#x200B;__ONLY__，在AEM上作为Cloud Service开发环境(以及本地AEM SDK)可用。

## 访问AEM作者的CRXDE Lite

CRXDE Lite是&#x200B;__仅__&#x200B;可作为Cloud Service开发环境在AEM上访问的，并且&#x200B;__不__&#x200B;可用于舞台或生产环境。

要访问AEM作者的CRXDE Lite:

1. 以Cloud ServiceAEM作者服务的身份登录到AEM。
1. 导航到工具>常规>CRXDE Lite

这将打开使用用于登录AEM作者的凭据和权限的CRXDE Lite。

## 调试内容

CRXDE Lite提供对JCR的直接访问。 通过CRXDE Lite显示的内容受授予用户的权限限制，这意味着您可能无法查看或修改JCR中的所有内容，具体取决于您的访问权限。

请注意，`/apps`、`/libs`和`/oak:index`是不可变的，这意味着任何用户在运行时都不能更改它们。 JCR中的这些位置只能通过代码部署进行修改。

+ JCR结构使用左侧导航窗格进行导航和操作
+ 在左侧导航窗格中选择一个节点后，将在底部窗格中显示该节点属性。
   + 可以从窗格中添加、删除或更改属性
+ 多次单击左侧导航中的文件节点，在右上方窗格中打开文件的内容
+ 点按左上角的“全部保存”按钮以保留更改，或点按“全部保存”以恢复所有未保存的更改旁边的向下箭头。

![CRXDE Lite-调试内容](./assets/crxde-lite/debugging-content.png)

在AEM作为Cloud Service开发环境，通过CRXDE Lite在运行时对可变内容进行更改时必须小心。
通过CRXDE Lite直接对AEM所做的任何更改可能难以跟踪和管理。 根据需要，确保通过CRXDE Lite所做的更改返回AEM项目的可变内容包(`ui.content`)并提交到Git，以确保问题得到解决。 理想情况下，所有应用程序内容更改都源自代码库，并通过部署流入AEM，而不是通过CRXDE Lite直接对AEM进行更改。

### 调试访问控制

CRXDE Lite提供了一种对特定用户或组（又称主体）的特定节点测试和评估访问控制的方法。

要在CRXDE Lite中访问测试访问控制控制台，请导航到：

+ CRXDE Lite>工具>测试访问控制...

![CRXDE Lite-测试访问控制](./assets/crxde-lite/permissions__test-access-control.png)

1. 使用“路径”字段，选择要评估的JCR路径
1. 使用“承担者”字段，选择用户或用户组以根据
1. 点击“测试”按钮

结果如下：

+ __路__ 径重申评估的路径
+ __原__ 则重申评估路径的用户或组
+ __原__ 理将选定主体所属的所有主体全部保留。
   + 这有助于了解通过继承提供权限的传递组成员关系
+ __路径上__ 的权限列出所选主体在评估路径上具有的所有JCR权限

### 不支持的调试活动

以下是调试活动，这些CRXDE Lite不能执行&#x200B;____。

### 调试OSGi配置

已部署的OSGi配置无法通过CRXDE Lite进行审核。 OSGi配置在AEM Project的`ui.apps`代码包中保留在`/apps/example/config.xxx`，但在部署到AEM作为Cloud Service环境时，OSGi配置不会保留到JCR，因此无法通过CRXDE Lite显示。

请改用[开发人员控制台>配置](./developer-console.md#configurations)来查看已部署的OSGi配置。
