---
title: 将数据从PDF文件导入自适应表单
description: 本教程介绍如何通过导入PDF文件填充自适应表单
feature: Adaptive Forms
version: Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
exl-id: f21753b2-f065-4556-add4-b1983fb57031
duration: 21
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '117'
ht-degree: 0%

---

# 部署示例资源

您可以部署示例资源，以便此解决方案在本地的AEM Forms实例上运行

* [导入客户端库和自定义组件，以通过包管理器上传PDF表单](./assets/client-libs-custom-component.zip)
* 使用OSGi Web控制台[自定义文档服务捆绑包](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)下载并部署该捆绑包
* 使用OSGi Web控制台[使用服务用户包进行开发](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)下载并部署该包
* 使用OSGi Web控制台[从pdf文件导入数据](./assets/onlineToOffline.core-1.0.0-SNAPSHOT.jar)下载并部署捆绑包
* 在&#x200B;_**Apache Sling服务用户映射器服务**_ OSGi配置控制台中添加条目&#x200B;_**DevelopingWithServiceUser.core：getresourceresolver=data**_
