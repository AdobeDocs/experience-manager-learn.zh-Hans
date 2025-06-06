---
title: 使用Analysis Workspace分析数据
description: 了解如何将从Adobe Experience Manager网站捕获的数据映射到Adobe Analytics报表包中的量度和维度。 了解如何使用Adobe Analytics的Analysis Workspace功能构建详细的报表仪表板。
version: Experience Manager as a Cloud Service
topic: Integrations
feature: Adobe Client Data Layer
role: User
level: Intermediate
jira: KT-6409
thumbnail: KT-6296.jpg
doc-type: Tutorial
exl-id: b5722fe2-93bf-4b25-8e08-4cb8206771cb
badgeIntegration: label="集成" type="positive"
last-substantial-update: 2022-06-15T00:00:00Z
duration: 443
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '2072'
ht-degree: 0%

---

# 使用Analysis Workspace分析数据

了解如何将从Adobe Experience Manager网站捕获的数据映射到Adobe Analytics报表包中的量度和维度。 了解如何使用Adobe Analytics的Analysis Workspace功能构建详细的报表仪表板。

## 您即将构建的内容 {#what-build}

WKND营销团队有兴趣了解哪些`Call to Action (CTA)`按钮在主页上的表现最佳。 在本教程中，在&#x200B;**Analysis Workspace**&#x200B;中创建项目以可视化各种CTA按钮的性能并了解用户在网站上的行为。 当用户单击WKND主页上的行动号召(CTA)按钮时，使用Adobe Analytics捕获以下信息。

**Analytics变量**

以下是当前正在跟踪的Analytics变量：

* `eVar5` - `Page template`
* `eVar6` - `Page Id`
* `eVar7` - `Page last modified date`
* `eVar8` - `CTA Button Id`
* `eVar9` - `Page Name`
* `event8` - `CTA Button Click event`
* `prop8` - `CTA Button Id`

![CTA单击Adobe Analytics](assets/create-analytics-workspace/page-analytics.png)

### 目标 {#objective}

