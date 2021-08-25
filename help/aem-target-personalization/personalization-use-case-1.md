---
title: 使用AEM体验片段和Adobe Target进行个性化
seo-title: Personalization using Adobe Experience Manager (AEM) Experience Fragments and Adobe Target
description: 一个端到端教程，其中演示了如何使用Adobe Experience Manager体验片段和Adobe Target创建和提供个性化体验。
seo-description: An end-to-end tutorial showing how to create and deliver personalized experience using Adobe Experience Manager Experience Fragments and Adobe Target.
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
source-git-commit: ea7d49985e69ecf9713e17e51587125b3fb400ee
workflow-type: tm+mt
source-wordcount: '1692'
ht-degree: 1%

---


# 使用AEM体验片段和Adobe Target进行个性化

借助将AEM体验片段作为HTML选件导出到Adobe Target的功能，您可以将AEM的易用性和强大功能与Target中强大的自动智能(AI)和机器学习(ML)功能结合使用，以测试和个性化大量体验。

AEM将您的所有内容和资产整合到一个中心位置，以助您实现个性化策略。 通过AEM，您可以在一个位置轻松地为台式机、平板电脑和移动设备创建内容，而无需编写代码。 无需为每个设备创建页面，因为AEM会使用您的内容自动调整每个体验。

Target让您能够根据基于规则和AI驱动的机器学习方法的组合大规模提供个性化体验，这些方法包含行为、情境和离线变量。  借助Target，您可以轻松设置并运行A/B和多变量(MVT)活动，以确定最佳的选件、内容和体验。

在将内容创建者与使用Target推动业务成果的营销人员关联方面，体验片段代表了向前迈出了一大步。

## 方案概述

WKND网站计划通过其网站在美国各地宣布一个&#x200B;**SkateFest Challenge**，并希望其网站用户注册参加在每个州进行的试用。 作为营销人员，您已经被分配了在WKND网站主页上运行营销活动的任务，该任务中包含与用户位置相关的横幅消息以及指向事件详细信息页面的链接。 让我们浏览WKND网站主页，并了解如何根据用户的当前位置为其创建和提供个性化体验。

### 涉及的用户

在本练习中，需要涉及以下用户，并且要执行一些可能需要管理访问权限的任务。

* **内容制作者/内容编辑器** (Adobe Experience Manager)
* **营销人员** (Adobe Target/优化团队)

### 前提条件

