---
title: 在AEM Forms工作流程中使用LDAP
description: 将AEM Forms工作流任务分配给提交者的管理器
feature: Adaptive Forms, Workflow
topic: Integrations
role: Developer
version: Experience Manager 6.4, Experience Manager 6.5
level: Intermediate
exl-id: 2e9754ff-49fe-4260-b911-796bcc4fd266
last-substantial-update: 2021-09-18T00:00:00Z
duration: 111
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '508'
ht-degree: 0%

---

# 在AEM Forms工作流程中使用LDAP

将AEM Forms工作流任务分配给提交者的管理器。

在AEM工作流中使用自适应表单时，您需要动态地将任务分配给表单提交者的经理。 要完成此用例，我们必须使用Ldap配置AEM。

[此处](https://helpx.adobe.com/cn/experience-manager/6-5/sites/administering/using/ldap-config.html)详细说明了使用LDAP配置AEM所需的步骤。

出于本文的目的，我附加了使用Adobe Ldap配置AEM时使用的配置文件。 这些文件包含在包中，可使用包管理器导入这些文件。

在下面的屏幕截图中，我们将获取属于特定成本中心的所有用户。 如果要获取LDAP中的所有用户，则不能使用额外的过滤器。

![LDAP配置](assets/costcenterldap.gif)

在下面的屏幕截图中，我们将这些组分配给从LDAP获取到AEM中的用户。 请注意分配给导入用户的表单 — 用户组。 用户必须是此组的成员才能与AEM Forms交互。 我们还将manager资产存储在AEM中的profile/manager节点下。

![同步器](assets/synchandler.gif)

配置LDAP并将用户导入AEM后，我们可以创建工作流以将任务分配给提交者的管理器。 为此，我们开发了一个简单的一步式审批工作流。

工作流中的第一个步骤将initialstep的值设置为“否”。 自适应表单中的业务规则将禁用“提交者详细信息”面板，并根据初始步骤值显示“批准者”面板。

第二步将任务分配给提交者的管理器。 我们使用自定义代码获取提交者的经理。

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

代码片段负责获取管理器id并将任务分配给管理器。

我们掌握启动工作流程的人员。 然后，我们获取manager属性的值。

根据Manager属性在LDAP中的存储方式，您可能需要执行一些字符串操作才能获取Manager ID。

请阅读本文以实施您自己的[ParticipantChooser 。](https://helpx.adobe.com/experience-manager/using/dynamic-steps.html)

要在您的系统上对此进行测试(对于Adobe员工，您可以开箱即用地使用此示例)

* [下载并部署setvalue包](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)。 这是用于设置管理器的属性的自定义OSGI捆绑包。
* [下载并安装DevelopingWithServiceUserBundle](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [使用包管理器](assets/aem-forms-ldap.zip)将与本文关联的Assets导入AEM。此包中包含有LDAP配置文件、工作流和自适应表单。
* 使用适当的LDAP凭据在LDAP中配置AEM。
* 使用您的LDAP凭据登录AEM。
* 打开[timeoffrequestform](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)
* 填写表单并提交。
* 提交者的经理应获取表单以供审阅。

>[!NOTE]
>
>此用于提取管理器名称的自定义代码已针对Adobe LDAP进行了测试。 如果您要针对其他LDAP执行此代码，则必须修改或编写自己的getParticipant实施才能获取经理的名称。
