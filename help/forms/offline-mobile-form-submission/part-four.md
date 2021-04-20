---
title: 在HTM5表单提交时触发AEM工作流
seo-title: 在HTML5表单提交时触发AEM Workflow
description: 在脱机模式下继续填写移动表单并提交移动表单以触发AEM工作流
seo-description: 在脱机模式下继续填写移动表单并提交移动表单以触发AEM工作流
feature: Mobile Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '476'
ht-degree: 1%

---


# 让此用例在您的系统上工作

>[!NOTE]
>
>要使示例资产在您的系统上工作，假定您的AEM作者实例和发布实例分别在端口4502和4503上运行。 还假定可通过`admin`/`admin`访问AEM作者。 如果端口号或管理员密码已更改，则这些示例资产将无法工作。 您必须使用提供的示例代码创建自己的资产。

要使此用例在本地系统上工作，请执行以下步骤：

* 在端口4502上安装AEM作者实例，在端口4503上安装AEM发布实例
* [按照在AEM Forms中与服务用户一起开发时指定的说明进行操作](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/adaptive-forms/service-user-tutorial-develop.html)。请确保创建服务用户并在AEM作者实例和发布实例上部署捆绑包。
* [打开OSGI配置 ](http://localhost:4503/system/console/configMgr)。
* 搜索&#x200B;**Apache Sling推荐人筛选器**。 确保选中“允许空”复选框。
* [部署自定义AEMFormDocumentService Bundle](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)。需要在AEM Publish实例上部署此捆绑包。此捆绑包包含从移动表单生成交互式PDF的代码。
* [下载并解压缩与本文相关的资产。](assets/offline-pdf-submission-assets.zip) 您将获得以下
   * **offline-submission-用户档案.zip**  — 此AEM包中包含自定义用户档案，允许您将交互式pdf下载到本地文件系统。在AEM发布实例上部署此包。
   * **xdp-form-and-workflow.zip**  — 此AEM包包含XDP、示例工作流、在节点内容/pdf提交上配置的启动程序。在AEM作者实例和发布实例上部署此包。
   * **HandlePDFSubmission.HandlePDFSubmission.core-1.0-SNAPSHOT** .jar — 这是执行大部分工作的AEM包。此捆绑包包含装在`/bin/startworkflow`上的servlet。 此servlet将提交的表单数据保存在AEM存储库的`/content/pdfsubmissions`节点下。 在AEM作者实例和发布实例上部署此捆绑包。
* [预览移动表单](http://localhost:4503/content/dam/formsanddocuments/testsubmision.xdp/jcr:content)
* 填写多个字段，然后单击工具栏上的按钮以下载交互式PDF。
* 使用Acrobat填写下载的PDF并点击“提交”按钮。
* 您应收到成功消息
* 以管理员身份登录到AEM作者实例
* [检查AEM收件箱](http://localhost:4502/aem/inbox)
* 您应该有一个工作项来审阅提交的PDF

>[!NOTE]
>
>一些客户已在Servlet容器（如Tomcat）中部署了Servlet，而不是将PDF提交到发布实例上运行的Servlet。 这取决于客户熟悉的拓扑。为了本教程的目的，我们将使用发布实例上部署的servlet来处理pdf提交。

