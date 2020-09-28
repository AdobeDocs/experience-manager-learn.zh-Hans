---
title: 将LDAP与AemForms Workflow
seo-title: 将LDAP与AemForms Workflow
description: 将AEM Forms工作流任务分配给提交者的经理
seo-description: 将AEM Forms工作流任务分配给提交者的经理
feature: adaptive-forms,workflow
topics: integrations
audience: developer
doc-type: article
activity: setup
version: 6.3,6.4,6.5
uuid: 3e32c3a7-387f-4652-8a94-4e6aa6cd5ab8
discoiquuid: 671872b3-3de0-40da-9691-f8b7e88a9443
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '543'
ht-degree: 0%

---


# 将LDAP与AEM Forms工作流程结合使用

将AEM Forms工作流任务分配给提交者的经理。

在AEM工作流中使用自适应表单时，您需要将任务动态分配给表单提交者的管理者。 要完成此用例，我们必须使用Ldap配置AEM。

此处详细介绍了使用LDAP配置AEM所需 [的步骤。](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ldap-config.html)

为了本文，我将附加配置AEM时使用AdobeLdap时使用的配置文件。 这些文件包含在包中，可以使用包管理器导入。

在下面的屏幕截图中，我们正在提取属于特定费用中心的所有用户。 如果要获取LDAP中的所有用户，则不能使用额外的过滤器。

![LDAP配置](assets/costcenterldap.gif)

在下面的屏幕截图中，我们将组分配给从LDAP访问到AEM的用户。 请注意分配给导入用户的表单用户组。 用户必须是该组的成员才能与AEM Forms交互。 我们还将manager属性存储在AEM的用户档案/管理器节点下。

![辛钱德勒](assets/synchandler.gif)

配置LDAP并将用户导入AEM后，我们可以创建一个工作流，将任务分配给提交者的管理者。 为此，我们开发了一个简单的一步式审批工作流。

工作流中的第一步将初始步骤的值设置为“否”。 自适应表单中的业务规则将禁用“提交者详细信息”面板，并根据初始步骤值显示“批准者”面板。

第二步将任务分配给提交者的管理者。 我们使用自定义代码向提问者的经理咨询。

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

我们会找到启动该工作流的人。 然后，我们得到管理器属性的值。

根据LDAP中存储管理器属性的方式，您可能需要执行一些字符串处理才能获取管理器ID。

请阅读本文以实施您自己的参加者 [ 选择器。](https://helpx.adobe.com/experience-manager/using/dynamic-steps.html)

要在系统上测试此示例(对于Adobe员工，您可以使用此示例开箱即用)

* [下载并部署setvalue捆绑包](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)。 这是用于设置管理器属性的自定义OSGI捆绑包。
* [下载并安装DevelopingWithServiceUserBundle](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [使用包管理器将与本文关联的资产导入AEM](assets/aem-forms-ldap.zip)。作为此包的一部分，包括LDAP配置文件、工作流和自适应表单。
* 使用适当的LDAP凭据在LDAP中配置AEM。
* 使用LDAP凭据登录AEM。
* 打开时间 [offrequestform](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)
* 填写表单并提交。
* 提交者的经理应该得到表单以供审阅。

>[!NOTE]
>
>此用于提取管理器名称的自定义代码已针对AdobeLDAP进行测试。 如果要针对其他LDAP执行此代码，则必须修改或编写自己的getParticipant实现以获取管理者的姓名。
