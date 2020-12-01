---
title: 简单的请求退款时间工作流
description: 在AEM工作流中隐藏和显示自适应表单面板
uuid: 28ceb72b-24d9-488e-92af-7e85775dc682
feature: integrations
topics: workflow
audience: developer
doc-type: article
activity: use
version: 6.4,6.5
discoiquuid: 1c4822e6-76ce-446b-98cb-408900d68b24
translation-type: tm+mt
source-git-commit: 449202af47b6bbcd9f860d5c5391d1f7096d489e
workflow-type: tm+mt
source-wordcount: '332'
ht-degree: 0%

---


# 简单的请求退款时间工作流

在本文中，我们将看到一个用于请求付费休假的简单工作流。 业务要求如下：

* 用户通过填写自适应表单请求暂停时间。
* 表单将发送给AEM管理员用户（在现实生活中，它将发送给提交者的经理）
* 管理员打开表单。 管理员不能编辑提交者填写的任何信息。
* 审批者部分应对审批者可见(在本例中为AEM管理员用户)。

为满足上述要求，我们在表单中使用一个名为&#x200B;**initialstep**&#x200B;的隐藏字段，其默认值设置为“是”。提交表单时，工作流中的第一步将初始步骤的值设置为“否”。 表单具有业务规则，可根据初始步骤值隐藏和显示相应的部分。

**将表单配置为触发AEM工作流**

>[!VIDEO](https://video.tv.adobe.com/v/28406?quality=9&learn=on)

**工作流程演练**

>[!VIDEO](https://video.tv.adobe.com/v/28407?quality=9&learn=on)

**提交者对“结束时间”请求表的视图**

![初始值](assets/initialstep.gif)

**表单的审批者视图**

![approverview](assets/approversview.gif)

在审批者视图中，审批者无法编辑提交的数据。 此外，还新增了仅适用于批准者的章节。

要在系统上测试此工作流，请按照以下步骤操作：
* [下载和部署DevelopingWithServiceUserBundle](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [下载和部署SetValue自定义OSGI捆绑包](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)
* [将与本文相关的资产导入AEM](assets/helpxworkflow.zip)
* 打开[结束时间请求表](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)
* 填写详细信息并提交
* 打开[收件箱](http://localhost:4502/mnt/overlay/cq/inbox/content/inbox.html)。 您应当看到分配了新任务。 打开表单。 提交者的数据应为只读，并且应显示新的审批者部分。
* 浏览[工作流模型](http://localhost:4502/editor.html/conf/global/settings/workflow/models/helpxworkflow.html)
* 浏览流程步骤。 此步骤将初始步骤的值设置为“否”。
