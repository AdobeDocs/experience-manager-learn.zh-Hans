---
title: 利用Analysis Workspace分析数据
description: 了解如何将从Adobe Experience Manager站点捕获的数据映射到Adobe Analytics报告套件中的指标和维度。 了解如何使用Adobe Analytics的Analysis Workspace功能构建详细的报告仪表板。
feature: analytics
topics: integrations
audience: administrator
doc-type: tutorial
activity: setup
version: cloud-service
kt: 6409
thumbnail: KT-6296.jpg
translation-type: tm+mt
source-git-commit: 55beee99b91c44f96cd37d161bb3b4ffe38d2687
workflow-type: tm+mt
source-wordcount: '2202'
ht-degree: 0%

---


# 利用Analysis Workspace分析数据

了解如何将从Adobe Experience Manager站点捕获的数据映射到Adobe Analytics报告套件中的指标和维度。 了解如何使用Adobe Analytics的Analysis Workspace功能构建详细的报告仪表板。

## 您将构建的内容

WKND营销团队希望了解哪个行动动员(CTA)按钮在主页上表现最佳。 在本教程中，我们将在Analysis Workspace创建一个新项目，以可视化不同CTA按钮的性能并了解站点上的用户行为。 当用户单击WKND主页上的“行动动员”(CTA)按钮时，以下信息将使用Adobe Analytics捕获。

**分析变量**

以下是当前跟踪的Analytics变量：

* `eVar5` -  `Page template`
* `eVar6` - `Page Id`
* `eVar7` -  `Page last modified date`
* `eVar8` -  `CTA Button Id`
* `eVar9` -  `Page Name`
* `event8` -  `CTA Button Click event`
* `prop8` -  `CTA Button Id`

![CTA单击Adobe Analytics](assets/create-analytics-workspace/page-analytics.png)

### 目标{#objective}

