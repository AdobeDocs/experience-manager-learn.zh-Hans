---
title: 创作内容片段 — AEM Headless的高级概念 — GraphQL
description: 在Adobe Experience Manager (AEM) Headless的高级概念的这一章中，了解如何在内容片段中使用选项卡、日期和时间、JSON对象以及片段引用。 设置文件夹策略以限制可以包含的内容片段模型。
version: Experience Manager as a Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Intermediate
exl-id: 998d3678-7aef-4872-bd62-0e6ea3ff7999
duration: 609
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '2931'
ht-degree: 1%

---

# 创作内容片段

在[上一章](/help/headless-tutorial/graphql/advanced-graphql/create-content-fragment-models.md)中，您创建了五个内容片段模型：人员、团队、位置、地址和联系信息。 本章将指导您完成基于这些模型创建内容片段的步骤。 它还探讨了如何创建文件夹策略以限制可在文件夹中使用的内容片段模型。

## 先决条件 {#prerequisites}

本文档是多部分教程的一部分。 在继续本章之前，请确保已完成[上一章](create-content-fragment-models.md)。

## 目标 {#objectives}

在本章中，了解如何：

* 使用文件夹策略创建文件夹并设置限制
* 直接从内容片段编辑器创建片段引用
* 使用选项卡、日期和JSON对象数据类型
* 将内容和片段引用插入到多行文本编辑器中
* 添加多个片段引用
* 嵌套内容片段

## 安装示例内容 {#sample-content}

安装一个AEM包，其中包含多个用于加速教程的文件夹和示例图像。

1. 下载[Advanced-GraphQL-Tutorial-Starter-Package-1.1.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/Advanced-GraphQL-Tutorial-Starter-Package-1.1.zip)
1. 在AEM中，导航到&#x200B;**工具** > **部署** > **包**&#x200B;以访问&#x200B;**包管理器**。
1. 上载并安装在上一步中下载的软件包（zip文件）。

   ![包通过包管理器上传](assets/author-content-fragments/install-starter-package.png)

## 使用文件夹策略创建文件夹并设置限制

从AEM主页中，选择&#x200B;**Assets** > **文件** > **WKND共享** > **英语**。 在这里，您可以看到各种内容片段类别，包括冒险和参与者。

### 创建文件夹 {#create-folders}

导航到&#x200B;**冒险**&#x200B;文件夹。 您可以看到已创建团队和位置文件夹来存储团队和位置内容片段。

为基于人员内容片段模型的讲师内容片段创建文件夹。

1. 从“冒险”页面右上角选择&#x200B;**创建** > **文件夹**。

   ![创建文件夹](assets/author-content-fragments/create-folder.png)

1. 在出现的“创建文件夹”模式中，在&#x200B;**标题**&#x200B;字段中输入“讲师”。 记下结尾的“s”。 包含多个片段的文件夹的标题必须为复数。 选择&#x200B;**创建**。

   ![创建文件夹模式](assets/author-content-fragments/create-folder-modal.png)

   您现在已创建用于存储冒险讲师的文件夹。

### 使用文件夹策略设置限制

AEM允许您定义内容片段文件夹的权限和策略。 通过使用权限，您可以仅向特定用户（作者）或作者组授予对特定文件夹的访问权限。 通过使用文件夹策略，您可以限制作者在这些文件夹中可以使用的内容片段模型。 在此示例中，我们将文件夹限制为人员和联系信息模型。 配置文件夹策略：

1. 选择您已创建的&#x200B;**讲师**&#x200B;文件夹，然后从顶部导航栏中选择&#x200B;**属性**。

   ![属性](assets/author-content-fragments/properties.png)

1. 选择&#x200B;**策略**&#x200B;选项卡，然后取消选择&#x200B;**继承自/content/dam/wknd-shared**。 在&#x200B;**按路径允许的内容片段模型**&#x200B;字段中，选择文件夹图标。

   ![文件夹图标](assets/author-content-fragments/folder-icon.png)

