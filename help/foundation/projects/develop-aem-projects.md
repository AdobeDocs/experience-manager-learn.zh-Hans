---
title: 在AEM中开发项目
description: 一个开发教程，其中演示了如何为AEM项目开发。 在本教程中，我们将创建一个自定义项目模板，该模板可用于在AEM中创建新项目，以管理内容创作工作流和任务。
version: Experience Manager 6.4, Experience Manager 6.5
feature: Projects, Workflow
doc-type: Tutorial
topic: Development
role: Developer
level: Beginner
exl-id: 9bfe3142-bfc1-4886-85ea-d1c6de903484
duration: 1417
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '4441'
ht-degree: 0%

---

# 在AEM中开发项目

这是一个开发教程，说明如何为[!DNL AEM Projects]开发。 在本教程中，我们将创建一个自定义项目模板，该模板可用于在AEM中创建用于管理内容创作工作流和任务的项目。

>[!VIDEO](https://video.tv.adobe.com/v/16904?quality=12&learn=on)

*此视频简要演示了以下教程中创建的已完成工作流程。*

## 简介 {#introduction}

[[!DNL AEM Projects]](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-65/content/sites/authoring/projects/projects)是AEM的一项功能，旨在简化对作为AEM Sites或Assets实施的一部分与内容创建相关的所有工作流和任务的管理和分组。

AEM项目附带多个[OOTB项目模板](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-65/content/sites/authoring/projects/projects)。 创建项目时，作者可以从这些可用的模板中进行选择。 具有独特业务需求的大型AEM实施将要创建定制的项目模板，以便满足其需求。 通过创建自定义项目模板，开发人员可以配置项目仪表板、挂接到自定义工作流，并为项目创建其他业务角色。 我们将查看项目模板的结构并创建一个示例模板。

![自定义项目卡](./assets/develop-aem-projects/custom-project-card.png)

## 设置

本教程将逐步介绍创建自定义项目模板所需的代码。 您可以将[附加的包](./assets/develop-aem-projects/projects-tasks-guide.ui.apps-0.0.1-SNAPSHOT.zip)下载并安装到本地环境，以便与教程一起阅读。 您还可以访问在[GitHub](https://github.com/Adobe-Marketing-Cloud/aem-guides/tree/feature/projects-tasks-guide)上托管的完整Maven项目。

* [已完成的教程包](./assets/develop-aem-projects/projects-tasks-guide.ui.apps-0.0.1-SNAPSHOT.zip)
* GitHub上的[完整代码存储库](https://github.com/Adobe-Marketing-Cloud/aem-guides/tree/feature/projects-tasks-guide)

本教程假定您了解[AEM开发实践](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-65/content/implementing/developing/introduction/the-basics)的一些基本知识以及对[AEM Maven项目设置](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/ht-projects-maven.html?lang=zh-Hans)的一些了解。 所有提及的代码均旨在用作参考，并且只应部署到[本地开发AEM实例](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-65/content/implementing/deploying/deploying/deploy)。

## 项目模板的结构

项目模板应该置于源代码管理下，并且应该位于/apps下的应用程序文件夹下。 理想情况下，应将它们放置在命名惯例为&#x200B;**&#42;/projects/templates/**&lt;my-template>的子文件夹中。 通过遵循此命名约定，任何新的自定义模板都将在创建项目时自动可供作者使用。 可用项目模板的配置由&#x200B;**cq：allowedTemplates**&#x200B;属性设置为&#x200B;**/content/projects/jcr：content**&#x200B;节点。 默认情况下，这是正则表达式： **/(apps|libs)/。&#42;/项目/模板/.&#42;**

项目模板的根节点将具有&#x200B;**cq：Template**&#x200B;的&#x200B;**jcr：primaryType**。 在根节点下有三个节点： **小工具**、**角色**&#x200B;和&#x200B;**工作流**。 这些节点都是&#x200B;**nt：unstructured**。 根节点下方也可以是一个thumbnail.png文件，在“创建项目”向导中选择模板时显示该文件。

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

项目模板的根节点为&#x200B;**cq：Template**&#x200B;类型。 在此节点上，您可以配置在“创建项目向导”中显示的属性&#x200B;**jcr：title**&#x200B;和&#x200B;**jcr：description**。 还有一个名为&#x200B;**wizard**&#x200B;的属性，它指向将填充项目属性的表单。 默认值： **/libs/cq/core/content/projects/wizard/steps/defaultproject.html**&#x200B;在大多数情况下应该可以正常使用，因为它允许用户填充基本的项目属性并添加组成员。

*&#42;请注意，“创建项目向导”不使用Sling POST servlet。 而是将值发布到自定义servlet：**com.adobe.cq.projects.impl.servlet.ProjectServlet**。 添加自定义字段时应考虑这一点。*

可以为翻译项目模板找到自定义向导示例： **/libs/cq/core/content/projects/wizard/translationproject/defaultproject**。

### 小工具 {#gadgets}

此节点上没有其他属性，但小工具的子节点控制创建新项目时用于填充项目仪表板中的项目拼贴。 [项目磁贴](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-65/content/sites/authoring/projects/projects)（也称为小工具或pod）是填充项目工作区的简单卡片。 您可以在&#x200B;**/libs/cq/gui/components/projects/admin/pod下找到ootb图块的完整列表。 &#x200B;** 在创建项目后，项目所有者始终可以添加/删除图块。

### 角色 {#roles}

每个项目有三个[默认角色](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-65/content/sites/authoring/projects/projects)：**观察者**、**编辑器**&#x200B;和&#x200B;**所有者**。 通过在角色节点下添加子节点，可以为模板添加其他特定于业务的项目角色。 然后，您可以将这些角色关联到与项目关联的特定工作流。

### 工作流 {#workflows}

创建自定义项目模板的一个最诱人的原因是，它使您能够配置可用于项目的工作流。 这些工作流可以是OOTB工作流或自定义工作流。 在&#x200B;**工作流**&#x200B;节点下，需要一个&#x200B;**模型**&#x200B;节点（也是`nt:unstructured`）以及指定可用工作流模型的子节点。 属性&#x200B;**modelId**&#x200B;指向/etc/workflow下的工作流模型，属性&#x200B;**wizard**&#x200B;指向启动工作流时使用的对话框。 Projects的一个显着优势是能够添加自定义对话框（向导），以在工作流开始时捕获特定于业务的元数据，从而推动工作流中的进一步操作。

```shell
<projects-template-root> (cq:Template)
    + workflows (nt:unstructured)
         + models (nt:unstructured)
              + <workflow-model> (nt:unstructured)
                   - modelId = points to the workflow model
                   - wizard = dialog used to start the workflow
```

## 创建项目模板 {#creating-project-template}

由于我们主要复制/配置节点，因此将使用CRXDE Lite。 在本地AEM实例中，打开[CRXDE Lite](http://localhost:4502/crx/de/index.jsp)。

1. 首先，在`/apps/&lt;your-app-folder&gt;`下创建一个名为`projects`的文件夹。 在该名为`templates`的文件夹下创建另一个文件夹。

   ```shell
   /apps/aem-guides/projects-tasks/
                       + projects (nt:folder)
                                + templates (nt:folder)
   ```

1. 为了简化操作，我们将从现有的“简单项目”模板开始自定义模板。

   1. 将节点&#x200B;**/libs/cq/core/content/projects/templates/default**&#x200B;复制并粘贴到步骤1中创建的&#x200B;*templates*&#x200B;文件夹下。

   ```shell
   /apps/aem-guides/projects-tasks/
                + templates (nt:folder)
                     + default (cq:Template)
   ```

1. 您现在应具有类似于&#x200B;**/apps/aem-guides/projects-tasks/projects/templates/authoring-project**&#x200B;的路径。

   1. 编辑author-project节点的&#x200B;**jcr：title**&#x200B;和&#x200B;**jcr：description**&#x200B;属性以自定义标题和描述值。

      1. 将&#x200B;**向导**&#x200B;属性保留为指向默认的项目属性。

   ```shell
   /apps/aem-guides/projects-tasks/projects/
            + templates (nt:folder)
                 + authoring-project (cq:Template)
                      - jcr:title = "Authoring Project"
                      - jcr:description = "A project to manage approval and publish process for AEM Sites or Assets"
                      - wizard = "/libs/cq/core/content/projects/wizard/steps/defaultproject.html"
   ```

1. 对于此项目模板，我们希望使用“任务”。
   1. 在名为&#x200B;**tasks**&#x200B;的authoring-project/gadgets下添加新的&#x200B;**nt：unstructured**&#x200B;节点。
   1. 将String属性添加到&#x200B;**cardWeight** = &quot;100&quot;、**jcr：title**=&quot;Tasks&quot;和&#x200B;**sling：resourceType**=&quot;cq/gui/components/projects/admin/pod/taskpod&quot;的任务节点。

   现在，在创建新项目时，默认情况下将显示[任务拼贴](https://experienceleague.adobe.com/zh-hans/docs)。

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

   1. 在项目模板(authoring-project)节点下添加一个新的&#x200B;**nt：unstructured**&#x200B;节点，标记为&#x200B;**角色**。
   1. 将另一个标记为批准者的&#x200B;**nt：unstructured**&#x200B;节点添加为角色节点的子节点。
   1. 添加字符串属性&#x200B;**jcr：title** = &quot;**审批者**&quot;，**roleclass** =&quot;**所有者**&quot;，**roleid**=&quot;**审批者**&quot;。
      1. 审批者节点的名称以及jcr：title和roleid可以是任何字符串值（只要roleid是唯一的）。
      1. **roleclass**&#x200B;根据[三个OOTB角色](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-65/content/sites/authoring/projects/projects)控制应用于该角色的权限： **所有者**、**编辑者**&#x200B;和&#x200B;**观察者**。
      1. 一般来说，如果自定义角色更像是管理角色，则角色可以是&#x200B;**所有者；**&#x200B;如果自定义角色更像是摄影师或Designer之类的创作角色，则&#x200B;**编辑者**&#x200B;角色就足够了。 **所有者**&#x200B;和&#x200B;**编辑者**&#x200B;之间的主要区别在于，项目所有者可以更新项目属性并将新用户添加到项目中。

   ```shell
   ../projects/templates/authoring-project
       + gadgets (nt:unstructured)
       + roles (nt:unstructured)
           + approvers (nt:unstructured)
                - jcr:title = "Approvers"
                - roleclass = "owner"
                - roleid = "approver"
   ```

1. 通过复制简单项目模板，您将配置四个OOTB工作流。 工作流/模型下的每个节点都指向该工作流的特定工作流和开始对话框向导。 在本教程的后面部分，我们将为此项目创建一个自定义工作流。 现在，请删除工作流/模型下的节点：

   ```shell
   ../projects/templates/authoring-project
       + gadgets (nt:unstructured)
       + roles (nt:unstructured)
       + workflows (nt:unstructured)
            + models (nt:unstructured)
               - (remove ootb models)
   ```

1. 为了便于内容作者识别项目模板，您可以添加自定义缩略图。 建议的大小为319x319像素。
   1. 在CRXDE Lite中，将文件创建为名为&#x200B;**thumbnail.png**&#x200B;的小工具、角色和工作流节点的同级。
   1. 保存，然后导航到`jcr:content`节点并双击`jcr:data`属性（避免单击“查看”）。
      1. 这将提示您编辑`jcr:data`文件对话框，您可以上传自定义缩略图。

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

现在，我们可以通过创建项目来测试项目模板。

1. 您应该看到自定义模板作为创建项目的选项之一。

   ![选择模板](./assets/develop-aem-projects/choose-template.png)

1. 选择自定义模板后，单击“下一步”，并注意在填充项目成员时，您可以将其添加为审批者角色。

   ![批准](./assets/develop-aem-projects/user-approver.png)

1. 单击“创建”以根据自定义模板完成项目创建。 您会注意到“项目功能板”上的“任务拼贴”以及在小工具下配置的其他拼贴会自动显示。

   ![任务拼贴](./assets/develop-aem-projects/tasks-tile.png)


## 为什么选择工作流？

传统上，以审批流程为中心的AEM工作流已使用参与者工作流步骤。 AEM的收件箱中包含有关任务和工作流的详细信息，并增强了与AEM项目的集成。 这些功能使得使用项目创建任务流程步骤成为一个更具吸引力的选项。

### 为何要执行任务？

与传统“参与者”步骤相比，使用“任务创建步骤”具有如下优点：

* **开始和到期日期** — 使作者能够轻松管理其时间，新的日历功能将使用这些日期。
* **优先级** — 内置优先级为“低”、“正常”和“高”的优先级允许作者优先处理工作
* **线程注释** — 当作者处理任务时，他们能够留下注释，从而增加协作能力
* **可见性** — 具有项目的任务磁贴和视图允许经理查看时间的使用情况
* **项目集成** — 任务已与项目角色和功能板集成

与参与者步骤类似，任务也可以动态分配和路由。 标题、优先级等任务元数据也可以根据之前的操作动态设置，如下面的教程所示。

虽然与参与者步骤相比，任务具有一些优势，但它们的确会产生额外的开销，并且在项目之外不那么有用。 此外，任务的所有动态行为都必须使用具有自身限制的ecma脚本进行编码。

## 示例用例要求 {#goals-tutorial}

![工作流流程图](./assets/develop-aem-projects/workflow-process-diagram.png)

上图概述了我们的示例审批工作流的概要要求。

第一步是创建一个Task以完成内容的编辑。 我们将允许工作流发起者选择此第一项任务的被分配人。

完成第一个任务后，被分派人将有三个选项来路由工作流：

**普通** — 普通路由会创建一项任务，该任务将分配给项目的审批者组进行审阅和批准。 任务的优先级为正常，截止日期为创建任务后的五天。

**Rush** — 快速路由还会创建分配给项目审批者组的任务。 任务的优先级为“高”，截止日期只有一天。

**绕过** — 在此示例工作流中，初始参与者可以选择绕过审批组。 （是的，这可能会破坏“审批”工作流的用途，但它允许我们说明额外的路由选择功能）

审批者组可以审批内容，也可以将内容发回初始被分配人重新工作。 如果被发回重工，则会创建一个新任务并相应地标记为“发回重工”。

工作流的最后一个步骤使用ootb激活页面/资产流程步骤并复制有效负载。

## 创建工作流模型

1. 从AEM的“开始”菜单中，导航到工具 — >工作流 — >模型。 单击右上角的“创建”以创建工作流模型。

   为新模型指定标题“内容审批工作流”和URL名称“content-approval-workflow”。

   ![工作流创建对话框](./assets/develop-aem-projects/workflow-create-dialog.png)

   [有关创建工作流的详细信息，请阅读此处](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-65/content/implementing/developing/extending-aem/extending-workflows/workflows-models)。

1. 作为最佳实践，自定义工作流应分组到/etc/workflow/models下方的各自文件夹中。 在CRXDE Lite中，在名为&#x200B;**&quot;aem-guides&quot;**&#x200B;的/etc/workflow/models下创建一个&#x200B;**&quot;nt：folder&quot;**。 添加子文件夹可确保自定义工作流在升级或Service Pack安装期间不会意外覆盖。

   &#42;请注意，切勿将文件夹或自定义工作流放置在ootb子文件夹（如/etc/workflow/models/dam或/etc/workflow/models/projects）下，因为升级或Service Pack也可能会覆盖整个子文件夹。

   ![工作流模型在6.3](./assets/develop-aem-projects/custom-workflow-subfolder.png)中的位置

   工作流模型在6.3中的位置

   >[!NOTE]
   >
   >如果使用AEM 6.4+，则工作流的位置已更改。 查看[此处，了解更多详细信息。](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-65/content/implementing/developing/extending-aem/extending-workflows/workflows-best-practices)

   如果使用AEM 6.4+，将在`/conf/global/settings/workflow/models`下创建工作流模型。 对/conf目录重复上述步骤，添加名为`aem-guides`的子文件夹，并将`content-approval-workflow`移到其下方。

   ![现代工作流定义位置](./assets/develop-aem-projects/modern-workflow-definition-location.png)
工作流模型在6.4+中的位置

1. AEM 6.3中引入了将工作流暂存添加到给定工作流的功能。 这些暂存将显示在“工作流信息”选项卡的“收件箱”中，供用户使用。 它将向用户显示工作流中的当前阶段以及工作流之前和之后的阶段。

   要配置阶段，请从Sidekick中打开页面属性对话框。 第四个选项卡标记为“暂存”。 添加以下值以配置此工作流的三个阶段：

   1. 编辑内容
   1. 审批
   1. 发布

   ![工作流暂存配置](./assets/develop-aem-projects/workflow-model-stage-properties.png)

   从“页面属性”对话框中配置工作流暂存。

   ![工作流进度条](./assets/develop-aem-projects/workflow-info-progress.png)

   从AEM收件箱中看到的工作流进度条。

   （可选）您可以将&#x200B;**图像**&#x200B;上传到页面属性，该属性在用户选择时用作工作流缩略图。 图像尺寸应为319x319像素。 当用户选择工作流时，也会显示向页面属性添加&#x200B;**Description**。

1. “创建项目任务”工作流进程旨在创建作为工作流步骤的任务。 只有在完成任务后，工作流才会前进。 “创建项目任务”步骤的一个强大方面是，它可以读取工作流元数据值，并使用这些值动态创建任务。

   首先删除默认创建的参与者步骤。 从Sidekick的“组件”菜单中，展开&#x200B;**“项目”**&#x200B;子标题，并将&#x200B;**“创建项目任务”**&#x200B;拖放到模型上。

   双击“创建项目任务”步骤以打开工作流对话框。 配置以下属性：

   此选项卡在所有工作流流程步骤中都是通用的，我们将设置标题和描述（最终用户看不到这两个标题）。 我们将设置的重要属性是从下拉菜单中将“工作流暂存”设置为&#x200B;**“编辑内容”**。

   ```shell
   Common Tab
   -----------------
       Title = "Start Task Creation"
       Description = "This the first task in the Workflow"
       Workflow Stage = "Edit Content"
   ```

   “创建项目任务”工作流进程旨在创建作为工作流步骤的任务。 “任务”选项卡允许我们设置任务的所有值。 在本例中，我们希望被分派人具有动态性，因此我们将保留为空。 其余属性值：

   ```shell
   Task Tab
   -----------------
       Name* = "Edit Content"
       Task Priority = "Medium"
       Description = "Edit the content and finalize for approval. Once finished submit for approval."
       Due In - Days = "2"
   ```

   “路由”选项卡是一个可选的对话框，可为完成任务的用户指定可用的操作。 这些操作只是字符串值，且会保存到工作流的元数据中。 可以通过脚本读取这些值，和/或稍后在工作流中执行流程步骤以动态“路由”工作流。 根据我们将要执行的工作流目标，向此选项卡中添加三个操作：

   ```shell
   Routing Tab
   -----------------
       Actions =
           "Normal Approval"
           "Rush Approval"
           "Bypass Approval"
   ```

   通过此选项卡，我们可以配置创建前任务脚本，在其中我们可以在创建任务之前以编程方式决定任务的各种值。 我们可以选择将脚本指向外部文件，或者直接在对话框中嵌入简短脚本。 在我们的示例中，我们将预创建任务脚本指向外部文件。 在步骤5中，我们将创建该脚本。

   ```shell
   Advanced Settings Tab
   -----------------
      Pre-Create Task Script = "/apps/aem-guides/projects/scripts/start-task-config.ecma"
   ```

1. 在上一步中，我们引用了一个预创建任务脚本。 我们现在将创建该脚本，其中将根据工作流元数据值&quot;**代理人**&quot;的值设置任务的代理人。 工作流启动时设置了&#x200B;**“代理人”**&#x200B;值。 我们还将读取工作流元数据，以通过读取工作流元数据的“**taskPriority”**&#x200B;值以及用于在第一个任务到期时动态设置的&#x200B;**“taskDueDate”**&#x200B;来动态选择任务的优先级。

   出于组织目的，我们在应用程序文件夹下创建了一个文件夹，用于保存所有与项目相关的脚本： **/apps/aem-guides/projects-tasks/projects/scripts**。 在此文件夹下创建名为&#x200B;**&quot;start-task-config.ecma&quot;**&#x200B;的文件。 &#42;注意：请确保start-task-config.ecma文件的路径与步骤4中“高级设置”选项卡中设置的路径相匹配。

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

1. 导航回内容审批工作流。 将&#x200B;**OR Split**&#x200B;组件(可在Sidekick中的“工作流”类别下找到)拖放到&#x200B;**开始任务**&#x200B;步骤下。 在“常用”对话框中，选择“3个分支”的单选按钮。 OR拆分将读取工作流元数据值&#x200B;**&quot;lastTaskAction&quot;**&#x200B;以确定工作流的路由。 **&quot;lastTaskAction&quot;**&#x200B;属性设置为步骤4中配置的路由选项卡中的值之一。 对于每个“分支”选项卡，使用以下值填写&#x200B;**脚本**&#x200B;文本区域：

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

   &#42;请注意，我们正在执行直接字符串匹配以确定路由，因此分支脚本中设置的值必须与步骤4中设置的路由值相匹配。

1. 将另一个&quot;**创建项目任务**&quot;步骤拖放到模型的最左侧（分支1）的OR拆分下方。 使用下列属性填写对话框：

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

   由于这是常规审批路由，因此任务的优先级将设置为Medium。 此外，我们给批准者组五天时间来完成任务。 “任务”选项卡上的“被分派人”留空，因为我们将在“高级设置”选项卡中动态分配此项。 完成此任务时，我们为批准者组提供两种可能的途径：如果批准内容并且可以发布，则为批准者组提供&#x200B;**“批准并发布”**；如果存在原始编辑器需要更正的问题，则为批准者组提供&#x200B;**“发送回以进行修订”**。 批准者可以添加注释，如果工作流返回给他/她，原始编辑者将看到这些注释。

在本教程的前面，我们创建了一个项目模板，其中包含批准者角色。 每次从此模板创建新项目时，都会为批准者角色创建项目特定的组。 就像参与者步骤一样，任务只能分配给用户或组。 我们希望将此任务分配给与批准者组对应的项目组。 从项目内启动的所有工作流都将具有元数据，这些元数据会将项目角色映射到项目特定的组。

在{高级设置}选项卡的&#x200B;**脚本&lbrace;1**&#x200B;文本区域中复制+粘贴以下代码&#x200B;**&#x200B;**&#x200B;此代码将读取工作流元数据并将任务分配给项目的审批者组。 如果找不到批准者组值，它将回退以将任务分配给管理员组。

```
var projectApproverGrp = workflowData.getMetaDataMap().get("project.group.approvers","administrators");

task.setCurrentAssignee(projectApproverGrp);
```

1. 将另一个&quot;**创建项目任务**&quot;步骤拖放到模型的OR拆分下的中间分支（分支2）中。 使用下列属性填写对话框：

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

   由于这是紧急批准路由，因此任务的优先级将设置为“高”。 此外，我们只给批准者组一天时间来完成任务。 “任务”选项卡上的“被分派人”留空，因为我们将在“高级设置”选项卡中动态分配此项。

   我们可以重复使用与步骤7中相同的脚本片段来填充&lbrace;**高级设置&lbrace;选项卡**&#x200B;上的&#x200B;**脚本**&#x200B;文本区域。 复制并粘贴以下代码：

   ```
   var projectApproverGrp = workflowData.getMetaDataMap().get("project.group.approvers","administrators");
   
   task.setCurrentAssignee(projectApproverGrp);
   ```

1. 将a&#x200B;**无操作**&#x200B;组件拖放到最右侧分支（分支3）中。 无操作组件不执行任何操作，它将会立即进行高级操作，表示原始编辑者希望绕过批准步骤。 从技术上讲，我们可以离开此分支而不执行任何工作流步骤，但作为最佳实践，我们将添加“不操作”步骤。 这使其他开发人员清楚了解分支3的用途。

   双击工作流步骤并配置标题和描述：

   ```
   Common Tab
   -----------------
       Title = "Bypass Approval"
       Description = "Placeholder step to indicate that the original editor decided to bypass the approver group."
   ```

   ![工作流模型OR拆分](./assets/develop-aem-projects/workflow-stage-after-orsplit.png)

   在配置OR拆分中的所有三个分支后，工作流模型应该如下所示。

1. 由于“审批者”组可以选择将工作流发送回原始编辑器以进行进一步修订，因此我们将依赖&#x200B;**转至**&#x200B;步骤来读取上次执行的操作并将工作流路由到开头，或者让它继续。

   将“跳转步骤”组件(可在Sidekick中的“工作流”下找到)拖放到重新联接处的OR拆分下。 双击并在对话框中配置以下属性：

   ```
   Common Tab
   ----------------
       Title = "Goto Step"
       Description = "Based on the Approver groups action route the workflow to the beginning or continue and publish the payload."
   
   Process Tab
   ---------------
       The step to go to. = "Start Task Creation"
   ```

   我们将配置的最后一个部分是作为“转至”流程步骤一部分的脚本。 脚本值可以通过对话框嵌入，也可以配置为指向外部文件。 Goto脚本必须包含&#x200B;**函数check()**，如果工作流应转到指定的步骤，则返回true。 返回false会导致工作流继续运行。

   如果审批者组选择&#x200B;**“Send Back for Revision”**&#x200B;操作（在步骤7和步骤8中配置），则我们希望将工作流返回到&#x200B;**“Start Task Creation”**&#x200B;步骤。

   在“流程”选项卡上，将以下代码片段添加到“脚本”文本区域：

   ```
   function check() {
   var lastAction = workflowData.getMetaDataMap().get("lastTaskAction","");
   
   if(lastAction == "Send Back for Revision") {
       return true;
   }
   
   return false;
   }
   ```

1. 为了发布有效负载，我们将使用ootb **激活页面/资产**&#x200B;流程步骤。 此流程步骤几乎不需要进行配置，并且会将工作流的负载添加到复制队列以进行激活。 我们将在“转至”步骤下添加步骤，以这种方式，只有在审批者组已审批要发布的内容或原始编辑者选择“略过审批”路由时，才能访问此步骤。

   将&#x200B;**激活页面/资产**&#x200B;流程步骤(可在Sidekick中的WCM工作流下找到)拖放到模型中的跳转步骤下。

   ![工作流模型完成](assets/develop-aem-projects/workflow-model-final.png)

   添加“转至”步骤和“激活页面/资产”步骤后工作流模型的外观。

1. 如果“审批者”组将内容发送回以进行修订，我们想让原始编辑器知道。 我们可以通过动态更改“任务”创建属性来实现这一点。 我们将关闭&#x200B;**“Send Back for Revision”**&#x200B;的lastActionTaked属性值。 如果存在该值，我们将修改标题和描述，以指示此任务是将内容发送回以进行修订的结果。 我们还将优先级更新为&#x200B;**“高”**，以便它是编辑器处理的第一个项目。 最后，我们将任务到期日设置为从发送回工作流以进行修订起的一天。

   将开始`start-task-config.ecma`脚本（在步骤5中创建）替换为以下内容：

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

从项目中启动工作流时，必须指定向导以启动工作流。 默认向导： `/libs/cq/core/content/projects/workflowwizards/default_workflow`允许用户为要运行的工作流输入工作流标题、启动注释和有效负荷路径。 在以下位置也发现了几个其他示例： `/libs/cq/core/content/projects/workflowwizards`。

创建自定义向导的功能非常强大，因为您可以在工作流启动之前收集关键信息。 数据存储为工作流元数据的一部分，工作流进程可以读取此数据，并根据输入的值动态更改行为。 我们将创建一个自定义向导，以根据启动向导值动态分配工作流中的第一个任务。

1. 在CRXDE-Lite中，我们将在`/apps/aem-guides/projects-tasks/projects`文件夹下创建一个名为“向导”的子文件夹。 从新建向导文件夹下方`/libs/cq/core/content/projects/workflowwizards/default_workflow`复制默认向导，并将其重命名为&#x200B;**content-approval-start**。 现在，完整路径应为： `/apps/aem-guides/projects-tasks/projects/wizards/content-approval-start`。

   默认向导是一个两列向导，其中第一列显示所选工作流模型的标题、描述和缩略图。 第二列包括工作流标题、开始注释和有效负载路径的字段。 该向导是标准的Touch UI表单，它使用标准的[Granite UI表单组件](https://experienceleague.adobe.com/zh-hans/docs)来填充字段。

   ![内容审批工作流向导](./assets/develop-aem-projects/content-approval-start-wizard.png)

1. 我们将在向导中添加一个附加字段，用于设置工作流中第一个任务的被分配人（请参阅[创建工作流模型](#create-workflow-model)：步骤5）。

   在`../content-approval-start/jcr:content/items/column2/items`下创建名为&#x200B;**&quot;assign&quot;**&#x200B;的类型为`nt:unstructured`的新节点。 我们将使用项目用户选取器组件（基于[Granite用户选取器组件](https://experienceleague.adobe.com/zh-hans/docs)）。 利用此表单字段，可以轻松地将用户和组选择限制为仅属于当前项目的用户和组。

   以下是&#x200B;**分配**&#x200B;节点的XML表示形式：

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

1. 我们还将添加一个优先级选择字段，用于确定工作流中第一个任务的优先级（请参阅[创建工作流模型](#create-workflow-model)：步骤5）。

   在`/content-approval-start/jcr:content/items/column2/items`下创建名为&#x200B;**优先级**&#x200B;的类型为`nt:unstructured`的新节点。 我们将使用[Granite UI Select组件](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions)填充表单字段。

   在&#x200B;**优先级**&#x200B;节点下，我们将添加&#x200B;**nt：unstructured**&#x200B;的&#x200B;**项**&#x200B;节点。 在&#x200B;**项**&#x200B;节点下添加另外3个节点以填充“高”、“Medium”和“低”的选择选项。 每个节点的类型为&#x200B;**nt：unstructured**，应具有&#x200B;**text**&#x200B;和&#x200B;**value**&#x200B;属性。 文本和值应为相同的值：

   1. 高
   1. 中
   1. 低

   对于Medium节点，添加名为“**selected”**&#x200B;的附加布尔属性，其值设置为&#x200B;**true**。 这将确保Medium是选择字段中的默认值。

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

1. 我们将允许工作流发起者设置初始任务的截止日期。 我们将使用[Granite UI DatePicker](https://experienceleague.adobe.com/zh-hans/docs)表单字段来捕获此输入。 我们还将添加一个具有[TypeHint](https://sling.apache.org/documentation/bundles/manipulating-content-the-slingpostservlet-servlets-post.html#typehint)的隐藏字段，以确保在JCR中将输入存储为Date type属性。

   添加两个&#x200B;**nt：unstructured**&#x200B;节点，这些节点的以下属性以XML形式表示：

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

1. 您可以在[此处](https://github.com/Adobe-Marketing-Cloud/aem-guides/blob/master/projects-tasks-guide/ui.apps/src/main/content/jcr_root/apps/aem-guides/projects-tasks/projects/wizards/content-approval-start/.content.xml)查看启动向导对话框的完整代码。

## 连接工作流和项目模板 {#connecting-workflow-project}

我们最后需要做的是确保可以从其中一个项目中启动工作流模型。 为此，我们需要重新访问在本系列第1部分中创建的项目模板。

工作流配置是项目模板的一个区域，用于指定要用于该项目的可用工作流。 该配置还负责在启动工作流（我们在[前面的步骤中创建）](#start-workflow-wizard)时指定“启动工作流向导”。 项目模板的工作流配置处于“实时”状态，这意味着更新工作流配置将会影响创建的新项目以及使用该模板的现有项目。

1. 在CRXDE-Lite中，导航到之前在`/apps/aem-guides/projects-tasks/projects/templates/authoring-project/workflows/models`创建的创作项目模板。

   在模型节点下添加名为&#x200B;**contentapproval**&#x200B;的新节点，该节点的节点类型为&#x200B;**nt：unstructured**。 将以下属性添加到节点：

   ```xml
   <contentapproval
       jcr:primaryType="nt:unstructured"
       modelId="/etc/workflow/models/aem-guides/content-approval-workflow/jcr:content/model"
       wizard="/apps/aem-guides/projects-tasks/projects/wizards/content-approval-start.html"
   />
   ```

   >[!NOTE]
   >
   >如果使用AEM 6.4，则工作流的位置已更改。 将`modelId`属性指向`/var/workflow/models/aem-guides/content-approval-workflow`下的运行时工作流模型的位置
   >
   >
   >有关工作流位置更改的更多详细信息，请参阅[此处。](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-65/content/implementing/developing/extending-aem/extending-workflows/workflows-best-practices)

   ```xml
   <contentapproval
       jcr:primaryType="nt:unstructured"
       modelId="/var/workflow/models/aem-guides/content-approval-workflow"
       wizard="/apps/aem-guides/projects-tasks/projects/wizards/content-approval-start.html"
   />
   ```

1. 将内容审批工作流添加到项目模板后，该工作流应该可以从项目的“工作流”拼贴启动。 开始并尝试使用我们创建的各种工艺路线。

## 支持材料

* [下载完成的教程包](./assets/develop-aem-projects/projects-tasks-guide.ui.apps-0.0.1-SNAPSHOT.zip)
* GitHub上的[完整代码存储库](https://github.com/Adobe-Marketing-Cloud/aem-guides/tree/feature/projects-tasks-guide)
* [AEM项目文档](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-65/content/sites/authoring/projects/projects)
