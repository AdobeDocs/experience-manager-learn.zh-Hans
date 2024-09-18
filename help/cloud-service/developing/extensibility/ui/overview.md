---
title: AEM UI可扩展性
description: 了解使用App Builder创建扩展的AEM UI可扩展性。
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay
last-substantial-update: 2023-06-02T00:00:00Z
exl-id: 73f5d90d-e007-41a0-9bb3-b8f36a9b1547
duration: 50
source-git-commit: 12d7f8f0afc1c19f289c847771cb9f4f965c650c
workflow-type: tm+mt
source-wordcount: '275'
ht-degree: 0%

---

# AEM UI可扩展性 {#aem-ui-extensibility}

Adobe Experience Manager (AEM)提供了强大的用户界面(UI)来创建数字体验。 为了自定义和扩展UI，Adobe引入了App Builder。 该工具使开发人员无需使用JavaScript和React进行复杂编码即可增强用户体验。

App Builder提供了一个实施层，用于创建绑定到AEM中明确定义扩展点的扩展。 App Builder与AEM无缝集成，允许实时预览和测试。 将更改部署到AEM既快速又简化。 通过使用App Builder，开发人员可以节省时间和精力，实现快速原型制作并与利益相关者协作。

>[!CONTEXTUALHELP]
>id="aemcloud_learn_extensibility_app_builder"
>title="Adobe Developer App Builder和AEM Headless快速入门"
>abstract="了解AEM App Builder如何让开发人员通过JavaScript和React快速自定义和扩展AEM UI，从而支持无缝集成和快速部署。"

## 开发AEM用户界面扩展

AEM的各种UI具有不同的扩展点，但基本概念是相同的。

以下链接的视频和演练展示了如何使用内容片段控制台扩展来说明各种活动。 但是，请务必注意，所涵盖的概念可应用于所有AEM UI扩展。

1. [创建Adobe Developer Console项目](./adobe-developer-console-project.md)
1. [初始化扩展](./app-initialization.md)
1. [注册扩展](./extension-registration.md)
1. 实施扩展点

   扩展点及其实施因扩展的UI而异。

   + [开发内容片段UI扩展](./content-fragments/overview.md)

1. [开发模态](./modal.md)
1. [开发Adobe I/O Runtime操作](./runtime-action.md)
1. [验证扩展](./verify.md)
1. [部署扩展](./deploy.md)

## Adobe Developer文档

Adobe Developer包含有关AEM UI可扩展性的开发人员详细信息。 请查看[Adobe Developer内容以获取更多技术详细信息](https://developer.adobe.com/uix/docs/)。
