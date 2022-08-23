---
title: 将LDAP与AEM Forms工作流结合使用
description: 将AEM Forms工作流任务分配给提交者的管理器
feature: Adaptive Forms, Workflow
topic: Integrations
role: Developer
version: 6.4,6.5
level: Intermediate
exl-id: 2e9754ff-49fe-4260-b911-796bcc4fd266
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '528'
ht-degree: 0%

---

# 将LDAP与AEM Forms工作流结合使用

将AEM Forms工作流任务分配给提交者的管理器。

在AEM工作流中使用自适应表单时，您需要将任务动态分配给表单提交者的管理器。 要完成此用例，我们必须使用Ldap配置AEM。

有关使用LDAP配置AEM所需的步骤，请参阅 [详情请见。](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ldap-config.html)

为本文的目的，我附加了用于使用AdobeLDAP配置AEM的配置文件。 这些文件包含在包中，可使用包管理器导入该包。

在下面的屏幕截图中，我们正在获取属于某个特定成本中心的所有用户。 如果要获取LDAP中的所有用户，则不能使用额外的过滤器。

![LDAP配置](assets/costcenterldap.gif)

在下面的屏幕截图中，我们将组分配给从LDAP获取到AEM的用户。 请注意分配给导入用户的表单用户组。 用户需要是此组的成员才能与AEM Forms进行交互。 我们还将manager属性存储在AEM的profile/manager节点下。

![辛钱德勒](assets/synchandler.gif)

配置LDAP并将用户导入AEM后，我们可以创建一个工作流，将任务分配给提交者的管理器。 为了撰写本文，我们开发了一个简单的一步式审批工作流程。

工作流中的第一步将初始步骤的值设置为“否”。 自适应表单中的业务规则将禁用“提交者详细信息”面板，并根据初始步骤值显示“批准者”面板。

第二步将任务分配给提交者的管理器。 使用自定义代码获取提交者的经理。

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

代码段负责获取管理器ID并将任务分配给管理器。

我们会联系启动工作流的人员。 然后，获取manager属性的值。

根据manager属性在LDAP中的存储方式，您可能需要执行一些字符串处理来获取管理器ID。

请阅读本文以实施您自己的 [  参与者选择器。](https://helpx.adobe.com/experience-manager/using/dynamic-steps.html)

要在系统上测试此示例(对于Adobe员工，您可以开箱即用此示例)

* [下载和部署setvalue包](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). 这是用于设置管理器属性的自定义OSGI包。
* [下载并安装DevelopingWithServiceUserBundle](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [使用包管理器将与本文关联的资产导入AEM](assets/aem-forms-ldap.zip)此包中包含的.LDAP配置文件、工作流和自适应表单。
* 使用适当的LDAP凭据在LDAP中配置AEM。
* 使用LDAP凭据登录AEM。
* 打开 [timeoffrequestform](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)
* 填写表格并提交。
* 提交者的经理应该得到该表格进行审核。

>[!NOTE]
>
>已针对AdobeLDAP测试此用于提取管理器名称的自定义代码。 如果您针对其他LDAP执行此代码，则必须修改或编写您自己的getParticipant实施，以获取管理器的名称。
