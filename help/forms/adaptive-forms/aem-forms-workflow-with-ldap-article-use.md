---
title: 将LDAP与AemForms Workflow
seo-title: 将LDAP与AemForms Workflow
description: 将AEM Forms工作流任务分配给提交者的管理者
seo-description: 将AEM Forms工作流任务分配给提交者的管理者
feature: 自适应Forms，工作流
topics: integrations
audience: developer
doc-type: article
activity: setup
version: 6.3,6.4,6.5
uuid: 3e32c3a7-387f-4652-8a94-4e6aa6cd5ab8
discoiquuid: 671872b3-3de0-40da-9691-f8b7e88a9443
topic: 开发
role: 管理员
level: 中间
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '549'
ht-degree: 0%

---


# 将LDAP与AEM Forms Workflow结合使用

将AEM Forms工作流任务分配给提交者的管理者。

在AEM工作流中使用自适应表单时，您需要将任务动态分配给表单提交者的管理者。 要完成此用例，我们必须使用Ldap配置AEM。

此处的[详细信息中介绍了配置AEM与LDAP所需的步骤。](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ldap-config.html)

为本文之目的，我附加了用于使用Adobe Ldap配置AEM的配置文件。 这些文件包含在包中，可以使用包管理器导入。

在下面的屏幕截图中，我们正在获取属于特定费用中心的所有用户。 如果要获取LDAP中的所有用户，则不能使用额外的过滤器。

![LDAP配置](assets/costcenterldap.gif)

在下面的屏幕截图中，我们将用户组分配给从LDAP获取到AEM的用户。 请注意分配给导入用户的表单用户组。 用户需要是此组的成员才能与AEM Forms交互。 我们还将manager属性存储在AEM的用户档案/manager节点下。

![辛钱德勒](assets/synchandler.gif)

配置LDAP并将用户导入AEM后，我们可以创建一个工作流，该工作流将任务分配给提交者的管理者。 为了本文的目的，我们开发了一个简单的一步式审批工作流。

工作流中的第一步将初始步骤的值设置为“否”。 自适应表单中的业务规则将禁用“提交者详细信息”面板，并根据初始步骤值显示“批准者”面板。

第二步将任务分配给提交者的管理者。 我们使用自定义代码让提问者的经理。

![分配任务](assets/assigntask.gif)

```java
public String getParticipant(WorkItem workItem, WorkflowSession wfSession, MetaDataMap arg2) throws WorkflowException{
resourceResolver = wfSession.adaptTo(ResourceResolver.class);
UserManager userManager = resourceResolver.adaptTo(UserManager.class);
Authorizable workflowInitiator = userManager.getAuthorizable(workItem.getWorkflow().getInitiator());
.
.
String managerPorperty = workflowInitiator.getProperty("profile/manager")[0].getString();
.
.

}
```

代码片断负责获取管理者ID并将任务分配给管理者。

我们了解启动该工作流的人。 然后，我们得到manager属性的值。

根据LDAP中存储manager属性的方式，您可能需要执行一些字符串处理才能获取管理器ID。

请阅读本文以实现您自己的[ ParticipantChooser .](https://helpx.adobe.com/experience-manager/using/dynamic-steps.html)

要在您的系统上测试此示例(对于Adobe员工，您可以开箱使用此示例)

* [下载并部署setvalue包](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)。这是用于设置管理器属性的自定义OSGI包。
* [下载并安装DevelopingWithServiceUserBundle](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [使用包管理器将与此文章关联的资产导入AEM](assets/aem-forms-ldap.zip)。作为此包的一部分，包括LDAP配置文件、工作流和自适应表单。
* 使用适当的LDAP凭据在LDAP中配置AEM。
* 使用LDAP凭据登录AEM。
* 打开[timeofrequestform](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)
* 填写表单并提交。
* 提交者的经理应将表单提交审阅。

>[!NOTE]
>
>此用于提取管理器名称的自定义代码已针对Adobe LDAP进行测试。 如果要针对其他LDAP执行此代码，则必须修改或编写自己的getParticipant实现，以获取管理者的姓名。
