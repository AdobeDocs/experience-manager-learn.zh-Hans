---
title: 使用AEM Experience Fragments和Adobe Target实现个性化
seo-title: 使用Adobe Experience Manager(AEM)Experience Fragments和Adobe Target实现个性化
description: 一个端到端教程，其中演示了如何使用Adobe Experience Manager Experience Fragments和Adobe Target创建和提供个性化体验。
seo-description: 一个端到端教程，其中演示了如何使用Adobe Experience Manager Experience Fragments和Adobe Target创建和提供个性化体验。
feature: 体验片段
topic: 个性化
role: 开发人员
level: 中间
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1734'
ht-degree: 1%

---


# 使用AEM Experience Fragments和Adobe Target实现个性化

借助将AEM Experience Fragments作为HTML优惠导出到Adobe Target的能力，您可以将AEM的易用性和强大功能与目标中强大的自动智能(AI)和机器学习(ML)功能相结合，以大规模测试和个性化体验。

AEM将您的所有内容和资产集中在一个中心位置，助您实现个性化战略。 借助AEM，您可以在一个位置轻松创建适合台式机、平板电脑和移动设备的内容，而无需编写代码。 无需为每个设备创建页面 — AEM使用您的内容自动调整每个体验。

目标使您能够结合基于规则和人工智能驱动的机器学习方法，大规模提供个性化体验，这些方法包含行为、情境和离线变量。  借助目标，您可以轻松设置和运行A/B和多变量(MVT)活动，以确定最佳优惠、内容和体验。

体验片段是将内容创建者与使用目标推动业务成果的营销人员联系在一起的一个重大进步。

## 方案概述

WKND网站计划通过其网站在美国各地宣布一个&#x200B;**SkateFest Challenge**，并希望其网站用户注册参加在每个州进行的试镜。 作为营销人员，您已分配任务在WKND站点主页上运行活动，其中包含与用户位置相关的横幅消息以及指向事件详细信息页面的链接。 让我们浏览WKND网站主页，了解如何根据用户当前所在的位置为用户创建和提供个性化体验。

### 涉及的用户

对于本练习，需要参与以下用户，并执行某些任务，您可能需要管理访问权限。

* **Content Producer/Content Editor** (Adobe Experience Manager)
* **营销人** (Adobe Target/优化团队)

### 前提条件