1. 创建新的报表包或使用现有报表包。
1. 在报告包中配置[转换变量(eVar)](https://docs.adobe.com/content/help/en/analytics/admin/admin-tools/conversion-variables/conversion-var-admin.html)和[成功事件(事件)](https://docs.adobe.com/help/en/analytics/admin/admin-tools/success-events/success-event.html)。
1. 创建[Analysis Workspace项目](https://docs.adobe.com/content/help/en/analytics/analyze/analysis-workspace/home.html)，借助可让您快速构建、分析和共享洞察的工具分析数据。
1. 与其他团队成员共享Analysis Workspace项目。

## 前提条件

本教程是[跟踪单击的组件与Adobe Analytics](./track-clicked-component.md)的延续，并假定您已：

* 启用了[Adobe Analytics扩展](https://docs.adobe.com/content/help/en/launch/using/extensions-ref/adobe-extension/analytics-extension/overview.html)的&#x200B;**启动属性**
* **Adobe** Analyticst/dev报告套件ID和跟踪服务器。有关[创建新报表包](https://docs.adobe.com/content/help/en/analytics/admin/manage-report-suites/new-report-suite/new-report-suite.html)，请参阅以下文档。
* [Experience Platform](https://docs.adobe.com/content/help/en/platform-learn/tutorials/data-ingestion/web-sdk/introduction-to-the-experience-platform-debugger.html) 调试器浏览器扩展配置为在启用了Adobe数 [据层的](https://wknd.site/us/en.html) https://wknd.site/us/en.html或AEM站点上加载启动属性。

## 转换变量(eVar)和成功事件(事件)

Custom Insight转换变量(或eVar)位于您网站所选网页的Adobe代码中。 其主要目的是在自定义营销报告中细分转化成功指标。 eVar可以基于访问，功能与cookies类似。 传入eVar变量的值在预定时间段内跟随用户。

当eVar设置为访客的值时，Adobe会自动记住该值，直到该值过期。 访客在eVar值处于活动状态时遇到的任何成功事件都计入eVar值。

eVar最适合用于衡量原因和效果，例如：

* 哪些内部活动影响了
* 最终，哪些横幅广告会导致注册
* 进行订购前使用内部搜索的次数

成功事件是可跟踪的操作。 您决定成功事件。 例如，如果访客单击CTA按钮，则单击事件可被视为成功事件。

### 配置eVar

1. 在Adobe Experience Cloud主页，选择您的组织，并启动Adobe Analytics。

   ![Analytics AEP](assets/create-analytics-workspace/analytics-aep.png)

1. 在“Analytics”工具栏中，单击&#x200B;**Admin** > **报表包**&#x200B;并找到您的报表包。

   ![分析报告套件](assets/create-analytics-workspace/select-report-suite.png)

1. 选择“报告包”>“**编辑设置”**>“**转换**”>“转换变量”****

   ![分析转化变量](assets/create-analytics-workspace/conversion-variables.png)

1. 使用&#x200B;**添加新**&#x200B;选项，我们创建转换变量以映射模式，如下所示：

   * `eVar5` -   `Page Template`
   * `eVar6` -  `Page ID`
   * `eVar7` -  `Last Modified Date`
   * `eVar8` -  `Button Id`
   * `eVar9` -  `Page Name`

   ![添加新eVar](assets/create-analytics-workspace/add-new-evars.png)

1. 为每个eVar和&#x200B;**保存**&#x200B;更改提供适当的名称和说明。 我们将在下一节中使用这些eVar创建一个Analysis Workspace项目。 因此，用户友好的名称使变量易于发现。

   ![eVar](assets/create-analytics-workspace/evars.png)

### 配置成功事件

接下来，我们创建一个用于跟踪“CTA按钮”单击的EV。

1. 从&#x200B;**报表包管理器**&#x200B;窗口中，选择&#x200B;**报表包Id**&#x200B;并单击&#x200B;**编辑设置**。
1. 单击&#x200B;**转换** > **成功事件**
1. 使用&#x200B;**添加新**&#x200B;选项，创建新的自定义成功事件以跟踪CTA按钮，单击，然后&#x200B;**保存**&#x200B;更改。
   * `Event` : `event8`
   * `Name`:`CTA Click`
   * `Type`:`Counter`

   ![eVar](assets/create-analytics-workspace/add-success-event.png)

## 在Analysis Workspace创建新项目{#workspace-project}

Analysis Workspace是一款灵活的浏览器工具，可让您快速构建分析并分享见解。 使用拖放界面，您可以制作分析、添加可视化功能以栩栩如生地呈现数据、创建数据集、与组织中的任何人共享和计划项目。

然后，新建一个[项目](https://docs.adobe.com/content/help/en/analytics/analyze/analysis-workspace/build-workspace-project/t-freeform-project.html)以构建仪表板来分析整个站点中CTA按钮的性能。

1. 从“Analytics”工具栏中，选择&#x200B;**Workspace**&#x200B;并单击&#x200B;**创建新项目**。

   ![工作区](assets/create-analytics-workspace/create-workspace.png)

1. 选择从&#x200B;**空项目**&#x200B;中开始，或选择预建模板之一，该模板由Adobe提供或由您的组织创建的自定义模板。 根据您的分析或用例，可以使用多个模板。 [进一](https://docs.adobe.com/content/help/en/analytics/analyze/analysis-workspace/build-workspace-project/starter-projects.html) 步了解可用的不同模板选项。

   在您的Workspace项目中，面板、表、可视化和组件可从左边栏中访问。 这些是您的项目构件。

   * **[组件](https://docs.adobe.com/content/help/en/analytics/analyze/analysis-workspace/components/analysis-workspace-components.html)** -组件是维度、量度、细分或日期范围，所有这些组件都可以合并到自由形式表中，以开始回答您的业务问题。进入分析前，请务必熟悉每个组件类型。 掌握组件术语后，您可以开始拖放以在自由形式表中构建分析。
   * **[可视化](https://docs.adobe.com/content/help/en/analytics/analyze/analysis-workspace/visualizations/freeform-analysis-visualizations.html)** -可视化（如条形图或折线图）随后添加在数据上，以可视方式使其栩栩如生。在最左边的边栏上，选择中间的可视化图标，以查看可用可视化的完整列表。
   * **[面板](https://docs.adobe.com/content/help/en/analytics/analyze/analysis-workspace/panels/panels.html)** -面板是表和可视化的集合。您可以从工作区的左上角图标访问面板。 当您希望根据时段、报表包或分析使用案例组织项目时，面板会很有帮助。 Analysis Workspace提供以下面板类型：

   ![模板选择](assets/create-analytics-workspace/workspace-tools.png)

### 使用Analysis Workspace添加数据可视化

接下来，构建一个表，以直观呈现用户如何与WKND站点主页上的行动动员(CTA)按钮交互。 要构建这样的表示形式，我们使用在[跟踪单击的组件中收集的数据和Adobe Analytics](./track-clicked-component.md)。 以下是用户与WKND站点的“行动动员”按钮进行交互时跟踪的数据的快速摘要。

* `eVar5` -   `Page template`
* `eVar6` -  `Page Id`
* `eVar7` -  `Page last modified date`
* `eVar8` -  `CTA Button Id`
* `eVar9` -  `Page Name`
* `event8` -  `CTA Button Click event`
* `prop8` -  `CTA Button Id`

1. 将&#x200B;**Page**&#x200B;维组件拖放到自由形式表上。 您现在应能够视图显示页面名称(eVar9)的可视化，以及表中显示的相应页面视图（发生次数）。

   ![页面Dimension](assets/create-analytics-workspace/evar9-dimension.png)

1. 将&#x200B;**CTA单击**(事件8)度量拖放到发生量度上并替换它。 您现在可以视图显示页面名称(eVar9)的可视化，以及页面上相应的CTA单击事件计数。

   ![页面度量- CTA单击](assets/create-analytics-workspace/evar8-cta-click.png)

1. 让我们按页面的模板类型划分。 从组件中选择页面模板度量，然后将页面模板度量拖放到页面名称维。 您现在可以视图按页面名称模板类型划分的页面名称。

   * **之前**

      ![eVar5](assets/create-analytics-workspace/evar5.png)

   * **之后**

      ![eVar5指标](assets/create-analytics-workspace/evar5-metrics.png)

1. 要了解用户在WKND站点页面上与CTA按钮进行交互时，我们需要通过添加按钮ID(eVar8)度量进一步细分页面模板度量。

   ![eVar8](assets/create-analytics-workspace/evar8.png)

1. 在下面，您可以看到WKND站点的可视表示，它按其页面模板进行划分，并进一步按用户与WKND站点单击以操作(CTA)按钮的交互进行划分。

   ![eVar8](assets/create-analytics-workspace/evar8-metric.png)

1. 您可以使用“Adobe Analytics分类”将“按钮ID”值替换为更易用的名称。 您可以阅读有关如何为特定度量[创建分类的更多信息，请参阅此处](https://docs.adobe.com/content/help/en/analytics/components/classifications/c-classifications.html)。 在这种情况下，我们为`eVar8`设置了分类度量`Button Section (Button ID)`，该分类度量将按钮id映射到用户友好名称。

   ![按钮部分](assets/create-analytics-workspace/button-section.png)

## 向分析变量添加分类

### 转换分类

分析分类是一种将Analytics变量数据分类，然后在生成报告时以不同方式显示数据的方法。 为了改进按钮ID在Analytics Workspace报告中的显示方式，我们为按钮ID创建一个分类变量(eVar8)。 分类时，您将在变量和与该变量相关的元数据之间建立关系。

接下来，让我们为Analytics变量创建一个分类。

1. 从&#x200B;**Admin**&#x200B;工具栏菜单，选择&#x200B;**报表包**
1. 从&#x200B;**报表包管理器**&#x200B;窗口中选择&#x200B;**报表包Id**，然后单击&#x200B;**编辑设置** > **转换** > **转换分类**

   ![转换分类](assets/create-analytics-workspace/conversion-classification.png)

1. 从&#x200B;**选择分类类型**&#x200B;下拉列表中，选择变量(eVar8-按钮ID)以添加分类。
1. 单击“分类”部分下列出的“分类”变量旁边的箭头可添加新的“分类”。

   ![转换分类类型](assets/create-analytics-workspace/select-classification-variable.png)

1. 在&#x200B;**编辑分类**&#x200B;对话框中，为文本分类提供合适的名称。 将创建具有文本分类名称的维组件。

   ![转换分类类型](assets/create-analytics-workspace/new-classification.png)

1. **保** 存更改。

### 分类导入程序

使用导入程序将分类上传到Adobe Analytics。 您还可以导出数据以在导入之前进行更新。 使用导入工具导入的数据必须采用特定格式。 Adobe为您提供以下选项：下载一个数据模板，其中包含制表符分隔的数据文件中所有正确的标题详细信息。 您可以将新数据添加到此模板，然后使用FTP在浏览器中导入数据文件。

#### 分类模板

在将分类导入市场营销报告之前，您可以下载一个模板来帮助您创建分类数据文件。 数据文件将您所需的分类用作列标题，然后在相应的分类标题下组织报告数据集。

接下来，让我们下载按钮ID(eVar8)变量的分类模板

1. 导航到&#x200B;**Admin** > **分类导入程序**
1. 让我们从&#x200B;**下载模板**选项卡中下载转换变量的分类模板。
   ![转换分类类型](assets/create-analytics-workspace/classification-importer.png)

1. 在“下载模板”选项卡上，指定数据模板配置。
   * **选择报表包** :选择要在模板中使用的报表包。报表包和数据集必须匹配。
   * **要分类的数据集** :为数据文件选择数据类型。该菜单包含为分类配置的报表包中的所有报表。
   * **编码** :为数据文件选择字符编码。默认编码格式为UTF-8。

1. 单击&#x200B;**下载**&#x200B;并将模板文件保存到本地系统。 模板文件是大多数电子表格应用程序支持的制表符分隔数据文件（.tab文件扩展名）。
1. 使用您选择的编辑器打开制表符分隔的数据文件。
1. 为该部分中步骤9中的每个eVar9值添加按钮ID(eVar9)和相应的按钮名称。

   ![键值](assets/create-analytics-workspace/key-value.png)

1. **保** 存制表符分隔的文件。
1. 导航到&#x200B;**导入文件**&#x200B;选项卡。
1. 配置文件导入的目标。
   * **选择报表包** :WKND Site AEM(Report Suite)
   * **要分类的数据集** :按钮Id(转换变量eVar8)
1. 单击&#x200B;**选择文件**&#x200B;选项，从系统上传制表符分隔的文件，然后单击&#x200B;**导入文件**

   ![文件导入程序](assets/create-analytics-workspace/file-importer.png)

   >[!NOTE]
   >
   > 成功导入会立即在导出中显示相应的更改。 但是，使用浏览器导入时，报告中的数据更改最长需要4小时，使用FTP导入时最长需要24小时。

#### 将转换变量替换为分类变量

1. 从“Analytics”工具栏中，选择&#x200B;**Workspace**&#x200B;并打开我们在本教程的“在Analysis Workspace创建新项目”部分[中创建的工作区。](#workspace-project)

   ![工作区按钮ID](assets/create-analytics-workspace/workspace-report-button-id.png)

1. 接下来，将工作区中显示行动动员(CTA)按钮的ID的&#x200B;**按钮Id**&#x200B;量度替换为上一步中创建的分类名称。

1. 在组件查找器中，搜索&#x200B;**WKND CTA按钮**&#x200B;并将&#x200B;**WKND CTA按钮（按钮Id）**&#x200B;维拖放到按钮Id度量上并替换它。

   * **之前**

      ![之前的工作区按钮](assets/create-analytics-workspace/wknd-button-before.png)
   * **之后**

      ![之后的工作区按钮](assets/create-analytics-workspace/wknd-button-after.png)

1. 您可以注意到包含行动动员(CTA)按钮的按钮id的按钮ID度量现在替换为分类模板中提供的相应名称。
1. 让我们将Analytics Workspace表与WKND主页进行比较，了解CTA按钮点击计数及其分析。 根据工作区自由表格数据，用户单击&#x200B;**SKI NOW**&#x200B;按钮22次，在西澳大利亚州WKND主页露营按钮&#x200B;**阅读更多**&#x200B;四次。

   ![CTA报告](assets/create-analytics-workspace/workspace-report-buttons-wknd.png)

1. 确保保存您的Adobe Analytics工作区项目并提供正确的名称和说明。 或者，您也可以向工作区项目添加标记。

   ![保存项目](assets/create-analytics-workspace/save-project.png)

1. 成功保存项目后，您可以使用共享选项与其他同事或队友共享您的工作区项目。

   ![共享项目](assets/create-analytics-workspace/share.png)

## 恭喜！

您刚刚学习了如何将从Adobe Experience Manager站点捕获的数据映射到Adobe Analytics报表包中的度量和维度，对度量执行分类，以及使用Adobe Analytics的Analysis Workspace功能构建详细的报告仪表板。

