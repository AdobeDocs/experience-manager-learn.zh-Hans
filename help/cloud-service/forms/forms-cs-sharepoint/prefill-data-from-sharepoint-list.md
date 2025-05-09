---
title: 使用SharePoint列表中的数据预填充自适应表单
description: 了解如何使用由共享点列表支持的表单数据模型预填自适应表单
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Experience Manager as a Cloud Service
topic: Integrations
jira: KT-14795
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
duration: 46
exl-id: 9abe9f9d-8fb3-4e01-a830-1dad1c27274d
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '240'
ht-degree: 0%

---

# 使用共享点列表数据预填充自适应表单

在以前版本的AEM Form(6.5)中，必须写入自定义代码以使用请求属性预填充表单数据模型支持的自适应表单。 在AEM Forms as a Cloud Service中，不再需要编写自定义代码。

本文介绍了使用表单数据模型预填充服务使用从SharePoint列表中获取的数据预填充/预填充自适应表单所需的步骤。

本文假设您已[成功配置自适应表单以将数据提交到SharePoint列表。](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/forms/adaptive-forms-authoring/authoring-adaptive-forms-core-components/create-an-adaptive-form-on-forms-cs/configure-submit-actions-core-components.html?lang=zh-Hans#connect-af-sharepoint-list)

以下是SharePoint列表中的数据
![sharepoint-list](assets/list-data.png)

要使用与特定guid关联的数据预填充自适应表单，需要执行以下步骤

## 配置get服务

* 使用guid属性为表单数据模型的顶级对象创建get服务
  ![获取服务](assets/mapping-request-attribute.png)

在此屏幕快照中，GUID列通过名为`submissionid`的请求属性进行绑定。

完全配置的get服务如下所示

![获取服务](assets/fdm-request-attribute.png)

## 配置自适应表单以使用表单数据模型预填充服务

* 打开基于共享点列表表单数据模型的自适应表单。 关联表单数据模型预填充服务
  ![表单预填充服务](assets/form-prefill-service.png)

## 测试表单

通过在URL中包含`submissionid`预览表单，如下所示

```html
http://localhost:4502/content/dam/formsanddocuments/contactusform/jcr:content?wcmmode=disabled&submissionid=57e12249-751a-4a38-a81f-0a4422b24412
```
