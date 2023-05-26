---
title: 在AEM中开发项目
description: 一个开发教程，说明如何为AEM项目开发。  在本教程中，我们将创建一个自定义项目模板，该模板可用于在AEM中创建新项目，以管理内容创作工作流和任务。
version: 6.4, 6.5
feature: Projects, Workflow
topics: collaboration, development, governance
activity: develop
audience: developer, implementer, administrator
doc-type: tutorial
topic: Development
role: Developer
level: Beginner
exl-id: 9bfe3142-bfc1-4886-85ea-d1c6de903484
source-git-commit: 481b8877e252b885da307fcf4d96f8a50f026fa6
workflow-type: tm+mt
source-wordcount: '4571'
ht-degree: 0%

---

# 在AEM中开发项目

这是一个开发教程，说明了如何为开发 [!DNL AEM Projects].  在本教程中，我们将创建一个自定义项目模板，该模板可用于在AEM中创建新项目，以管理内容创作工作流和任务。

>[!VIDEO](https://video.tv.adobe.com/v/16904?quality=12&learn=on)

*此视频简要演示了以下教程中创建的已完成工作流。*

## 简介 {#introduction}

[[!DNL AEM Projects]](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/projects.html) 是AEM的一项功能，旨在简化对作为AEM Sites或Assets实施的一部分与内容创建相关的所有工作流和任务的管理和分组。

AEM项目附带多个项目 [OOTB项目模板](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/projects.html#ProjectTemplates). 创建新项目时，作者可以从这些可用的模板中进行选择。 具有独特业务要求的大型AEM实施将要创建定制的项目模板，以便满足其需求。 通过创建自定义项目模板，开发人员可以配置项目仪表板、挂接到自定义工作流，并为项目创建其他业务角色。 我们将查看项目模板的结构并创建一个示例模板。

![自定义项目信息卡](./assets/develop-aem-projects/custom-project-card.png)

## 设置

本教程将逐步介绍创建自定义项目模板所需的代码。 您可以下载并安装 [附加的包](./assets/develop-aem-projects/projects-tasks-guide.ui.apps-0.0.1-SNAPSHOT.zip) ，以供您在本教程中学习使用。 您还可以访问上托管的完整Maven项目 [GitHub](https://github.com/Adobe-Marketing-Cloud/aem-guides/tree/feature/projects-tasks-guide).

* [完成的教程包](./assets/develop-aem-projects/projects-tasks-guide.ui.apps-0.0.1-SNAPSHOT.zip)
* [GitHub上的完整代码存储库](https://github.com/Adobe-Marketing-Cloud/aem-guides/tree/feature/projects-tasks-guide)

本教程假定您具备以下的一些基本知识 [AEM开发实践](https://helpx.adobe.com/cn/experience-manager/6-5/sites/developing/using/the-basics.html) 而且很熟悉 [AEM Maven项目设置](https://helpx.adobe.com/cn/experience-manager/6-5/sites/developing/using/ht-projects-maven.html). 提到的所有代码均旨在用作参考，并且只应部署到 [本地开发AEM实例](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html#GettingStarted).

## 项目模板的结构

项目模板应该置于源代码管理之下，并且应该位于/apps下的应用程序文件夹下。 理想情况下，应将它们放置在子文件夹中，其命名约定为 **&#42;/projects/templates/**&lt;my-template>. 通过遵循此命名约定，在创建项目时，任何新的自定义模板都将自动可供作者使用。 可用项目模板的配置设置为： **/content/projects/jcr：content** 节点依据 **cq：allowedTemplates** 属性。 默认情况下，这是一个正则表达式： **/(apps|libs)/.&#42;/projects/templates/.&#42;**

项目模板的根节点将具有 **jcr：primaryType** 之 **cq：Template**. 在的根节点下有3个节点： **小工具**， **角色**、和 **工作流**. 这些节点都是 **nt：unstructured**. 根节点下方也可以是thumbnail.png文件，在“创建项目”向导中选择模板时显示该文件。

完整的节点结构：

```shell
/apps/<my-app>
    + projects (nt:folder)
         + templates (nt:folder)
              + <project-template-root> (cq:Template)
                   + gadgets (nt:unstructured)
                   + roles (nt:unstructured)
                   + workflows (nt:unstructured)
```

### 项目模板根目录

项目模板的根节点属于类型 **cq：Template**. 在此节点上，您可以配置属性 **jcr：title** 和 **jcr：description** 创建项目向导中显示的属性。 还有一个属性称为 **向导** 指向将填充项目属性的表单。 默认值： **/libs/cq/core/content/projects/wizard/steps/defaultproject.html** 大多数情况下应该可以正常使用，因为它允许用户填充基本项目属性并添加组成员。

*&#42;请注意，“创建项目向导”不使用SlingPOSTservlet。 而是将值发布到自定义servlet：**com.adobe.cq.projects.impl.servlet.ProjectServlet**. 添加自定义字段时应考虑这一点。*

可以在翻译项目模板中找到自定义向导的示例： **/libs/cq/core/content/projects/wizard/translationproject/defaultproject**.

### 小工具 {#gadgets}

此节点上没有其他属性，但小工具的子节点控制创建新项目时哪些项目拼贴填充了项目的仪表板。 [项目拼贴](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/projects.html#ProjectTiles) （也称为小工具或pod）是填充项目工作区的简单信息卡。 您可以在**/libs/cq/gui/components/projects/admin/pod下找到ootb图块的完整列表。 **创建项目后，项目所有者始终可以添加/删除图块。

### 角色 {#roles}

有3个 [默认角色](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/projects.html#UserRolesinaProject) 对于每个项目： **观察者**， **编辑者**、和 **所有者**. 通过在角色节点下添加子节点，您可以为模板添加其他特定于业务的项目角色。 然后，您可以将这些角色关联到与项目关联的特定工作流。

### 工作流 {#workflows}

创建自定义项目模板的最诱人原因之一是，它使您能够配置可用于项目的工作流。 这些工作流可以是OOTB工作流或自定义工作流。 在 **工作流** 节点中需要有一个 **模型** 节点(也 `nt:unstructured`)和下面的子节点指定可用的工作流模型。 属性**modelId **指向/etc/workflow下的工作流模型和属性 **向导** 指向启动工作流时使用的对话框。 Projects的一大优势是能够添加自定义对话框（向导），以在工作流开始时捕获特定于业务的元数据，从而推动工作流中的进一步操作。

```shell
<projects-template-root> (cq:Template)
    + workflows (nt:unstructured)
         + models (nt:unstructured)
              + <workflow-model> (nt:unstructured)
                   - modelId = points to the workflow model
                   - wizard = dialog used to start the workflow
```

## 创建项目模板 {#creating-project-template}

由于我们主要复制/配置节点，因此我们将使用CRXDE Lite。 在本地AEM实例中打开 [CRXDE Lite](http://localhost:4502/crx/de/index.jsp).

1. 首先，在下方创建一个新文件夹 `/apps/&lt;your-app-folder&gt;` 已命名 `projects`. 在该文件夹下创建另一个名为 `templates`.

   ```shell
   /apps/aem-guides/projects-tasks/
                       + projects (nt:folder)
                                + templates (nt:folder)
   ```

1. 为了简化操作，我们将从现有的“简单项目”模板中启动自定义模板。

   1. 复制并粘贴节点 **/libs/cq/core/content/projects/templates/default** 在 *模板* 在第1步中创建的文件夹。

   ```shell
   /apps/aem-guides/projects-tasks/
                + templates (nt:folder)
                     + default (cq:Template)
   ```

1. 现在，您应该拥有类似于 **/apps/aem-guides/projects-tasks/projects/templates/authoring-project**.

   1. 编辑 **jcr：title** 和 **jcr：description** author-project节点的属性以自定义标题和描述值。

      1. 保留 **向导** 属性，指向默认的项目属性。

   ```shell
   /apps/aem-guides/projects-tasks/projects/
            + templates (nt:folder)
                 + authoring-project (cq:Template)
                      - jcr:title = "Authoring Project"
                      - jcr:description = "A project to manage approval and publish process for AEM Sites or Assets"
                      - wizard = "/libs/cq/core/content/projects/wizard/steps/defaultproject.html"
   ```

1. 对于此项目模板，我们希望使用“任务”。
   1. 添加新 **nt：unstructured** 已调用authoring-project/gadgets下的节点 **任务**.
   1. 将字符串属性添加到任务节点 **cardWeight** =“100”， **jcr：title**=“任务”，和 **sling：resourceType**=&quot;cq/gui/components/projects/admin/pod/taskpod&quot;。

   现在， [“任务”拼贴](https://experienceleague.adobe.com/docs/#Tasks) 默认情况下将在创建新项目时显示。

   ```shell
   ../projects/templates/authoring-project
       + gadgets (nt:unstructured)
            + team (nt:unstructured)
            + asset (nt:unstructured)
            + work (nt:unstructured)
            + experiences (nt:unstructured)
            + projectinfo (nt:unstructured)
            ..
            + tasks (nt:unstructured)
                 - cardWeight = "100"
                 - jcr:title = "Tasks"
                 - sling:resourceType = "cq/gui/components/projects/admin/pod/taskpod"
   ```

1. 我们将向项目模板添加自定义审批者角色。

   1. 在项目模板(authoring-project)节点下添加新的 **nt：unstructured** 节点已标记 **角色**.
   1. 添加另一个 **nt：unstructured** 标记为批准者的节点作为“角色”节点的子节点。
   1. 添加字符串属性 **jcr：title** = &quot;**审批者**“， **roleclass** =”**所有者**“， **roleid**=”**审批者**“。
      1. 审批者节点的名称以及jcr：title和roleid可以是任何字符串值（只要roleid是唯一的）。
      1. **roleclass** 根据权限管理应用于该角色的权限 [3个OOTB角色](https://docs.adobe.com/docs/en/aem/6-3/author/projects.html#User%20Roles%20in%20a%20Project)： **所有者**， **编辑者**、和 **观察者**.
      1. 通常，如果自定义角色更多地是管理角色，则角色可以是 **业主；** 如果它是更具体的创作角色，如摄影师或设计师，则 **编辑者** 罗勒克洛斯就足够了。 两者之间的巨大差异 **所有者** 和 **编辑者** 是项目所有者可以更新项目属性并将新用户添加到项目。

   ```shell
   ../projects/templates/authoring-project
       + gadgets (nt:unstructured)
       + roles (nt:unstructured)
           + approvers (nt:unstructured)
                - jcr:title = "Approvers"
                - roleclass = "owner"
                - roleid = "approver"
   ```

1. 通过复制简单项目模板，您将配置4个OOTB工作流。 工作流/模型下的每个节点都指向该工作流的特定工作流和开始对话框向导。 在本教程的后面部分，我们将为此项目创建一个自定义工作流。 现在，请删除工作流/模型下的节点：

   ```shell
   ../projects/templates/authoring-project
       + gadgets (nt:unstructured)
       + roles (nt:unstructured)
       + workflows (nt:unstructured)
            + models (nt:unstructured)
               - (remove ootb models)
   ```

1. 为了便于内容作者识别项目模板，您可以添加自定义缩略图。 建议的大小为319x319像素。
   1. 在CRXDE Lite中，创建一个新文件作为名为的小工具、角色和工作流节点的同级 **thumbnail.png**.
   1. 保存，然后导航到 `jcr:content` 节点，然后双击 `jcr:data` 属性（避免单击“视图”）。
      1. 此时应会提示您进行编辑 `jcr:data` 文件对话框，您可以上传自定义缩略图。

   ```shell
   ../projects/templates/authoring-project
       + gadgets (nt:unstructured)
       + roles (nt:unstructured)
       + workflows (nt:unstructured)
       + thumbnail.png (nt:file)
   ```

项目模板的XML表示已完成：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
    jcr:description="A project to manage approval and publish process for AEM Sites or Assets"
    jcr:primaryType="cq:Template"
    jcr:title="Authoring Project"
    ranking="{Long}1"
    wizard="/libs/cq/core/content/projects/wizard/steps/defaultproject.html">
    <jcr:content
        jcr:primaryType="nt:unstructured"
        detailsHref="/projects/details.html"/>
    <gadgets jcr:primaryType="nt:unstructured">
        <team
            jcr:primaryType="nt:unstructured"
            jcr:title="Team"
            sling:resourceType="cq/gui/components/projects/admin/pod/teampod"
            cardWeight="60"/>
        <tasks
            jcr:primaryType="nt:unstructured"
            jcr:title="Tasks"
            sling:resourceType="cq/gui/components/projects/admin/pod/taskpod"
            cardWeight="100"/>
        <work
            jcr:primaryType="nt:unstructured"
            jcr:title="Workflows"
            sling:resourceType="cq/gui/components/projects/admin/pod/workpod"
            cardWeight="80"/>
        <experiences
            jcr:primaryType="nt:unstructured"
            jcr:title="Experiences"
            sling:resourceType="cq/gui/components/projects/admin/pod/channelpod"
            cardWeight="90"/>
        <projectinfo
            jcr:primaryType="nt:unstructured"
            jcr:title="Project Info"
            sling:resourceType="cq/gui/components/projects/admin/pod/projectinfopod"
            cardWeight="100"/>
    </gadgets>
    <roles jcr:primaryType="nt:unstructured">
        <approvers
            jcr:primaryType="nt:unstructured"
            jcr:title="Approvers"
            roleclass="owner"
            roleid="approvers"/>
    </roles>
    <workflows
        jcr:primaryType="nt:unstructured"
        tags="[]">
        <models jcr:primaryType="nt:unstructured">
        </models>
    </workflows>
</jcr:root>
```

## 测试自定义项目模板

现在，我们可以通过创建新项目来测试项目模板。

1. 您应该看到自定义模板作为项目创建的选项之一。

   ![选择模板](./assets/develop-aem-projects/choose-template.png)

1. 选择自定义模板后，单击“下一步”，并注意填充项目成员时，您可以将其添加为审批者角色。

   ![批准](./assets/develop-aem-projects/user-approver.png)

1. 单击“创建”以根据自定义模板完成项目创建。 您会注意到项目功能板上的任务拼贴以及小工具下配置的其他拼贴会自动显示。

   ![任务拼贴](./assets/develop-aem-projects/tasks-tile.png)


## 为什么选择工作流？

传统上，以审批流程为中心的AEM工作流使用参与者工作流步骤。 AEM收件箱包括有关任务和工作流的详细信息以及与AEM项目增强的集成。 这些功能使使用项目创建任务流程步骤成为一个更具吸引力的选项。

### 为何要执行任务？

与传统“参与者”步骤相比，使用“任务创建步骤”具有如下优点：

* **开始和到期日期**  — 使作者能够轻松管理时间，新的日历功能充分利用了这些日期。
* **优先级**  — 内置“低”、“正常”和“高”优先级，可让作者优先考虑工作
* **线程注释**  — 在作者处理任务时，他们能够发表意见以增强协作
* **可见性**  — 项目中的任务拼贴和视图允许经理查看时间的使用情况
* **项目集成**  — 任务已与项目角色和功能板集成

与“参与者”步骤一样，任务也可以动态分配和路由。 任务元数据（如标题、优先级）也可以根据之前的操作动态设置，如下面的教程所示。

虽然与参与者步骤相比，任务具有一些优势，但它们的确会产生额外的开销，并且在项目之外没有那么有用。 此外，任务的所有动态行为都必须使用具有自身限制的ecma脚本进行编码。

## 示例用例要求 {#goals-tutorial}

![工作流流程图](./assets/develop-aem-projects/workflow-process-diagram.png)

上图概述了我们的示例审批工作流的高级要求。

第一步是创建一个Task以完成内容的编辑。 我们将允许工作流发起者选择此第一项任务的被分配人。

完成第一个任务后，被分派人将有三个选项来路由工作流：

**普通** — 普通路由会创建一项任务，该任务将分配给项目的批准者组进行审查和批准。 任务的优先级为正常，截止日期为创建日期后的5天。

**Rush**  — 快速传送还会创建分配给项目审批者组的任务。 任务的优先级为“高”，截止日期仅为1天。

**旁路**  — 在此示例工作流中，初始参与者可以选择绕过批准组。 （是的，这可能会破坏“审批”工作流的用途，但它允许我们说明其他路由功能）

审批者组可以审批内容，也可以将内容发回初始被分派人以便重新工作。 在被发回重工的情况下，将创建一个新任务，并相应地将其标记为“发回重工”。

工作流的最后一个步骤使用ootb激活页面/资产流程步骤并复制有效负载。

## 创建工作流模型

1. 从AEM的“开始”菜单中，导航到工具 — >工作流 — >模型。 单击右上角的“创建”以创建新的工作流模型。

   为新模型指定标题“内容审批工作流”和URL名称“content-approval-workflow”。

   ![工作流创建对话框](./assets/develop-aem-projects/workflow-create-dialog.png)

   有关详情，请参阅： [创建工作流程，请阅读此处](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/workflows-models.html).

1. 作为最佳实践，自定义工作流应分组到/etc/workflow/models下的它们自己的文件夹中。 在CRXDE Lite中新建 **&#39;nt：folder&#39;** /etc/workflow/models下名为 **&quot;aem-guides&quot;**. 添加子文件夹可确保自定义工作流在升级或Service Pack安装期间不会被意外覆盖。

   &#42;请注意，切勿将文件夹或自定义工作流放置在ootb子文件夹（如/etc/workflow/models/dam或/etc/workflow/models/projects）下，因为整个子文件夹也可能会被升级或Service Pack覆盖，这一点很重要。

   ![工作流模型在6.3中的位置](./assets/develop-aem-projects/custom-workflow-subfolder.png)

   工作流模型在6.3中的位置

   >[!NOTE]
   >
   >如果使用AEM 6.4+，则工作流的位置已更改。 参见 [此处了解更多详细信息。](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/workflows-best-practices.html#LocationsWorkflowModels)

   如果使用AEM 6.4+，将在下创建工作流模型 `/conf/global/settings/workflow/models`. 对/conf目录重复上述步骤，并添加名为的子文件夹 `aem-guides` 并移动 `content-approval-workflow` 在它下面。

   ![现代工作流定义位置](./assets/develop-aem-projects/modern-workflow-definition-location.png)
工作流模型在6.4+中的位置

1. AEM 6.3中引入了向给定工作流添加工作流暂存的功能。 这些暂存将显示在“工作流信息”选项卡的“收件箱”中，供用户使用。 它将向用户显示工作流中的当前阶段以及工作流之前和之后的阶段。

   要配置阶段，请从SideKick打开“页面属性”对话框。 第四个选项卡标记为“暂存”。 添加以下值以配置此工作流的三个阶段：

   1. 编辑内容
   1. 批准
   1. 发布

   ![工作流暂存配置](./assets/develop-aem-projects/workflow-model-stage-properties.png)

   从“页面属性”对话框中配置工作流暂存。

   ![工作流进度条](./assets/develop-aem-projects/workflow-info-progress.png)

   从AEM收件箱中看到的工作流进度条。

   （可选）您可以上传 **图像** 至用户选择时用作工作流缩略图的页面属性。 图像尺寸应为319x319像素。 添加 **描述** “页面属性”的操作也会在用户选择工作流时显示。

1. “创建项目任务”工作流进程旨在将任务创建为工作流中的步骤。 只有在完成任务后，工作流才会前进。 “创建项目任务”步骤的一个强大方面是，它可以读取工作流元数据值，并使用这些值动态创建任务。

   首先删除默认创建的参与者步骤。 从Sidekick的“组件”菜单中，展开 **&quot;项目&quot;** 添加子标题并拖放 **创建项目任务** 放到模型上。

   双击“创建项目任务”步骤以打开工作流对话框。 配置以下属性：

   此选项卡在所有工作流流程步骤中都是通用的，我们将设置标题和描述（最终用户看不到这些内容）。 我们将设置的重要属性是“工作流暂存” **编辑内容** 从下拉菜单中。

   ```shell
   Common Tab
   -----------------
       Title = "Start Task Creation"
       Description = "This the first task in the Workflow"
       Workflow Stage = "Edit Content"
   ```

   “创建项目任务”工作流进程旨在将任务创建为工作流中的步骤。 任务选项卡允许我们设置任务的所有值。 在我们的案例中，我们希望被分派人保持动态，以便将其留空。 其余属性值：

   ```shell
   Task Tab
   -----------------
       Name* = "Edit Content"
       Task Priority = "Medium"
       Description = "Edit the content and finalize for approval. Once finished submit for approval."
       Due In - Days = "2"
   ```

   “路由”选项卡是一个可选对话框，可以为完成任务的用户指定可用的操作。 这些操作只是字符串值，会保存到工作流的元数据中。 这些值可通过脚本读取，和/或稍后通过工作流中的流程步骤进行读取，以动态“路由”工作流。 基于 [工作流目标](#goals-tutorial) 我们将向此选项卡添加三个操作：

   ```shell
   Routing Tab
   -----------------
       Actions =
           "Normal Approval"
           "Rush Approval"
           "Bypass Approval"
   ```

   利用此选项卡，可配置预创建任务脚本，以便我们以编程方式在创建任务之前决定任务的各种值。 我们可以选择将脚本指向外部文件或直接在对话框中嵌入简短脚本。 在我们的示例中，我们将创建前任务脚本指向外部文件。 在步骤5中，我们将创建该脚本。

   ```shell
   Advanced Settings Tab
   -----------------
      Pre-Create Task Script = "/apps/aem-guides/projects/scripts/start-task-config.ecma"
   ```

1. 在上一步中，我们引用了一个预创建任务脚本。 我们现在将创建该脚本，其中我们将根据工作流元数据值&quot;**被分派人**“。 此 **&quot;assignee&quot;** 值在启动工作流时设置。 我们还将读取工作流元数据，以通过读取&#39;&#39;动态选择任务的优先级&#x200B;**任务优先级”** 工作流元数据的值以及**“taskDueDate”**在第一个任务到期时动态设置。

   出于组织目的，我们在应用程序文件夹下创建了一个文件夹，用于存放所有与项目相关的脚本： **/apps/aem-guides/projects-tasks/projects/scripts**. 在此文件夹下创建一个名为的新文件 **&quot;start-task-config.ecma&quot;**. &#42;注意：请确保start-task-config.ecma文件的路径与步骤4的“高级设置”选项卡中设置的路径相匹配。

   添加以下内容作为文件的内容：

   ```
   // start-task-config.ecma
   // Populate the task using values stored as workflow metadata originally posted by the start workflow wizard
   
   // set the assignee based on start workflow wizard
   var assignee = workflowData.getMetaDataMap().get("assignee", Packages.java.lang.String);
   task.setCurrentAssignee(assignee);
   
   //Set the due date for the initial task based on start workflow wizard
   var dueDate = workflowData.getMetaDataMap().get("taskDueDate", Packages.java.util.Date);
   if (dueDate != null) {
       task.setProperty("taskDueDate", dueDate);
   }
   
   //Set the priority based on start workflow wizard
   var taskPriority = workflowData.getMetaDataMap().get("taskPriority", "Medium");
   task.setProperty("taskPriority", taskPriority);
   ```

1. 导航回内容审批工作流。 拖放 **OR拆分** 组件（可在“工作流”类别下的Sidekick中找到） **开始任务** 步骤。 在“常用”对话框中，选择“3个分支”的单选按钮。 OR拆分将读取工作流元数据值 **&quot;lastTaskAction&quot;** 以确定工作流的路由。 此 **&quot;lastTaskAction&quot;** 属性设置为步骤4中配置的路由选项卡中的值之一。 对于每个“分支”选项卡，填写 **脚本** 文本区域包含以下值：

   ```
   function check() {
   var lastAction = workflowData.getMetaDataMap().get("lastTaskAction","");
   
   if(lastAction == "Normal Approval") {
       return true;
   }
   
   return false;
   }
   ```

   ```
   function check() {
   var lastAction = workflowData.getMetaDataMap().get("lastTaskAction","");
   
   if(lastAction == "Rush Approval") {
       return true;
   }
   
   return false;
   }
   ```

   ```
   function check() {
   var lastAction = workflowData.getMetaDataMap().get("lastTaskAction","");
   
   if(lastAction == "Bypass Approval") {
       return true;
   }
   
   return false;
   }
   ```

   &#42;请注意，我们正在执行直接字符串匹配以确定路由，因此分支脚本中设置的值与步骤4中设置的路由值匹配非常重要。

1. 拖放另一个»**创建项目任务**”在OR拆分部分下方向模型的最左侧（分支1）步进。 使用以下属性填写对话框：

   ```
   Common Tab
   -----------------
       Title = "Approval Task Creation"
       Description = "Create a an approval task for Project Approvers. Priority is Medium."
       Workflow Stage = "Approval"
   
   Task Tab
   ------------
       Name* = "Approve Content for Publish"
       Task Priority = "Medium"
       Description = "Approve this content for publication."
       Days = "5"
   
   Routing Tab - Actions
   ----------------------------
       "Approve and Publish"
       "Send Back for Revision"
   ```

   由于这是正常审批路由，因此任务的优先级被设置为“中”。 此外，我们给批准者组五天时间来完成任务。 “任务”选项卡上的“被分派人”留空，因为我们将在“高级设置”选项卡中动态分配此项。 完成此任务时，我们为批准者组提供了两种可能的路由： **&quot;批准并发布&quot;** 如果他们批准该内容，则可以发布该内容，并且 **&quot;发送回以进行修订&quot;** 如果存在原始编辑器需要更正的问题。 批准者可以留下注释，如果工作流返回给他/她，原始编辑者将看到这些注释。

在本教程的前面部分，我们创建了一个项目模板，其中包含批准者角色。 每次从此模板创建新项目时，都会为批准者角色创建一个特定于项目的组。 与参与者步骤一样，任务只能分配给用户或组。 我们希望将此任务分配给与批准者组对应的项目组。 从项目内启动的所有工作流都将具有元数据，这些元数据会将项目角色映射到项目特定的组。

将以下代码复制并粘贴到 **脚本** **高级设置**选项卡的文本区域。 此代码将读取工作流元数据，并将任务分配给项目的批准者组。 如果找不到批准者组值，它将回退以将任务分配给管理员组。

```
var projectApproverGrp = workflowData.getMetaDataMap().get("project.group.approvers","administrators");

task.setCurrentAssignee(projectApproverGrp);
```

1. 拖放另一个»**创建项目任务**”向模型靠近OR拆分线下的中间分支（分支2）。 使用以下属性填写对话框：

   ```
   Common Tab
   -----------------
       Title = "Rush Approval Task Creation"
       Description = "Create a an approval task for Project Approvers. Priority is High."
       Workflow Stage = "Approval"
   
   Task Tab
   ------------
       Name* = "Rush Approve Content for Publish"
       Task Priority = "High"
       Description = "Rush approve this content for publication."
       Days = "1"
   
   Routing Tab - Actions
   ----------------------------
       "Approve and Publish"
       "Send Back for Revision"
   ```

   由于这是紧急批准路由，因此任务的优先级被设置为“高”。 此外，我们只给批准者组一天时间来完成任务。 “任务”选项卡上的“被分派人”留空，因为我们将在“高级设置”选项卡中动态分配此项。

   我们可以重复使用与步骤7中相同的脚本代码片段来填充 **脚本** “高级设置”选项卡上**文本区域** 复制并粘贴以下代码：

   ```
   var projectApproverGrp = workflowData.getMetaDataMap().get("project.group.approvers","administrators");
   
   task.setCurrentAssignee(projectApproverGrp);
   ```

1. 将a**无操作**组件拖放到最右侧分支（分支3）中。 “无操作”组件不执行任何操作，它将立即进入高级状态，这表示原始编辑者希望绕过批准步骤。 从技术上讲，我们可以离开此分支而不执行任何工作流步骤，但作为最佳实践，我们将添加“不操作”步骤。 这使其他开发人员清楚地了解了分支3的用途。

   双击工作流步骤并配置标题和描述：

   ```
   Common Tab
   -----------------
       Title = "Bypass Approval"
       Description = "Placeholder step to indicate that the original editor decided to bypass the approver group."
   ```

   ![工作流模型或拆分](./assets/develop-aem-projects/workflow-stage-after-orsplit.png)

   在配置OR拆分中的所有三个分支后，工作流模型应如下所示。

1. 由于批准者组可以选择将工作流发送回原始编辑器以进行进一步修订，因此我们将依赖 **转至** 读取上次执行的操作并将工作流路由到起始位置的步骤，或让工作流继续执行的步骤。

   将“转至步骤”组件（可在“工作流”下的Sidekick中找到）拖放到重新联接处的OR拆分下。 双击并在对话框中配置以下属性：

   ```
   Common Tab
   ----------------
       Title = "Goto Step"
       Description = "Based on the Approver groups action route the workflow to the beginning or continue and publish the payload."
   
   Process Tab
   ---------------
       The step to go to. = "Start Task Creation"
   ```

   我们将配置的最后一个部分是作为“转至”流程步骤一部分的脚本。 脚本值可以通过对话框嵌入，也可以配置为指向外部文件。 Goto脚本必须包含 **函数check()** 如果工作流应转到指定的步骤，则和返回true。 返回false会导致工作流继续运行。

   如果审批人组选择 **&quot;发送回以进行修订&quot;** 操作（在步骤7和8中配置），然后我们想要将工作流返回到 **&quot;开始任务创建&quot;** 步骤。

   在“进程”选项卡上，将以下代码片段添加到“脚本”文本区域：

   ```
   function check() {
   var lastAction = workflowData.getMetaDataMap().get("lastTaskAction","");
   
   if(lastAction == "Send Back for Revision") {
       return true;
   }
   
   return false;
   }
   ```

1. 要发布有效负载，我们将使用ootb **激活页面/资产** 流程步骤。 此流程步骤几乎不需要进行配置，并且会将工作流的负载添加到复制队列以进行激活。 我们将在“转至”步骤下添加步骤，通过此方式，只有在审批者组已审批要发布的内容或原始编辑者选择“略过审批”路径时，才能访问此步骤。

   拖放 **激活页面/资产** 在模型中“转至”步骤下的“进程”步骤（可在WCM工作流下的Sidekick中找到）。

   ![工作流模型完成](assets/develop-aem-projects/workflow-model-final.png)

   添加“转至”步骤和“激活页面/资产”步骤后，工作流模型应是什么样的。

1. 如果“审批者”组将内容发送回以进行修订，我们想让原始编辑者知道。 我们可以通过动态更改“任务”创建属性来完成此操作。 我们将键入以下项的lastActionTaken属性值： **&quot;发送回以进行修订&quot;**. 如果该值存在，我们将修改标题和描述，以表明此任务是由发送回以进行修订的内容导致的。 我们还将更新优先级 **&quot;高&quot;** 以使其成为编辑器处理的第一个项目。 最后，我们将任务到期日设置为从工作流发送回以进行修订起的一天。

   替换开头 `start-task-config.ecma` 脚本（在步骤5中创建），包含以下内容：

   ```
   // start-task-config.ecma
   // Populate the task using values stored as workflow metadata originally posted by the start workflow wizard
   
   // set the assignee based on start workflow wizard
   var assignee = workflowData.getMetaDataMap().get("assignee", Packages.java.lang.String);
   task.setCurrentAssignee(assignee);
   
   //Set the due date for the initial task based on start workflow wizard
   var dueDate = workflowData.getMetaDataMap().get("taskDueDate", Packages.java.util.Date);
   if (dueDate != null) {
       task.setProperty("taskDueDate", dueDate);
   }
   
   //Set the priority based on start workflow wizard
   var taskPriority = workflowData.getMetaDataMap().get("taskPriority", "Medium");
   task.setProperty("taskPriority", taskPriority);
   
   var lastAction = workflowData.getMetaDataMap().get("lastTaskAction","");
   
   //change the title and priority if the approver group sent back the content
   if(lastAction == "Send Back for Revision") {
     var taskName = "Review and Revise Content";
   
     //since the content was rejected we will set the priority to High for the revison task
     task.setProperty("taskPriority", "High"); 
   
     //set the Task name (displayed as the task title in the Inbox) 
     task.setProperty("name", taskName);
     task.setProperty("nameHierarchy", taskName);
   
     //set the due date of this task 1 day from current date
     var calDueDate = Packages.java.util.Calendar.getInstance();
     calDueDate.add(Packages.java.util.Calendar.DATE, 1);
     task.setProperty("taskDueDate", calDueDate.getTime());
   
   }
   ```

## 创建“启动工作流”向导 {#start-workflow-wizard}

从项目内启动工作流时，必须指定向导以启动工作流。 默认向导： `/libs/cq/core/content/projects/workflowwizards/default_workflow` 允许用户为要运行的工作流输入工作流标题、开始注释和有效负荷路径。 下面还提供了其他几个示例： `/libs/cq/core/content/projects/workflowwizards`.

创建自定义向导的功能非常强大，因为您可以在工作流启动之前收集关键信息。 数据将作为工作流元数据的一部分存储，工作流进程可以读取此数据，并根据输入的值动态更改行为。 我们将创建一个自定义向导，以根据启动向导值动态分配工作流中的第一个任务。

1. 在CRXDE-Lite中，我们将在下创建子文件夹 `/apps/aem-guides/projects-tasks/projects` 名为“向导”的文件夹。 从以下位置复制默认向导： `/libs/cq/core/content/projects/workflowwizards/default_workflow` 在新创建的向导文件夹下方，并将其重命名为 **content-approval-start**. 现在，完整路径应为： `/apps/aem-guides/projects-tasks/projects/wizards/content-approval-start`.

   默认向导是一个两列向导，其中第一列显示所选工作流模型的标题、描述和缩略图。 第二列包含工作流标题、开始注释和有效负载路径的字段。 该向导是标准的触屏UI表单，并且使用标准 [Granite UI表单组件](https://experienceleague.adobe.com/docs/) 以填充字段。

   ![内容审批工作流向导](./assets/develop-aem-projects/content-approval-start-wizard.png)

1. 我们将在向导中添加一个附加字段，用于设置工作流中第一个任务的被分配人(请参阅 [创建工作流模型](#create-workflow-model)：步骤5)。

   下方 `../content-approval-start/jcr:content/items/column2/items` 创建类型为的新节点 `nt:unstructured` 已命名 **&quot;assign&quot;**. 我们将使用项目用户选取器组件(基于 [Granite用户选取器组件](https://experienceleague.adobe.com/docs/))。 利用此表单字段，可以轻松地将用户和组选择限制为仅属于当前项目的用户和组。

   以下是 **分配** 节点：

   ```xml
   <assign
       granite:class="js-cq-project-user-picker"
       jcr:primaryType="nt:unstructured"
       sling:resourceType="cq/gui/components/projects/admin/userpicker"
       fieldLabel="Assign To"
       hideServiceUsers="{Boolean}true"
       impersonatesOnly="{Boolean}true"
       showOnlyProjectMembers="{Boolean}true"
       name="assignee"
       projectPath="${param.project}"
       required="{Boolean}true"/>
   ```

1. 我们还将添加一个优先级选择字段，用于确定工作流中第一个任务的优先级(请参阅 [创建工作流模型](#create-workflow-model)：步骤5)。

   下方 `/content-approval-start/jcr:content/items/column2/items` 创建类型为的新节点 `nt:unstructured` 已命名 **优先级**. 我们将使用 [Granite UI Select组件](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions.html?lang=zh-Hans) 以填充表单字段。

   在 **优先级** 节点，我们将添加 **个项目** 节点 **nt：unstructured**. 在 **个项目** 节点再添加3个节点以填充“高”、“中”和“低”的选择选项。 每个节点的类型为 **nt：unstructured** 并且应该有一个 **text** 和 **值** 属性。 文本和值应该为相同的值：

   1. 高
   1. 中
   1. 低

   对于Medium节点，请添加一个名为&#39;&#39;的附加布尔属性&#x200B;**已选择”** 值设置为 **true**. 这将确保选择字段中的“中”为默认值。

   以下是节点结构和属性的XML表示形式：

   ```xml
   <priority
       jcr:primaryType="nt:unstructured"
       sling:resourceType="granite/ui/components/coral/foundation/form/select"
       fieldLabel="Task Priority"
       name="taskPriority">
           <items jcr:primaryType="nt:unstructured">
               <high
                   jcr:primaryType="nt:unstructured"
                   text="High"
                   value="High"/>
               <medium
                   jcr:primaryType="nt:unstructured"
                   selected="{Boolean}true"
                   text="Medium"
                   value="Medium"/>
               <low
                   jcr:primaryType="nt:unstructured"
                   text="Low"
                   value="Low"/>
               </items>
   </priority>
   ```

1. 我们将允许工作流发起者设置初始任务的截止日期。 我们将使用 [Granite UI日期选取器](https://experienceleague.adobe.com/docs/) 表单字段以捕获此输入。 我们还将添加一个隐藏字段，其中包含 [TypeHint](https://sling.apache.org/documentation/bundles/manipulating-content-the-slingpostservlet-servlets-post.html#typehint) 以确保输入作为Date type属性存储在JCR中。

   添加两个 **nt：unstructured** 以XML格式表示具有以下属性的节点：

   ```xml
   <duedate
       granite:rel="project-duedate"
       jcr:primaryType="nt:unstructured"
       sling:resourceType="granite/ui/components/coral/foundation/form/datepicker"
       displayedFormat="YYYY-MM-DD HH:mm"
       fieldLabel="Due Date"
       minDate="today"
       name="taskDueDate"
       type="datetime"/>
   <duedatetypehint
       jcr:primaryType="nt:unstructured"
       sling:resourceType="granite/ui/components/coral/foundation/form/hidden"
       name="taskDueDate@TypeHint"
       type="datetime"
       value="Calendar"/>
   ```

1. 您可以查看启动向导对话框的完整代码 [此处](https://github.com/Adobe-Marketing-Cloud/aem-guides/blob/master/projects-tasks-guide/ui.apps/src/main/content/jcr_root/apps/aem-guides/projects-tasks/projects/wizards/content-approval-start/.content.xml).

## 连接工作流和项目模板 {#connecting-workflow-project}

我们最后需要做的就是确保工作流模型可以从其中一个项目中启动。 为此，我们需要重新访问在本系列第1部分中创建的项目模板。

工作流配置是项目模板的一个区域，用于指定要用于该项目的可用工作流。 该配置还负责在启动工作流时指定“启动工作流向导”(我们在 [之前的步骤)](#start-workflow-wizard). 项目模板的工作流配置处于“实时”状态，这意味着更新工作流配置将会影响创建的新项目以及使用该模板的现有项目。

1. 在CRXDE-Lite中，导航到之前创建的创作项目模板 `/apps/aem-guides/projects-tasks/projects/templates/authoring-project/workflows/models`.

   在模型节点下，添加名为的新节点 **contentapproval** 节点类型为 **nt：unstructured**. 将以下属性添加到节点：

   ```xml
   <contentapproval
       jcr:primaryType="nt:unstructured"
       modelId="/etc/workflow/models/aem-guides/content-approval-workflow/jcr:content/model"
       wizard="/apps/aem-guides/projects-tasks/projects/wizards/content-approval-start.html"
   />
   ```

   >[!NOTE]
   >
   >如果使用AEM 6.4，则工作流的位置已更改。 指向 `modelId` 属性到下的运行时工作流模型的位置 `/var/workflow/models/aem-guides/content-approval-workflow`
   >
   >
   >参见 [此处了解有关工作流位置更改的更多详细信息。](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/workflows-best-practices.html#LocationsWorkflowModels)

   ```xml
   <contentapproval
       jcr:primaryType="nt:unstructured"
       modelId="/var/workflow/models/aem-guides/content-approval-workflow"
       wizard="/apps/aem-guides/projects-tasks/projects/wizards/content-approval-start.html"
   />
   ```

1. 将内容审批工作流添加到项目模板后，该工作流应该可以从项目的“工作流”拼贴启动。 继续进行并启动和尝试我们创建的各种工艺路线。

## 支持材料

* [下载完成的教程包](./assets/develop-aem-projects/projects-tasks-guide.ui.apps-0.0.1-SNAPSHOT.zip)
* [GitHub上的完整代码存储库](https://github.com/Adobe-Marketing-Cloud/aem-guides/tree/feature/projects-tasks-guide)
* [AEM项目文档](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/projects.html)
