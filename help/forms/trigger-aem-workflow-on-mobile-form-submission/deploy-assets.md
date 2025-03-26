---
title: 在HTML5表单提交时触发AEM工作流程 — 让用例发挥作用
description: 在本地系统上部署示例资源
feature: Mobile Forms
doc-type: article
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
jira: kt-16215
badgeVersions: label="AEM Forms 6.5" before-title="false"
exl-id: 9417235f-2e8d-45c7-86eb-104478a69a19
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '392'
ht-degree: 0%

---

# 让此用例在您的系统上正常工作

>[!NOTE]
>
>对于要在您的系统上工作的示例资源，假定您有权访问AEM Forms创作和AEM Forms发布实例。

要使此用例在本地系统上正常工作，请执行以下步骤：

## 在您的AEM Forms创作实例上部署以下内容

* [安装MobileFormToWorkflow捆绑包](assets/MobileFormToWorkflow.core-1.0.0-SNAPSHOT.jar)

* [部署使用服务进行开发的用户包](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/developingwithserviceuser.zip?lang=en)
使用configMgr在Apache Sling服务用户映射器服务中添加以下条目

```
DevelopingWithServiceUser.core:getformsresourceresolver=fd-service
```

* 通过使用[configMgr](http://localhost:4502/system/console/configMg)在AEM服务器凭据配置中指定文件夹名称，可以将表单提交存储在其他文件夹中。 如果更改文件夹，请确保在该文件夹上创建启动器以触发工作流&#x200B;**ReviewSubmittedPDF**

![config-author](assets/author-config.png)
* [使用包管理器](assets/xdp-form-and-workflow.zip)导入示例xdp和工作流包。


## 在发布实例上部署以下资源

* [安装MobileFormToWorkflow捆绑包](assets/MobileFormToWorkflow.core-1.0.0-SNAPSHOT.jar)

* 为创作实例指定用户名/密码，并在AEM存储库&#x200B;**中指定**&#x200B;现有位置，以使用[configMgr](http://localhost:4503/system/console/configMgr)在AEM Server凭据中存储提交的数据。 您可以将AEM Workflow Server上端点的URL保持原样。 这是从指定节点中的提交提取并存储数据的端点。
  ![发布配置](assets/publish-config.png)

* [部署使用服务进行开发的用户包](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/developingwithserviceuser.zip?lang=en)
* [打开osgi配置](http://localhost:4503/system/console/configMgr)。
* 搜索&#x200B;**Apache Sling引用过滤器**。 确保选中允许空复选框。


## 测试解决方案

* 登录您的创作实例
* [编辑w9.xdp](http://localhost:4502/libs/fd/fm/gui/content/forms/formmetadataeditor.html/content/dam/formsanddocuments/w9.xdp)的高级属性。 请确保正确设置了提交url和渲染配置文件，如下所示。
  ![xdp-advanced-properties](assets/mobile-form-properties.png)

* 发布w9.xdp
* 登录发布实例
* [预览w9表单](http://localhost:4503/content/dam/formsanddocuments/w9.xdp/jcr:content)
* 填写一些表单字段并提交表单
* 以管理员身份登录AEM创作实例
* [检查AEM收件箱](http://localhost:4502/aem/inbox)
* 您应该使用工作项目来审核提交的PDF

>[!NOTE]
>
>一些客户不是将PDF提交到在发布实例上运行的servlet，而是在servlet容器（如Tomcat）中部署了servlet。 这完全取决于客户熟悉的拓扑。在本教程中，我们将使用在发布实例上部署的servlet来处理表单提交。
