---
title: 将数据从PDF文件导入自适应表单
description: 本教程介绍如何通过导入PDF文件填充自适应表单
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
exl-id: f21753b2-f065-4556-add4-b1983fb57031
duration: 23
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '117'
ht-degree: 0%

---

# 部署示例资源

您可以部署示例资源，以便此解决方案在本地的AEM Forms实例上运行

* [导入客户端库和自定义组件，以通过包管理器上传PDF表单](./assets/client-libs-custom-component.zip)
* 使用OSGi Web控制台下载并部署该捆绑包[自定义文档服务捆绑包](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
* 使用OSGi Web控制台下载并部署该捆绑包 [使用服务用户捆绑包进行开发](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* 使用OSGi Web控制台下载并部署该捆绑包[从pdf文件导入数据](./assets/onlineToOffline.core-1.0.0-SNAPSHOT.jar)
* 添加条目 _**DevelopingWithServiceUser.core：getresourceresolver=data**_ 在 _**Apache Sling服务用户映射器服务**_ OSGi配置控制台
