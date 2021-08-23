---
title: 在HTM5表单提交时触发AEM工作流
seo-title: 在HTML5表单提交时触发AEM工作流
description: 在离线模式下继续填写移动表单，并提交移动表单以触发AEM工作流
seo-description: 在离线模式下继续填写移动表单，并提交移动表单以触发AEM工作流
feature: 移动设备表单
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: 开发
role: Developer
level: Experienced
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '472'
ht-degree: 1%

---


# 使此用例在您的系统上正常工作

>[!NOTE]
>
>要使示例资产在您的系统上工作，假定AEM创作实例和发布实例分别在端口4502和4503上运行。 此外，还假定AEM作者可通过`admin`/`admin`访问。 如果端口号或管理员密码已更改，则这些示例资产将无法正常工作。 您必须使用提供的示例代码创建自己的资产。

要使此用例在本地系统上工作，请执行以下步骤：

* 在端口4502上安装AEM创作实例，在端口4503上安装AEM发布实例
* [按照在AEM Forms中使用服务用户进行开发中指定的说明进行操作](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/adaptive-forms/service-user-tutorial-develop.html)。请确保创建服务用户并在AEM创作和发布实例上部署包。
* [打开osgi配置 ](http://localhost:4503/system/console/configMgr)。
* 搜索&#x200B;**Apache Sling反向链接过滤器**。 确保选中允许空复选框。
* [部署自定义AEMFormDocumentService包](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)。此包需要部署在AEM发布实例上。此捆绑包具有用于从移动设备表单生成交互式PDF的代码。
* [下载并解压缩与本文相关的资产。](assets/offline-pdf-submission-assets.zip) 您将获得以下信息
   * **offline-submission-profile.zip**  — 此AEM包中包含自定义配置文件，允许您将交互式pdf下载到本地文件系统。在AEM发布实例上部署此包。
   * **xdp-form-and-workflow.zip**  — 此AEM包包含XDP、示例工作流、在节点内容/pdf提交中配置的启动器。在AEM创作实例和发布实例上部署此包。
   * **HandlePDFSubmission.HandlePDFSubmission.core-1.0-SNAPSHOT.jar**  — 这是AEM包，可完成大部分工作。此包包含装载在`/bin/startworkflow`上的Servlet。 此Servlet将提交的表单数据保存在AEM存储库的`/content/pdfsubmissions`节点下。 在AEM创作实例和发布实例上部署此包。
* [预览移动设备表单](http://localhost:4503/content/dam/formsanddocuments/testsubmision.xdp/jcr:content)
* 填写多个字段，然后单击工具栏上的按钮以下载交互式PDF。
* 使用Acrobat填写下载的PDF，然后点击提交按钮。
* 您应会收到一则成功消息
* 以管理员身份登录AEM创作实例
* [查看AEM收件箱](http://localhost:4502/aem/inbox)
* 您应该有一个工作项来审核提交的PDF

>[!NOTE]
>
>有些客户未将PDF提交到发布实例上运行的Servlet，而是将Servlet部署在Servlet容器（如Tomcat）中。 这取决于客户熟悉的拓扑。为了在本教程中，我们将使用发布实例上部署的Servlet来处理PDF提交。