* **AEM**
   * [AEM创作和](./implementation.md#getting-aem) 发布实例在localhost 4502和4503上。
* **Experience Cloud**
   * 访问您的组织Adobe Experience Cloud - `https://<yourcompany>.experiencecloud.adobe.com`
   * Experience Cloud配置了以下解决方案
      * [Adobe Target](https://experiencecloud.adobe.com)

### WKND网站主页

![AEM Target方案1](assets/personalization-use-case-1/aem-target-use-case-1-4.png)

1. 营销人员与AEM内容编辑器一起发起WKND SkateFest营销活动讨论，并详细介绍相关要求。
   * ***要求***:在WKND网站主页上为来自美国每个州的访客提供个性化内容，以推广WKND SkateFest活动。在主页轮播下方添加一个新的内容块，其中包含背景图像、文本和按钮。
      * **背景图像**:图像应与用户从中访问WKND网站页面的状态相关。
      * **文本**:&quot;注册Audition&quot;
      * **按钮**:指向WKND SkateFest页面的“事件详细信息”
      * **WKND SkateFest页面**:包含事件详细信息（包括audition地点、日期和时间）的新页面。
1. 根据要求，AEM内容编辑器会为内容块创建一个体验片段，并将其作为选件导出到Adobe Target。 为了为美国的所有州提供个性化内容，内容作者可以创建一个体验片段主控变量，然后创建50个其他变量，每个州一个。 然后，可以手动编辑每个状态变体的内容以及相关的图像和文本。 在创作体验片段时，内容编辑器可以使用资产查找器选项快速访问AEM Assets中可用的所有资产。 当体验片段导出到Adobe Target时，其所有变量也会作为选件推送到Adobe Target。

1. 在将体验片段从AEM导出为“选件”后，营销人员可以使用这些选件在Target中创建活动。 根据WKND站点SkateFest营销活动，营销人员需要创建每个州的WKND站点访客并向其提供个性化体验。 要创建体验定位活动，营销人员需要识别受众。 对于WKND SkateFest活动，我们需要根据访客访问WKND网站的位置创建50个不同的受众。
   * [](https://experienceleague.adobe.com/docs/target/using/introduction/target-key-concepts.html#section_3F32DA46BDF947878DD79DBB97040D01) 受众为您的活动定义目标，可在任何有定位的地方使用。Target受众是一组定义的访客标准。 选件可以定位到特定受众（或区段）。 只有属于该受众的访客才会看到针对他们的体验。  例如，您可以向由使用特定浏览器或来自特定地理位置的访客组成的受众交付选件。
   * [Offer](https://experienceleague.adobe.com/docs/target/using/introduction/target-key-concepts.html#section_973D4CC4CEB44711BBB9A21BF74B89E9)是在营销活动或活动期间在您的网页上显示的内容。 在测试网页时，您可以使用不同位置的选件衡量每个体验的成功与否。 选件可以包含不同类型的内容，包括：
      * 图像
      * 文本
      * **HTML**
         * *HTML选件将用于此方案的活动*
      * 链接
      * 按钮

## 内容编辑器活动

>[!VIDEO](https://video.tv.adobe.com/v/28596?quality=12&learn=on)

>[!NOTE]
>
>在将体验片段导出到Adobe Target之前，先发布该体验片段。

## 营销人员活动

### 通过地域定位创建受众 {#marketer-audience}

1. 导航到您的组织[Adobe Experience Cloud](https://experiencecloud.adobe.com/)(`<https://<yourcompany>.experiencecloud.adobe.com`)
1. 使用Adobe ID登录，并确保您所在的组织正确。
1. 在解决方案切换器中，单击&#x200B;**Target**，然后单击&#x200B;**launch** Adobe Target。

   ![Experience Cloud- Adobe Target](assets/personalization-use-case-1/exp-cloud-adobe-target.png)

1. 导航到&#x200B;**选件**&#x200B;选项卡并搜索“WKND”选件。 您应该能够看到从AEM导出为HTML选件的体验片段变量列表。 每个选件都对应一个状态。 例如，*WKND SkateFest California*&#x200B;是提供给来自加利福尼亚的WKND站点访客的选件。

   ![Experience Cloud- Adobe Target](assets/personalization-use-case-1/html-offers.png)

1. 在主导航中，单击&#x200B;**受众**。

   营销人员需要为来自美国每个州的WKND网站访客创建50个单独的受众。

1. 要创建受众，请单击&#x200B;**创建受众**&#x200B;按钮，并为受众提供名称。

   **受众名称格式：WKND-\&lt;>state *\>***

   ![Experience Cloud- Adobe Target](assets/personalization-use-case-1/audience-target-1.png)

1. 单击&#x200B;**添加规则> Geo**。
1. 单击&#x200B;**选择**，然后选择以下选项之一：
   * 国家/地区
   * **状态** *（为WKND Site SkateFest营销活动选择状态）*
   * 城市
   * 邮政编码
   * 纬度
   * 经度
   * DMA
   * 移动设备运营商

   **地域**  — 使用受众根据用户的地理位置（包括其国家/地区、省/自治区/直辖市、城市、邮编/邮政编码、DMA或移动设备运营商）定位用户。地理位置参数允许您根据访客的地理位置定位活动和体验。 此数据随每个Target请求一起发送，并基于访客的IP地址。 与选择任何定位值一样，选择这些参数。

   >[!NOTE]
   >访客的IP地址会通过mbox请求进行传递，每次访问（会话）一次，以解析该访客的地理定位参数。

1. 选择运算符作为&#x200B;**匹配**，提供相应的值(例如：California)和&#x200B;**保存**&#x200B;您所做的更改。 在本例中，请提供州名称。

   ![Adobe Target — 地理规则](assets/personalization-use-case-1/audience-geo-rule.png)

   >[!NOTE]
   >您可以为受众分配多个规则。

1. 重复步骤6-9，为其他状态创建受众。

   ![Adobe Target - WKND受众](assets/personalization-use-case-1/adobe-target-audiences-50.png)

此时，我们已成功为位于美国不同州的所有WKND网站访客创建受众，并为每个州都提供相应的HTML选件。 现在，让我们创建一个体验定位活动，以使用WKND网站主页的相应选件来定位受众。

### 通过地域定位创建活动

1. 在Adobe Target窗口中，导航到&#x200B;**Activities**&#x200B;选项卡。
1. 单击&#x200B;**创建活动**，然后选择&#x200B;**体验定位**&#x200B;活动类型。
1. 选择&#x200B;**Web**&#x200B;渠道，然后选择&#x200B;**可视化体验编辑器**。
1. 输入&#x200B;**活动URL**，然后单击&#x200B;**下一步**&#x200B;以打开可视化体验编辑器。

   WKND站点主页发布URL:http://localhost:4503/content/wknd/en.html

   ![体验定位活动](assets/personalization-use-case-1/target-activity.png)

1. 要加载&#x200B;**可视化体验编辑器**，请在浏览器中启用&#x200B;**允许加载不安全脚本**&#x200B;并重新加载页面。

   ![体验定位活动](assets/personalization-use-case-1/load-unsafe-scripts.png)

1. 请注意在可视化体验编辑器编辑器中打开WKND站点主页。

   ![VEC](assets/personalization-use-case-1/vec.png)

1. 要向VEC添加受众，请单击“受众”下的&#x200B;**添加体验定位**，选择WKND-California受众，然后单击&#x200B;**下一步**。

   ![VEC](assets/personalization-use-case-1/vec-select-audience.png)

1. 单击VEC中的WKND网站页面，选择HTML元素以添加WKND-California受众的选件，选择&#x200B;**替换为**&#x200B;选项，然后选择&#x200B;**HTML选件**。

   ![体验定位活动](assets/personalization-use-case-1/vec-selecting-div.png)

1. 从选件选择UI中为&#x200B;**WKND-California**&#x200B;受众选择&#x200B;**WKND SkateFest California** HTML选件，然后单击&#x200B;**Done**。
1. 现在，您应该能够看到WKND-California受众的WKND网站页面中添加了&#x200B;**WKND SkateFest California** HTML选件。
1. 重复第7-10步，为其他状态添加体验定位，然后选择相应的HTML选件。
1. 单击&#x200B;**下一步**&#x200B;以继续，此时您可以看到“受众”到“体验”的映射。
1. 单击&#x200B;**下一步**&#x200B;以移动到“目标和设置”。
1. 选择报表源并确定活动的主要目标。 对于我们的方案，我们选择报表源作为&#x200B;**Adobe Target**，将活动测量为&#x200B;**转化**，将操作视为页面，以及指向WKND SkateFest详细信息页面的URL。

   ![目标和定位 — Target](assets/personalization-use-case-1/goal-metric-target.png)

   >[!NOTE]
   >您还可以选择Adobe Analytics作为报表源。

1. 将鼠标悬停在当前活动名称上，可将其重命名为&#x200B;**WKND SkateFest - USA**，然后单击&#x200B;**保存并关闭**&#x200B;您所做的更改。
1. 在活动详细信息屏幕中，确保&#x200B;**激活**&#x200B;您的活动。

   ![激活活动](assets/personalization-use-case-1/activate-activity.png)

1. 您的WKND SkateFest营销活动现已对所有WKND站点访客实时启用。
1. 导航到[WKND站点主页](http://localhost:4503/content/wknd/en.html)，您应该能够根据您的地理位置（*状态）查看WKND SkateFest选件：California*)。

   ![活动QA](assets/personalization-use-case-1/wknd-california.png)

### Target活动QA

1. 在&#x200B;**活动详细信息>概述**&#x200B;选项卡下，单击&#x200B;**活动QA**&#x200B;按钮，您可以获得所有体验的直接QA链接。

   ![活动QA](assets/personalization-use-case-1/activity-qa.png)

1. 导航到[WKND站点主页](http://localhost:4503/content/wknd/en.html)，您应该能够根据您的地理位置（状态）查看WKND SkateFest选件。
1. 请观看以下视频，了解如何将选件交付到您的页面、如何自定义响应令牌以及执行质量检查。

>[!VIDEO](https://video.tv.adobe.com/v/28658?quality=12&learn=on)

## 摘要

在本章中，内容编辑器能够创建所有内容以支持Adobe Experience Manager中的WKND SkateFest营销活动，并将其导出为HTML选件，以便根据用户的地理位置创建体验定位。