1. 创建报表包或使用现有报表包。
1. 在报表包中配置[转化变量(eVars)](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/manage-report-suites/edit-report-suite/conversion-variables/conversion-var-admin.html?lang=zh-Hans)和[成功事件（事件）](https://experienceleague.adobe.com/zh-hans/docs/analytics/admin/admin-tools/manage-report-suites/edit-report-suite/conversion-variables/success-event)。
1. 创建一个[Analysis Workspace项目](https://experienceleague.adobe.com/docs/analytics/analyze/analysis-workspace/home.html?lang=zh-Hans)以通过允许您快速构建、分析和共享见解的工具来分析数据。
1. 与其他团队成员共享Analysis Workspace项目。

## 先决条件

本教程是[在Adobe Analytics](./track-clicked-component.md)中跟踪已点击组件的延续，并假定您拥有：

* 启用了[Adobe Analytics扩展](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/client/analytics/overview.html?lang=zh-Hans)的&#x200B;**标记属性**
* **Adobe Analytics**&#x200B;测试/开发报表包ID和跟踪服务器。 请参阅以下有关[创建报表包](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/manage-report-suites/c-new-report-suite/new-report-suite.html?lang=zh-Hans)的文档。
* [Experience Platform Debugger](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html?lang=zh-Hans)浏览器扩展配置为在[WKND网站](https://wknd.site/us/en.html)或启用了Adobe数据层的AEM网站上加载标记属性。

## 转化变量(eVar)和成功事件（事件）

Custom Insight转化变量(或eVar)会放置在网站选定网页的Adobe代码中。 其主要目的是在自定义营销报表中细分转化成功量度。 eVar可以是基于访问的，其功能与Cookie类似。 传递到eVar变量的值会跟随用户预定的一段时间。

当eVar设置为访客的值时，Adobe会自动记住该值，直到它过期为止。 eVar值有效期间，访客遇到的任何成功事件将计入该eVar值。

eVar最适合用于衡量原因和结果，例如：

* 哪些内部活动影响了收入
* 最终导致注册的横幅广告
* 订单前使用内部搜索的次数

成功事件是可跟踪的操作。 成功事件由您决定。 例如，如果访客点击了CTA按钮，则该点击事件可被视为成功事件。

### 配置eVar

1. 从Adobe Experience Cloud主页中，选择您的组织，然后启动Adobe Analytics。

   ![Analytics AEP](assets/create-analytics-workspace/analytics-aep.png)

1. 在Analytics工具栏中，单击&#x200B;**管理员** > **报表包**，然后找到您的报表包。

   ![Analytics报表包](assets/create-analytics-workspace/select-report-suite.png)

1. 选择报表包> **编辑设置** > **转化** > **转化变量**

   ![Analytics转化变量](assets/create-analytics-workspace/conversion-variables.png)

1. 使用&#x200B;**添加新**&#x200B;选项，让我们创建转化变量来映射架构，如下所示：

   * `eVar5` - `Page Template`
   * `eVar6` - `Page ID`
   * `eVar7` - `Last Modified Date`
   * `eVar8` - `Button Id`
   * `eVar9` - `Page Name`

   ![添加新eVar](assets/create-analytics-workspace/add-new-evars.png)

1. 为每个eVar提供适当的名称和描述，并&#x200B;**保存**&#x200B;您的更改。 在Analysis Workspace项目中，使用具有适当名称的eVar，因此，用户友好型名称使变量易于发现。

   ![eVar](assets/create-analytics-workspace/evars.png)

### 配置成功事件

接下来，让我们创建一个事件来跟踪CTA按钮的点击情况。

1. 从&#x200B;**报表包管理器**&#x200B;窗口中，选择&#x200B;**报表包Id**&#x200B;并单击&#x200B;**编辑设置**。
1. 单击&#x200B;**转化** > **成功事件**
1. 使用&#x200B;**新增**&#x200B;选项，创建一个自定义成功事件以跟踪CTA按钮单击，然后&#x200B;**保存**&#x200B;您的更改。
   * `Event` ： `event8`
   * `Name`：`CTA Click`
   * `Type`：`Counter`

   ![eVar](assets/create-analytics-workspace/add-success-event.png)

## 在Analysis Workspace中创建项目 {#workspace-project}

Analysis Workspace是一款灵活的浏览器工具，可让您快速构建分析和共享见解。 您可以使用拖放界面进行分析、添加可视化图表以便直观地呈现数据、策划数据集、与组织中的任何人共享项目并设置共享计划。

接下来，创建一个[项目](https://experienceleague.adobe.com/docs/analytics/analyze/analysis-workspace/build-workspace-project/freeform-overview.html?lang=zh-Hans#analysis-workspace)以构建一个仪表板来分析整个网站中CTA按钮的表现。

1. 从Analytics工具栏中，选择&#x200B;**Workspace**，然后单击&#x200B;**创建新项目**。

   ![Workspace](assets/create-analytics-workspace/create-workspace.png)

1. 选择从&#x200B;**空白项目**&#x200B;开始，或者选择一个由Adobe提供的预建模板或您的组织创建的自定义模板。 根据您所考虑的分析或用例，可以使用多个模板。 [了解更多](https://experienceleague.adobe.com/docs/analytics/analyze/analysis-workspace/build-workspace-project/starter-projects.html?lang=zh-Hans)可用的不同模板选项。

   在Workspace项目中，从左边栏访问“面板、表格、可视化和组件” 。 它们构成了项目的构建块。

   * **[组件](https://experienceleague.adobe.com/docs/analytics/analyze/analysis-workspace/components/analysis-workspace-components.html?lang=zh-Hans)** — 组件包含维度、量度、区段或日期范围，所有这些组件都可以合并到一个自由格式表中，以便开始回答您的业务问题。 请务必先熟悉每个组件类型，然后再开始投入分析。 掌握组件术语后，即可开始拖放至自由格式表以构建分析。
   * **[可视化图表](https://experienceleague.adobe.com/docs/analytics/analyze/analysis-workspace/visualizations/freeform-analysis-visualizations.html?lang=zh-Hans)** — 接下来，将可视化图表（例如条形图或折线图）添加到数据的顶部，以便更加直观地将数据呈现出来。 在最左侧的边栏中，选择中间的可视化图表图标，以查看所有可用的可视化图表。
   * **[面板](https://experienceleague.adobe.com/docs/analytics/analyze/analysis-workspace/panels/panels.html?lang=zh-Hans)** — 面板是表格和可视化图表的集合。 您可以从Workspace左上角的图标访问面板。 当您要根据时段、报表包或分析用例组织您的项目时，面板非常有用。 Analysis Workspace中有以下面板类型：

   ![模板选择](assets/create-analytics-workspace/workspace-tools.png)

### 使用Analysis Workspace添加数据可视化

接下来，构建一个表以创建用户如何与WKND站点主页上的`Call to Action (CTA)`按钮交互的可视表示形式。 要生成此表示法，让我们将收集在[跟踪已点击组件中的数据与Adobe Analytics](./track-clicked-component.md)结合使用。 下面是用户与WKND网站的行动号召按钮进行交互时跟踪的数据快速摘要。

* `eVar5` - `Page template`
* `eVar6` - `Page Id`
* `eVar7` - `Page last modified date`
* `eVar8` - `CTA Button Id`
* `eVar9` - `Page Name`
* `event8` - `CTA Button Click event`
* `prop8` - `CTA Button Id`

1. 将&#x200B;**页面**&#x200B;维度组件拖放到自由格式表中。 现在，您应该能够查看一个可视化图表，该可视化图表显示表格中显示的页面名称(eVar9)和相应的页面查看次数（发生次数）。

   ![页Dimension](assets/create-analytics-workspace/evar9-dimension.png)

1. 将&#x200B;**CTA点击** (event8)指标拖放到发生次数量度上并将其替换。 您现在可以查看一个可视化图表，该可视化图表显示页面名称(eVar9)以及页面上CTA点击事件的相应计数。

   ![页面量度 — CTA点击](assets/create-analytics-workspace/evar8-cta-click.png)

1. 让我们按照页面的模板类型来划分页面。 从组件中选择页面模板度量，并将“页面模板”度量拖放到页面名称维上。 您现在可以查看按模板类型划分的页面名称。

   * **早于**

     ![eVar5](assets/create-analytics-workspace/evar5.png)

   * **After**

     ![eVar5指标](assets/create-analytics-workspace/evar5-metrics.png)

1. 要了解用户如何与WKND网站页面上的CTA按钮进行交互，需要通过添加按钮ID (eVar8)量度进一步细分。

   ![eVar8](assets/create-analytics-workspace/evar8.png)

1. 在下方，您可以看到WKND网站的可视化表示形式，它按页面模板划分，并进一步按用户与WKND网站点击操作(CTA)按钮的交互进行划分。

   ![eVar8](assets/create-analytics-workspace/evar8-metric.png)

1. 您可以使用“Adobe Analytics分类”将按钮ID值替换为更加用户友好的名称。 有关如何为特定量度[创建分类的详细信息，请参阅此处](https://experienceleague.adobe.com/docs/analytics/components/classifications/c-classifications.html?lang=zh-Hans)。 在本例中，我们为`eVar8`设置了一个分类量度`Button Section (Button ID)`，它将按钮ID映射到用户友好名称。

   ![按钮部分](assets/create-analytics-workspace/button-section.png)

## 向分析变量添加分类

### 转化分类

Analytics分类是在生成报表时对Analytics变量数据进行分类，然后以不同的方式显示数据的方法。 为了改进按钮ID在Analytics Workspace报表中的显示方式，让我们为按钮ID (eVar8)创建一个分类变量。 分类时，您会在变量和与该变量相关的元数据之间建立关系。

接下来，让我们为Analytics变量创建分类。

1. 从&#x200B;**管理员**&#x200B;工具栏菜单中，选择&#x200B;**报表包**
1. 从&#x200B;**报表包管理器**&#x200B;窗口中选择&#x200B;**报表包Id**，然后单击&#x200B;**编辑设置** > **转化** > **转化分类**

   ![转化分类](assets/create-analytics-workspace/conversion-classification.png)

1. 从&#x200B;**选择分类类型**&#x200B;下拉列表中，选择变量(eVar8-Button ID)以添加分类。
1. 单击“分类”部分下列出的分类变量旁边的右箭头可添加新分类。

   ![转化分类类型](assets/create-analytics-workspace/select-classification-variable.png)

1. 在&#x200B;**编辑分类**&#x200B;对话框中，为文本分类提供合适的名称。 将创建一个具有文本分类名称的维度组件。

   ![转化分类类型](assets/create-analytics-workspace/new-classification.png)

1. **保存**&#x200B;您的更改。

### 分类导入器

使用导入器将分类上传到Adobe Analytics。 您也可以在导入之前导出要更新的数据。 使用导入工具导入的数据必须使用特定格式。 Adobe为您提供了选项，用于下载数据模板，该数据模板采用制表符分隔的数据文件中所有正确的标题详细信息。 您可以将新数据添加到此模板，然后使用FTP在浏览器中导入数据文件。

#### 分类模板

在将分类导入营销报表之前，您可以下载模板来帮助您创建分类数据文件。 数据文件会将您所需的分类用作列标题，然后将报表数据集整理在相应的分类标题下。

接下来，让我们下载按钮ID (eVar8)变量的分类模板

1. 导航到&#x200B;**管理员** > **分类导入器**
1. 让我们从&#x200B;**下载模板**&#x200B;选项卡下载转化变量的分类模板。
   ![转化分类类型](assets/create-analytics-workspace/classification-importer.png)

1. 在下载模板选项卡上，指定数据模板配置。
   * **选择报表包** ：选择要在模板中使用的报表包。 报表包和数据集必须匹配。
   * **要分类的数据集** ：选择数据文件的数据类型。 该菜单包含报表包中针对分类配置的所有报表。
   * **编码** ：选择数据文件的字符编码。 默认编码格式为UTF-8。

1. 单击&#x200B;**下载**&#x200B;并将模板文件保存到您的本地系统。 模板文件是大多数电子表格应用程序支持的以制表符分隔的数据文件（文件扩展名为.tab）。
1. 使用您选择的编辑器打开以制表符分隔的数据文件。
1. 在部分中，将按钮ID (eVar9)和相应的按钮名称添加到以制表符分隔的文件中，以便用于执行步骤9中的每个eVar9值。

   ![键值](assets/create-analytics-workspace/key-value.png)

1. **保存**&#x200B;制表符分隔的文件。
1. 导航到&#x200B;**导入文件**&#x200B;选项卡。
1. 为文件导入配置目标。
   * **选择报表包** ：WKND站点AEM（报表包）
   * **要分类的数据集** ：按钮Id (转化变量eVar8)
1. 单击&#x200B;**选择文件**&#x200B;选项，从系统上传制表符分隔的文件，然后单击&#x200B;**导入文件**

   ![文件导入程序](assets/create-analytics-workspace/file-importer.png)

   >[!NOTE]
   >
   > 成功的导入会立即在导出中显示相应的更改。 但是，使用浏览器导入时，报表中的数据更改最多需要4小时，使用FTP导入时，的数据更改最多需要24小时。

#### 将转化变量替换为分类变量

1. 从Analytics工具栏中，选择&#x200B;**Workspace**，然后打开在本教程的[在Analysis Workspace中创建项目](#create-a-project-in-analysis-workspace)部分中创建的工作区。

   ![Workspace按钮ID](assets/create-analytics-workspace/workspace-report-button-id.png)

1. 接下来，将工作区中用于显示行动号召(CTA)按钮ID的&#x200B;**按钮ID**&#x200B;指标替换为上一步中创建的分类名称。

1. 在组件查找器中，搜索&#x200B;**WKND CTA按钮**，并将&#x200B;**WKND CTA按钮（按钮ID）**&#x200B;维度拖放到“按钮ID”量度上并将其替换。

   * **早于**

     ![Workspace按钮（早于](assets/create-analytics-workspace/wknd-button-before.png)）
   * **After**

     ![&#128279;](assets/create-analytics-workspace/wknd-button-after.png)之后的Workspace按钮

1. 您可以注意到，包含行动号召(CTA)按钮的按钮ID量度现已替换为分类模板中提供的相应名称。
1. 让我们将Analytics Workspace表与WKND主页进行比较，了解CTA按钮点击计数及其分析。 根据工作区自由格式表数据，用户已单击&#x200B;**SKI NOW**&#x200B;按钮的22次以及WKND Home Page Camping in Western Australia **Read More**&#x200B;按钮的4次很清楚。

   ![CTA报告](assets/create-analytics-workspace/workspace-report-buttons-wknd.png)

1. 确保保存您的Adobe Analytics Workspace项目并提供适当的名称和描述。 或者，您也可以将标记添加到Workspace项目。

   ![保存项目](assets/create-analytics-workspace/save-project.png)

1. 成功保存项目后，您可以使用“共享”选项与其他同事或队友共享您的工作区项目。

   ![共享项目](assets/create-analytics-workspace/share.png)

## 恭喜！

您刚刚了解如何将从Adobe Experience Manager网站捕获的数据映射到Adobe Analytics报表包中的量度和维度。 此外，还对量度执行了分类，并使用Adobe Analytics的Analysis Workspace功能构建了一个详细的报表仪表板。
