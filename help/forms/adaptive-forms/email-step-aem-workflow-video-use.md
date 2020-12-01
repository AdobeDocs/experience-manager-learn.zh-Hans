---
title: 使用发送电子邮件步骤Forms Workflow
seo-title: 使用发送电子邮件步骤Forms Workflow
description: “发送电子邮件”步骤在AEM Forms6.4中引入。使用此步骤，我们可以构建业务流程或工作流，以便您发送带有或不带附件的电子邮件。 以下视频将逐步介绍配置发送电子邮件组件的步骤
seo-description: “发送电子邮件”步骤在AEM Forms6.4中引入。使用此步骤，我们可以构建业务流程或工作流，以便您发送带有或不带附件的电子邮件。 以下视频将逐步介绍配置发送电子邮件组件的步骤
uuid: d054ebfb-3b9b-4ca4-8355-0eb0ee7febcb
feature: workflow
topics: integrations
audience: developer
doc-type: technical video
activity: setup
version: 6.4,6.5
discoiquuid: 3a11f602-2f4c-423a-baef-28824c0325a1
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '493'
ht-degree: 1%

---


# 使用Forms Workflow{#using-send-email-step-of-forms-workflow}的发送电子邮件步骤

“发送电子邮件”步骤在AEM Forms6.4中引入。使用此步骤，我们可以构建业务流程或工作流，以便您发送带有或不带附件的电子邮件。 以下视频将逐步介绍配置发送电子邮件组件的步骤。

>[!VIDEO](https://video.tv.adobe.com/v/21499/?quality=9&learn=on)

在本文中，我们将带您了解以下用例：

1. 用户填写请求表的结束时间
1. 提交表单时，将触发AEM Workflow
1. AEM工作流利用“发送电子邮件”组件以DoR作为附件发送电子邮件

在使用“发送电子邮件”步骤之前，请确保从[configMgr](http://localhost:4502/system/console/configMgr)配置Day CQ邮件服务。 提供特定于您的环境的值

![配置Day CQ邮件服务](assets/mailservice.png)

作为与本文关联的资产的一部分，您将获得以下

1. 自适应表单，在提交时触发工作流
1. 将以DOR作为附件发送电子邮件的示例工作流
1. 创建元数据属性的OSGi捆绑

要使示例在系统上运行，请执行以下操作：

1. [使用服务用户包部署开发](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

1. [下载和安装](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)setvalue bundle此捆绑包包含创建元数据属性的代码，该属性是工作流流程步骤的一部分。
1. [配置Day CQ邮件服务](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/notification.html)
1. [使用包管理器将与本文关联的资产导入和安装到CRX中](assets/emaildoraemformskt.zip)
1. 启动[自适应表单](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)。 填写必填字段并提交。
1. 您应收到一封以DocumentOfRecord为附件的电子邮件

浏览[工作流模型](http://localhost:4502/editor.html/conf/global/settings/workflow/models/emaildor.html)

查看工作流的流程步骤。 与流程步骤关联的自定义代码将创建元数据属性名称并根据提交的数据设置其值。这些值随后由发送电子邮件组件使用。

>[!NOTE]
>
>在AEM Forms6.5及更高版本中，您无需此自定义代码即可创建元数据属性。 请使用AEM Workflow中的变量功能

确保根据以下屏幕快照配置“发送电子邮件”组件的“附件”选项卡
![“发送电子邮件附件”选项卡](assets/sendemailcomponentconfigure.jpg)“DOR.pdf”值必须与自适应表单提交选项中指定的记录路径文档中指定的值相匹配。

