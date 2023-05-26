---
title: 在HTM5表单提交时触发AEM工作流 — 让用例发挥作用
seo-title: Trigger AEM Workflow on HTML5 Form Submission
description: 继续以离线模式填写移动表单并提交移动表单以触发AEM Workflow
seo-description: Continue filling mobile form in offline mode and submit mobile form to trigger AEM workflow
feature: Mobile Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 79935ef0-bc73-4625-97dd-767d47a8b8bb
source-git-commit: 012850e3fa80021317f59384c57adf56d67f0280
workflow-type: tm+mt
source-wordcount: '452'
ht-degree: 1%

---

# 让此用例在您的系统上工作

>[!NOTE]
>
>要使示例资产在您的系统上工作，假定您分别在端口4502和4503上运行AEM创作和发布实例。 此外，还假设AEM作者可通过以下方式访问 `admin`/`admin`. 如果端口号或管理员密码已更改，则这些示例资源将无法工作。 您必须使用提供的示例代码创建自己的资产。

要使该用例在本地系统上正常工作，请执行以下步骤：

* 在端口4502上安装AEM创作实例，在端口4503上安装AEM发布实例
* [按照使用AEM Forms中的服务用户进行开发中指定的说明进行操作](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/adaptive-forms/service-user-tutorial-develop.html). 请确保创建服务用户并在您的AEM创作和发布实例上部署捆绑包。
* [打开osgi配置 ](http://localhost:4503/system/console/configMgr).
* 搜索  **Apache Sling引用过滤器**. 确保选中允许空复选框。
* [部署自定义AEMFormDocumentService捆绑包](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar).此捆绑包需要部署在AEM发布实例上。 此捆绑包具有从移动设备表单生成交互式PDF的代码。
* [下载并解压缩与本文相关的资产。](assets/offline-pdf-submission-assets.zip) 您将获得以下内容
   * **offline-submission-profile.zip**  — 此AEM包包含自定义配置文件，通过该配置文件可将交互式pdf下载到本地文件系统。 在AEM发布实例上部署此包。
   * **xdp-form-and-workflow.zip**  — 此AEM包包含XDP、示例工作流、在节点content/pdfsubmissions上配置的启动器。 在您的AEM创作实例和发布实例上部署此包。
   * **HandlePDFSubmission.HandlePDFSubmission.core-1.0-SNAPSHOT.jar**  — 这是执行大部分工作的AEM捆绑包。 此捆绑包包含装载在 `/bin/startworkflow`. 此servlet将提交的表单数据保存在 `/content/pdfsubmissions` AEM节点。 在您的AEM创作实例和发布实例上部署此捆绑包。
* [预览移动设备表单](http://localhost:4503/content/dam/formsanddocuments/testsubmision.xdp/jcr:content)
* 填写多个字段，然后单击工具栏上的按钮以下载交互式PDF。
* 使用Acrobat填写下载的PDF并单击“提交”按钮。
* 您应会收到一条成功消息
* 以管理员身份登录AEM创作实例
* [检查AEM收件箱](http://localhost:4502/aem/inbox)
* 您应该有工作项目来审核提交的PDF

>[!NOTE]
>
>一些客户没有将PDF提交到发布实例上运行的servlet，而是在servlet容器（如Tomcat）中部署了servlet。 这完全取决于客户熟悉的拓扑。在本教程中，我们将使用在发布实例上部署的servlet来处理pdf提交。