* **AEM**
   * [AEM author和publish ](./implementation.md#getting-aem) instancerunning分别位于localhost 4502和4503上。
* **Experience Cloud**
   * 访问您的组织Adobe Experience Cloud - <https://>`<yourcompany>`.experiencecloud.adobe.com
   * Experience Cloud配置了以下解决方案
      * [Adobe Target](https://experiencecloud.adobe.com)

### WKND站点主页

![AEM 目标方案1](assets/personalization-use-case-1/aem-target-use-case-1-4.png)

1. 营销人员与AEM内容编辑器发起WKND SkateFest活动讨论，并详细说明要求。
   * ***要求***:在WKND网站主页上为来自美国每个州的访客提供个性化内容，宣传WKND SkateFest活动。在包含背景图像、文本和按钮的主页传送下面添加新内容块。
      * **背景图像**:图像应与用户从中访问WKND站点页面的状态相关。
      * **文本**:&quot;注册Audition&quot;
      * **按钮**:指向WKND SkateFest页面的“事件详细信息”
      * **WKND SkateFest页面**:包含事件详细信息（包括audition地点、日期和时间）的新页面。
1. 根据要求，AEM Content Editor为内容块创建一个体验片段，并将其作为优惠导出到Adobe Target。 为了为美国所有州提供个性化内容，内容作者可以创建一个体验片段主控变体，然后创建50个其他变体，每个州创建一个。 随相关图像和文本变化的每个状态的内容随后可以手动编辑。 创作体验片段时，内容编辑器可以使用资产查找器选项快速访问AEM Assets中的所有可用资产。 当体验片段导出到Adobe Target时，其所有变体也作为优惠推送到Adobe Target。

1. 将体验片段从AEM导出到Adobe Target作为优惠后，营销人员可以使用这些优惠在目标中创建活动。 基于WKND站点SkateFest活动，营销人员需要创建个性化体验并从每个州向WKND站点访客提供个性化体验。 要创建体验定位活动，营销人员需要识别受众。 对于我们的WKND SkateFest活动，我们需要根据受众访问WKND网站的位置创建50个单独的客户。
   * [受](https://docs.adobe.com/content/help/en/target/using/introduction/target-key-concepts.html#section_3F32DA46BDF947878DD79DBB97040D01) 众为您的活动定义目标，并可在可进行定位的任何位置使用。目标受众是一组定义的访客条件。 优惠可以针对特定受众（或区段）。 只有属于该受众的访客才能看到针对他们的体验。  例如，您可以将优惠传送到由使用特定浏览器或从特定地理位置的访客组成的受众。
   * [优惠](https://docs.adobe.com/content/help/en/target/using/introduction/target-key-concepts.html#section_973D4CC4CEB44711BBB9A21BF74B89E9)是在活动或活动期间在您的网页上显示的内容。 在测试网页时，可使用不同优惠评估每个体验在您所在位置的成功程度。 优惠可以包含不同类型的内容，包括：
      * 图像
      * 文本
      * **HTML**
         * *HTML优惠将用于此方案的活动*
      * 链接
      * 按钮

## 内容编辑器活动

>[!VIDEO](https://video.tv.adobe.com/v/28596?quality=12&learn=on)

>[!NOTE]
>
>在将体验片段导出到Adobe Target之前发布它。

## 营销人员活动

### 创建具有地理定位{#marketer-audience}的受众

1. 导航到您的组织[Adobe Experience Cloud](https://experiencecloud.adobe.com/)(<https://>`<yourcompany>`.experiencecloud.adobe.com)
1. 使用Adobe ID登录，并确保您所在的组织正确。
1. 在解决方案切换器中，单击&#x200B;**目标**，然后单击&#x200B;**启动** Adobe Target。

   ![Experience Cloud- Adobe Target](assets/personalization-use-case-1/exp-cloud-adobe-target.png)

1. 导航到&#x200B;**优惠**&#x200B;选项卡并搜索“WKND”优惠。 您应能够查看体验片段变量的列表，从AEM导出为HTML优惠。 每个优惠对应一个状态。 例如，*WKND SkateFest California*&#x200B;是从加利福尼亚提供给WKND站点访客的优惠。

   ![Experience Cloud- Adobe Target](assets/personalization-use-case-1/html-offers.png)

1. 在主导航中，单击&#x200B;**受众**。

   营销人员需要为来自美国每个州的WKND站点访客创建50个单独的受众。

1. 要创建受众，请单击&#x200B;**创建受众**&#x200B;按钮，并为受众提供名称。

   **受众名格式：WKND-\&lt;>state *\>***

   ![Experience Cloud- Adobe Target](assets/personalization-use-case-1/audience-target-1.png)

1. 单击&#x200B;**添加规则> Geo**。
1. 单击&#x200B;**选择**，然后选择以下选项之一：
   * 国家/地区
   * **状态** *(为WKND Site SkateFest活动选择状态)*
   * 城市
   * 邮政编码
   * 纬度
   * 经度
   * DMA
   * 移动运营商

   **地理**  — 根据目标用户的地理位置（包括其所在国家/地区、省/省、城市、邮政编码、DMA或移动运营商），使用受众。地理位置参数允许您根据访客的地理位置目标活动和体验。 此数据随每个目标请求一起发送，并基于访客的IP地址。 像选择任何定位值一样选择这些参数。

   >[!NOTE]
   >访客的IP地址随mbox请求一起传递，每次访问（会话）一次，以解析该访客的地理定位参数。

1. 选择&#x200B;**匹配**&#x200B;的运算符，提供相应的值(例如：California)和&#x200B;**保存您所做的更改。**&#x200B;在我们的案例中，请提供州名。

   ![Adobe Target — 地理规则](assets/personalization-use-case-1/audience-geo-rule.png)

   >[!NOTE]
   >您可以为受众分配多个规则。

1. 重复第6-9步，为其他状态创建受众。

   ![Adobe Target- WKND受众](assets/personalization-use-case-1/adobe-target-audiences-50.png)

此时，我们已成功为美国不同州间的所有WKND站点访客创建受众，并且每个州都有相应的HTML优惠。 现在，让我们创建一个体验定位活动，用WKND站点主页的相应优惠目标受众。

### 创建具有地理定位的活动

1. 在Adobe Target窗口中，导航到&#x200B;**活动**&#x200B;选项卡。
1. 单击&#x200B;**创建活动**，然后选择&#x200B;**体验定位**&#x200B;活动类型。
1. 选择&#x200B;**Web**&#x200B;渠道，然后选择&#x200B;**Visual Experience Composer**。
1. 输入&#x200B;**活动URL**，然后单击&#x200B;**下一步**&#x200B;以打开可视体验书写器。

   WKND站点主页发布URL:http://localhost:4503/content/wknd/en.html

   ![体验定位活动](assets/personalization-use-case-1/target-activity.png)

1. 要加载&#x200B;**Visual Experience Composer**，请在浏览器上启用&#x200B;**允许加载不安全脚本**&#x200B;并重新加载页面。

   ![体验定位活动](assets/personalization-use-case-1/load-unsafe-scripts.png)

1. 请注意，WKND站点主页在Visual Experience Composer编辑器中打开。

   ![VEC](assets/personalization-use-case-1/vec.png)

1. 要向VEC添加受众，请单击“受众”下的&#x200B;**添加体验定位**，然后选择WKND-California受众并单击&#x200B;**下一步**。

   ![VEC](assets/personalization-use-case-1/vec-select-audience.png)

1. 单击VEC中的WKND站点页，选择HTML元素以添加WKND-California受众的优惠，然后选择&#x200B;**替换为**&#x200B;选项，然后选择&#x200B;**HTML优惠**。

   ![体验定位活动](assets/personalization-use-case-1/vec-selecting-div.png)

1. 从优惠选择UI中为&#x200B;**WKND-California**&#x200B;受众选择&#x200B;**WKND SkateFest California** HTML优惠，然后单击&#x200B;**Done**。
1. 您现在应能看到WKND-California优惠的WKND站点页面中添加的&#x200B;**WKND SkateFest California** HTML受众。
1. 重复第7-10步，为其他状态添加“体验定位”，然后选择相应的HTML优惠。
1. 单击&#x200B;**下一步**&#x200B;继续，您会看到受众到体验的映射。
1. 单击&#x200B;**下一步**&#x200B;移至“目标和设置”。
1. 选择报告源并确定活动的主要目标。 对于我们的方案，我们选择报告源作为&#x200B;**Adobe Target**，将活动度量为&#x200B;**转换**，将操作作为查看的页面，以及指向WKND SkateFest详细信息页面的URL。

   ![目标和定位 — 目标](assets/personalization-use-case-1/goal-metric-target.png)

   >[!NOTE]
   >您还可以选择Adobe Analytics作为报告源。

1. 将鼠标悬停在当前活动名称上，您可以将其重命名为&#x200B;**WKND SkateFest - USA**，然后单击&#x200B;**保存并关闭**&#x200B;您所做的更改。
1. 在活动详细信息屏幕中，确保&#x200B;**激活**&#x200B;您的活动。

   ![激活活动](assets/personalization-use-case-1/activate-activity.png)

1. 您的WKND SkateFest活动现已对所有WKND站点访客实时。
1. 导航到[WKND站点主页](http://localhost:4503/content/wknd/en.html)，您应该能够根据您的地理位置(*状态：California*)。

   ![活动 QA](assets/personalization-use-case-1/wknd-california.png)

### 目标活动QA

1. 在&#x200B;**活动详细信息>概述**&#x200B;选项卡下，单击&#x200B;**活动QA**&#x200B;按钮，您可以获得指向所有体验的直接QA链接。

   ![活动 QA](assets/personalization-use-case-1/activity-qa.png)

1. 导航到[WKND站点主页](http://localhost:4503/content/wknd/en.html)，您应该能够根据您的地理位置（状态）查看WKND SkateFest优惠。
1. 观看以下视频，了解优惠如何传递到您的页面、如何自定义响应令牌以及执行质量检查。

>[!VIDEO](https://video.tv.adobe.com/v/28658?quality=12&learn=on)

## 摘要

在本章中，内容编辑器能够创建所有内容以支持Adobe Experience Manager中的WKND SkateFest活动，并将其导出为HTML优惠，以便根据用户地理位置创建体验定位。