1. 在打开的“选择路径”对话框中，遵循路径&#x200B;**conf** > **WKND已共享**。 在上一章中创建的人员内容片段模型包含对联系信息内容片段模型的引用。 必须在“讲师”文件夹中同时允许使用人员和联系信息模型，才能创建讲师内容片段。 选择&#x200B;**人员**&#x200B;和&#x200B;**联系人信息**，然后按&#x200B;**选择**&#x200B;关闭对话框。

   ![选择路径](assets/author-content-fragments/select-path.png)

1. 选择&#x200B;**保存并关闭**，然后在显示的成功对话框中选择&#x200B;**确定**。

1. 您现在已为“讲师”文件夹配置了文件夹策略。 导航到&#x200B;**讲师**&#x200B;文件夹并选择&#x200B;**创建** > **内容片段**。 您现在可以选择的模型只有&#x200B;**人员**&#x200B;和&#x200B;**联系信息**。

   ![文件夹策略](assets/author-content-fragments/folder-policies.png)

## 为讲师创作内容片段

导航到&#x200B;**讲师**&#x200B;文件夹。 从此处，我们创建一个嵌套文件夹来存储讲师的联系信息。

按照[创建文件夹](#create-folders)一节中介绍的步骤创建名为“联系信息”的文件夹。 嵌套文件夹继承父文件夹的文件夹策略。 请随意配置更具体的策略，以便新创建的文件夹仅允许使用联系信息模型。

### 创建讲师内容片段

让我们创建四个可以添加到冒险讲师团队中的人员。

1. 在“讲师”文件夹中，创建一个基于人员内容片段模型的内容片段，并将其标题命名为“Jacob Wester”。

   新创建的内容片段如下所示：

   ![人员内容片段](assets/author-content-fragments/person-content-fragment.png)

1. 在字段中输入以下内容：

   * **全名**： Jacob Wester
   * **个人简历**：雅各布·韦斯特十年以来一直是一名远足教练，喜欢这其中的每一分钟！ 雅各布是一名探险家，擅长攀岩和背包。 雅各布是攀岩比赛的赢家，包括海湾之战攀岩比赛。 雅各布现在住在加利福尼亚。
   * **讲师经验级别**：专家
   * **技能**：攀岩、冲浪、背包
   * **管理员详细信息**： Jacob Wester已协调背包冒险活动三年。

1. 在&#x200B;**个人资料图片**&#x200B;字段中，添加对图像的内容引用。 浏览到&#x200B;**共享的WKND** > **英语** > **参与者** > **jacob_wester.jpg**&#x200B;以创建该图像的路径。

### 从内容片段编辑器创建片段引用 {#fragment-reference-from-editor}

AEM允许您直接从内容片段编辑器创建片段引用。 让我们创建一个引用Jacob的联系信息。

1. 在&#x200B;**联系信息**&#x200B;字段下选择&#x200B;**新内容片段**。

   ![新内容片段](assets/author-content-fragments/new-content-fragment.png)

1. 此时将打开新内容片段模式窗口。 在“选择目标”选项卡下，按照路径&#x200B;**冒险** > **讲师**&#x200B;进行操作，然后选中&#x200B;**联系人信息**&#x200B;文件夹旁边的复选框。 选择&#x200B;**下一步**&#x200B;以继续进入“属性”选项卡。

   ![新内容片段模式](assets/author-content-fragments/new-content-fragment-modal.png)

1. 在“属性”选项卡下，在&#x200B;**标题**&#x200B;字段中输入“Jacob Wester联系信息”。 选择&#x200B;**创建**，然后在出现的成功对话框中按&#x200B;**打开**。

   ![“属性”选项卡](assets/author-content-fragments/properties-tab.png)

   显示的新字段允许您编辑联系人信息内容片段。

   ![联系人信息内容片段](assets/author-content-fragments/contact-info-content-fragment.png)

1. 在字段中输入以下内容：

   * **电话**： 209-888-0000
   * **电子邮件**： jwester@wknd.com

   完成后，选择&#x200B;**保存**。 您现在已创建了一个联系信息内容片段。

1. 要导航回讲师内容片段，请选择编辑器左上角的&#x200B;**Jacob Wester**。

   ![导航回原始内容片段](assets/author-content-fragments/back-to-jacob-wester.png)

   **联系人信息**&#x200B;字段现在包含引用联系人信息片段的路径。 这是一个嵌套片段引用。 完成的讲师内容片段如下所示：

   ![Jacob Wester内容片段](assets/author-content-fragments/jacob-wester-content-fragment.png)

1. 选择&#x200B;**保存并关闭**&#x200B;以保存内容片段。 您现在有了一个新的讲师内容片段。

### 创建其他片段

按照[上一节](#fragment-reference-from-editor)中所述的相同过程操作，为这些讲师再创建三个讲师内容片段和三个联系信息内容片段。 在讲师片段中添加以下内容：

**Stacey Roswells**

| 字段 | 值 |
| --- | --- |
| 内容片段标题 | 史黛西·罗斯韦尔斯 |
| 完整名称 | 史黛西·罗斯韦尔斯 |
| 联系信息 | /content/dam/wknd-shared/en/adventures/instructors/contact-info/stacey-roswells-contact-info |
| 个人资料图片 | /content/dam/wknd-shared/en/contributors/stacey-roswells.jpg |
| 传记 | 史黛西·罗斯威尔是一位高超的攀岩家和高山冒险家。 斯泰西出生于马里兰州巴尔的摩，是六个孩子中的老幺。 斯泰西的父亲是美国海军中校，母亲是一位现代舞蹈教练。 斯泰西的家人经常因父亲的职务任务而搬家，并在父亲驻泰国时拍摄了第一张照片。 这也是史黛西学会攀岩的地方。 |
| 讲师经验级别 | 高级 |
| 技能 | 攀岩 | 滑雪 | 背包 |

**Kumar Selvaraj**

| 字段 | 值 |
| --- | --- |
| 内容片段标题 | Kumar Selvaraj |
| 完整名称 | Kumar Selvaraj |
| 联系信息 | /content/dam/wknd-shared/en/adventures/instructors/contact-info/kumar-selvaraj-contact-info |
| 个人资料图片 | /content/dam/wknd-shared/en/contributors/kumar-selvaraj.jpg |
| 传记 | Kumar Selvaraj是一名经验丰富的AMGA认证专业讲师，其主要目标是帮助学生提高攀爬和徒步旅行技能。 |
| 讲师经验级别 | 高级 |
| 技能 | 攀岩 | 背包 |

**Ayo Ogunseinde**

| 字段 | 值 |
| --- | --- |
| 内容片段标题 | 阿依奥·奥贡塞因德 |
| 完整名称 | 阿依奥·奥贡塞因德 |
| 联系信息 | /content/dam/wknd-shared/en/adventures/instructors/contact-info/ayo-ogunseinde-contact-info |
| 个人资料图片 | /content/dam/wknd-shared/en/contributors/ayo-ogunseinde-237739.jpg |
| 传记 | Ayo Ogunseinde是一名职业登山师和背包教练，居住在中加利福利亚州弗雷斯诺市。 Ayo的目标是引导徒步旅行者进行他们最壮观的国家公园冒险。 |
| 讲师经验级别 | 高级 |
| 技能 | 攀岩 | 循环 | 背包 |

将&#x200B;**附加信息**&#x200B;字段留空。

在联系信息片段中添加以下信息：

| 内容片段标题 | 手机 | 电子邮件 |
| ------- | -------- | -------- |
| Stacey Roswells联系信息 | 209-888-0011 | sroswells@wknd.com |
| Kumar Selvaraj联系信息 | 209-888-0002 | kselvaraj@wknd.com |
| Ayo Ogunseinde联系信息 | 209-888-0304 | aogunseinde@wknd.com |

您现在可以创建团队！

## 创作位置的内容片段

导航到&#x200B;**位置**&#x200B;文件夹。 在这里，您会看到两个已创建的嵌套文件夹：约塞米蒂国家公园和约塞米蒂山谷旅馆。

![位置文件夹](assets/author-content-fragments/locations-folder.png)

暂时忽略Yosemite Valley Lodge文件夹。 在本节的后面部分，当我们创建一个位置作为讲师团队的Home Base时，我们又会返回到此位置。

导航到&#x200B;**约塞米蒂国家公园**&#x200B;文件夹。 目前，其中只包含约塞米蒂国家公园的图片。 让我们使用位置内容片段模型创建一个内容片段，并将其命名为“Yosemite National Park”。

### 制表符占位符

AEM允许您使用选项卡占位符对不同类型的内容进行分组，并使内容片段更易于阅读和管理。 在上一章中，您向“位置”模型添加了制表符占位符。 因此，位置内容片段现在具有两个选项卡部分：**位置详细信息**&#x200B;和&#x200B;**位置地址**。

![制表符占位符](assets/author-content-fragments/tabs.png)

**位置详细信息**&#x200B;选项卡包含&#x200B;**名称**、**描述**、**联系信息**、**位置图像**&#x200B;和&#x200B;**按季节划分的天气**&#x200B;字段，而&#x200B;**位置地址**&#x200B;选项卡包含对地址内容片段的引用。 通过选项卡，可以清楚了解必须填充的内容类型，因此创作内容更易于管理。

### JSON对象数据类型

**按季节划分的天气**&#x200B;字段是一个JSON对象数据类型，这意味着它接受JSON格式的数据。 此数据类型非常灵活，可用于要包含在内容中的任何数据。

您可以通过将鼠标悬停在字段右侧的信息图标上，查看在上一章中创建的字段说明。

![JSON对象信息图标](assets/author-content-fragments/json-object-info.png)

在这种情况下，我们需要提供位置的平均天气。 输入以下数据：

```json
{
    "summer": "81 / 89°F",
    "fall": "56 / 83°F",
    "winter": "46 / 51°F",
    "spring": "57 / 71°F"
}
```

**按季节列出的天气**&#x200B;字段现在应如下所示：

![JSON 对象](assets/author-content-fragments/json-object.png)

### 添加内容

让我们将其余内容添加到位置内容片段，以便在下一章中使用GraphQL查询信息。

1. 在&#x200B;**位置详细信息**&#x200B;选项卡中，在字段中输入以下信息：

   * **名称**：约塞米蒂国家公园
   * **描述**：约塞米蒂国家公园位于加利福尼亚的内华达山脉。 它以华丽的瀑布、巨大的红杉树以及埃尔卡皮坦和半圆顶悬崖的标志性景色而闻名。 徒步旅行和露营是体验约塞米蒂的最佳方式。 许多小径为探险和探索提供了无穷无尽的机会。

1. 从&#x200B;**联系信息**&#x200B;字段中，根据联系信息模型创建内容片段，并将其标题命名为“Yosemite National Park Contact Info”。 按照上一节中有关[从编辑器](#fragment-reference-from-editor)创建片段引用的相同过程操作，并在字段中输入以下数据：

   * **电话**： 209-999-0000
   * **电子邮件**： yosemite@wknd.com

1. 从&#x200B;**位置图像**&#x200B;字段中，浏览到&#x200B;**冒险** > **位置** > **约塞米蒂国家公园** > **约塞米蒂国家公园.jpeg**&#x200B;以创建该图像的路径。

   请记住，在上一章中，您配置了图像验证，因此“位置”图像的尺寸必须小于2560 x 1800，并且其文件大小必须小于3 MB。

1. 添加完所有信息后，**位置详细信息**&#x200B;选项卡现在看起来如下所示：

   ![位置详细信息选项卡已完成](assets/author-content-fragments/location-details-tab-completed.png)

1. 导航到&#x200B;**位置地址**&#x200B;选项卡。 从&#x200B;**Address**&#x200B;字段中，使用您在上一章中创建的地址内容片段模型创建标题为“Yosemite National Park Address”的内容片段。 按照有关[从编辑器创建片段引用](#fragment-reference-from-editor)的部分中所述的相同过程操作，并在字段中输入以下数据：

   * **街道地址**： 9010 Curry Village Drive
   * **城市**：约塞米蒂谷
   * **状态**： CA
   * **邮政编码**： 95389
   * **国家/地区**：美国

1. 约塞米蒂国家公园片段的已完成&#x200B;**位置地址**&#x200B;选项卡如下所示：

   ![位置地址选项卡已完成](assets/author-content-fragments/location-address-tab-completed.png)

1. 选择&#x200B;**保存并关闭**。

### 再创建一个片段

1. 导航到&#x200B;**Yosemite Valley Lodge**&#x200B;文件夹。 使用位置内容片段模型创建内容片段，并将其命名为“Yosemite Valley Lodge”。

1. 在&#x200B;**位置详细信息**&#x200B;选项卡中，在字段中输入以下信息：

   * **名称**： Yosemite Valley Lodge
   * **描述**： Yosemite Valley Lodge是一个小组会议和各种活动（如购物、用餐、钓鱼、远足等）的中心。

1. 从&#x200B;**联系信息**&#x200B;字段中，根据联系信息模型创建内容片段，并将其标题命名为“Yosemite Valley Lodge联系信息”。 按照有关[从编辑器创建片段引用](#fragment-reference-from-editor)的部分中所述的相同过程操作，并在新内容片段的字段中输入以下数据：

   * **电话**： 209-992-0000
   * **电子邮件**： yosemitelodge@wknd.com

   保存新创建的内容片段。

1. 导航回&#x200B;**Yosemite Valley Lodge**，然后转到&#x200B;**位置地址**&#x200B;选项卡。 从&#x200B;**地址**&#x200B;字段中，使用您在上一章中创建的地址内容片段模型创建标题为“Yosemite Valley Lodge Address”的内容片段。 按照有关[从编辑器创建片段引用](#fragment-reference-from-editor)的部分中所述的相同过程操作，并在字段中输入以下数据：

   * **街道地址**： 9006 Yosemite Lodge Drive
   * **城市**：约塞米蒂国家公园
   * **状态**： CA
   * **邮政编码**： 95389
   * **国家/地区**：美国

   保存新创建的内容片段。

1. 导航回&#x200B;**Yosemite Valley Lodge**，然后选择&#x200B;**保存并关闭**。 **Yosemite Valley Lodge**&#x200B;文件夹现在包含三个内容片段：Yosemite Valley Lodge、Yosemite Valley Lodge联系信息和Yosemite Valley Lodge地址。

   ![Yosemite Valley Lodge文件夹](assets/author-content-fragments/yosemite-valley-lodge-folder.png)

## 创作团队内容片段

浏览文件夹到&#x200B;**团队** > **Yosemite团队**。 您可以看到Yosemite Team文件夹当前仅包含团队徽标。

![Yosemite团队文件夹](assets/author-content-fragments/yosemite-team-folder.png)

让我们使用团队内容片段模型创建一个内容片段，并将其命名为“Yosemite团队”。

### 多行文本编辑器中的内容和片段引用

AEM允许您直接将内容和片段引用添加到多行文本编辑器中，并在以后使用GraphQL查询检索它们。 让我们将内容和片段引用添加到&#x200B;**描述**&#x200B;字段中。

1. 首先，在&#x200B;**描述**&#x200B;字段中添加以下文本：“在约塞米蒂国家公园工作的专业冒险家和登山指导员团队。”

1. 要添加内容引用，请在多行文本编辑器的工具栏中选择&#x200B;**插入资产**&#x200B;图标。

   ![插入资源图标](assets/author-content-fragments/insert-asset-icon.png)

1. 在出现的模式窗口中，选择&#x200B;**team-yosemite-logo.png**，然后按&#x200B;**选择**。

   ![选择图像](assets/author-content-fragments/select-image.png)

   内容引用现已添加到&#x200B;**描述**&#x200B;字段中。

请记住，在上一章中，您允许将片段引用添加到&#x200B;**描述**&#x200B;字段。 让我们在此处添加一个。

1. 在多行文本编辑器的工具栏中选择&#x200B;**插入内容片段**&#x200B;图标。

   ![插入内容片段图标](assets/author-content-fragments/insert-content-fragment-icon.png)

1. 浏览到&#x200B;**共享的WKND** > **英语** > **冒险** > **位置** > **约塞米蒂谷旅馆** > **约塞米蒂谷旅馆**。 按&#x200B;**选择**&#x200B;插入内容片段。

   ![插入内容片段模式](assets/author-content-fragments/insert-content-fragment-modal.png)

   **Description**&#x200B;字段现在如下所示：

   ![描述字段](assets/author-content-fragments/description-field.png)

现在，您已将内容和片段引用直接添加到多行文本编辑器中。

### 日期和时间数据类型

让我们看一下“日期和时间”数据类型。 选择&#x200B;**团队创建日期**&#x200B;字段右侧的&#x200B;**日历**&#x200B;图标以打开日历视图。

![日期日历视图](assets/author-content-fragments/date-calendar-view.png)

可以使用月份两侧的向前和向后箭头设置过去日期或未来日期。 比方说，约塞米蒂团队于2016年5月24日成立，因此我们将确定成立日期。

### 添加多个片段引用

让我们向团队成员片段引用添加讲师。

1. 在&#x200B;**团队成员**&#x200B;字段中选择&#x200B;**添加**。

   ![“添加”按钮](assets/author-content-fragments/add-button.png)

1. 在显示的新字段中，选择文件夹图标以打开“选择路径”模式窗口。 浏览文件夹至&#x200B;**WKND共享** > **英语** > **冒险** > **讲师**，然后选中&#x200B;**jacob-wester**&#x200B;旁边的复选框。 按&#x200B;**选择**&#x200B;保存路径。

   ![片段引用路径](assets/author-content-fragments/fragment-reference-path.png)

1. 再次选择&#x200B;**添加**&#x200B;按钮三次。 使用新字段将剩余的三个讲师添加到团队。 **团队成员**&#x200B;字段现在看起来像这样：

   ![团队成员字段](assets/author-content-fragments/team-members-field.png)

1. 选择&#x200B;**保存并关闭**&#x200B;以保存团队内容片段。

### 向冒险内容片段添加片段引用

最后，让我们将新创建的内容片段添加到冒险。

1. 导航到&#x200B;**Adventures** > **Yosemite Backpacking**，然后打开Yosemite Backpacking内容片段。 在表单底部，您可以看到在上一章中创建的三个字段：**位置**、**讲师团队**&#x200B;和&#x200B;**管理员**。

1. 在&#x200B;**位置**&#x200B;字段中添加片段引用。 位置路径应引用您创建的Yosemite国家公园内容片段： `/content/dam/wknd-shared/en/adventures/locations/yosemite-national-park/yosemite-national-park`。

1. 在&#x200B;**讲师团队**&#x200B;字段中添加片段引用。 团队路径应引用您创建的Yosemite团队内容片段： `/content/dam/wknd-shared/en/adventures/teams/yosemite-team/yosemite-team`。 这是一个嵌套片段引用。 团队内容片段包含对人员模型的引用，其中引用了联系信息和地址模型。 因此，您的嵌套内容片段向下三层显示。

1. 在&#x200B;**管理员**&#x200B;字段中添加片段引用。 假设雅各布·韦斯特是Yosemite背包冒险活动的管理员。 路径应该指向Jacob Wester内容片段，并如下所示： `/content/dam/wknd-shared/en/adventures/instructors/jacob-wester`。

1. 您现在为冒险内容片段添加了三个片段引用。 这些字段如下所示：

   ![冒险片段引用](assets/author-content-fragments/adventure-fragment-references.png)

1. 选择&#x200B;**保存并关闭**&#x200B;以保存您的内容。

## 恭喜！

恭喜！您现在已根据上一章中创建的高级内容片段模型创建了内容片段。 您还创建了一个文件夹策略以限制可以在文件夹中选择哪些内容片段模型。

## 后续步骤

在[下一章](/help/headless-tutorial/graphql/advanced-graphql/explore-graphql-api.md)中，您将了解有关使用GraphiQL Integrated Development Environment (IDE)发送高级GraphQL查询的信息。 这些查询允许我们查看在本章中创建的数据，并最终将这些查询添加到WKND应用程序。
