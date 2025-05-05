---
title: 基于预填充核心组件的自适应表单
description: 了解如何使用数据预填充自适应表单
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Experience Manager as a Cloud Service
topic: Integrations
jira: KT-14675
duration: 23
exl-id: a94deebd-e86e-4360-b0ed-193f13197ee2
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '119'
ht-degree: 0%

---

# 测试解决方案

部署代码后，根据核心组件创建自适应表单。 将自适应表单与预填充服务关联，如下面的屏幕快照所示。
![预填充服务](assets/pre-fill-service.png)

每次渲染表单时，都将执行关联的预填充服务，并使用预填充服务返回的数据填充表单。

例如，要使用与guid **d815a2b3-5f4c-4422-8197-d0b73479bf0e**&#x200B;关联的数据预填充表单，将使用以下URL。
预填充服务中的代码将提取guid参数的值，并从数据源获取与guid关联的数据。

```html
http://localhost:4502/content/dam/formsanddocuments/contactus/jcr:content?wcmmode=disabled&guid=d815a2b3-5f4c-4422-8197-d0b73479bf0e
```
