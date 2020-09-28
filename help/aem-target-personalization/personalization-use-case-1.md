---
title: 使用AEM Experience Fragments和Adobe Target实现个性化
seo-title: 使用Adobe Experience Manager(AEM)体验片段和Adobe Target实现个性化
description: 一个端到端教程，其中演示如何使用Adobe Experience Manager体验片段和Adobe Target创建和提供个性化体验。
seo-description: 一个端到端教程，其中演示如何使用Adobe Experience Manager体验片段和Adobe Target创建和提供个性化体验。
translation-type: tm+mt
source-git-commit: 1209064fd81238d4611369b8e5b517365fc302e3
workflow-type: tm+mt
source-wordcount: '1729'
ht-degree: 1%

---


# 使用AEM Experience Fragments和Adobe Target实现个性化

凭借将AEM Experience Fragments作为HTML优惠导出到Adobe Target的能力，您可以将AEM的易用性和强大功能与目标中强大的自动智能(AI)和机器学习(ML)功能结合起来，大规模地测试和个性化体验。

AEM将您的所有内容和资产集中在一个中心位置，以推动您的个性化战略。 通过AEM，您无需编写代码，即可在一个位置轻松创建适用于台式机、平板电脑和移动设备的内容。 无需为每个设备创建页面-AEM使用您的内容自动调整每个体验。

目标允许您根据融合了行为、情境和离线变量的基于规则和人工智能驱动的机器学习方法的组合大规模提供个性化体验。  借助目标，您可以轻松设置和运行A/B和多变量(MVT)活动，以确定最佳优惠、内容和体验。

体验片段是将内容创建者与使用目标推动业务成果的营销人员联系在一起的一个重大进步。

## 方案概述

WKND网站计划通过其 **网站** ，在美国各地宣布SkateFest挑战赛，并希望其网站用户注册参加在每个州进行的试镜。 作为营销人员，您已被分配任务在WKND站点主页上运行活动，该包含与用户位置相关的横幅消息和指向事件详细信息页面的链接。 让我们探索WKND站点主页，并了解如何根据用户当前所在的位置为其创建和提供个性化体验。

### 涉及的用户

对于本练习，需要参与以下用户，并执行某些任务，您可能需要管理访问权限。

* **内容制作者／内容编辑** (Adobe Experience Manager)
* **营销人员** (Adobe Target/优化团队)

### 前提条件

* **AEM**
   * [AEM author和publish实例](./implementation.md#getting-aem) （分别运行于localhost 4502和4503上）。
* **Experience Cloud**
   * 访问您的组织Adobe Experience Cloud <https://>`<yourcompany>`-.experiencecloud.adobe.com
   * Experience Cloud配置了以下解决方案
      * [Adobe Target](https://experiencecloud.adobe.com)

### WKND站点主页

![AEM目标方案1](assets/personalization-use-case-1/aem-target-use-case-1-4.png)

1. 营销人员与AEM内容编辑者发起WKND SkateFest活动讨论，并详细了解要求。
   * ***要求***:在WKND站点活动上为来自美国各州的访客宣传WKND SkateFest主页，提供个性化内容。 在主页轮盘下添加一个新内容块，其中包含背景图像、文本和按钮。
      * **背景图像**:图像应与用户从中访问WKND站点页面的状态相关。
      * **文本**:&quot;注册Audition&quot;
      * **按钮**:指向WKND SkateFest页面的“事件详细信息”
      * **WKND SkateFest页面**:包含事件详细信息（包括audition地点、日期和时间）的新页面。
2. 根据要求，AEM Content Editor为内容块创建体验片段并将其作为优惠导出到Adobe Target。 为了为美国所有州提供个性化内容，内容作者可以创建一个体验片段主控变体，然后创建50个其他变体，每个州创建一个。 然后，可以手动编辑每个状态变量与相关图像和文本的内容。 创作体验片段时，内容编辑器可以使用资产查找器选项快速访问AEM Assets内的所有可用资产。 当体验片段导出到Adobe Target时，其所有变体也作为优惠被推送到Adobe Target。

3. 将体验片段从AEM导出到Adobe Target作为优惠后，营销人员可以使用这些优惠创建目标活动。 根据WKND站点滑冰节活动，营销人员需要从每个州创建个性化体验并提供给WKND站点访客。 要创建体验定位活动，营销人员需要识别受众。 对于我们的WKND SkateFest活动，我们需要根据受众访问WKND网站的位置创建50个单独的客户。
   * [受众](https://docs.adobe.com/content/help/en/target/using/introduction/target-key-concepts.html#section_3F32DA46BDF947878DD79DBB97040D01) 为您的活动定义目标，可在可进行定位的任何地方使用。 目标受众是一组定义的访客条件。 优惠可以针对特定受众（或区段）。 只有属于该受众的访客才能看到针对他们的体验。  例如，您可以将优惠传送到由使用特定浏览器或特定地理位置的访客组成的受众。
   * 优惠 [是](https://docs.adobe.com/content/help/en/target/using/introduction/target-key-concepts.html#section_973D4CC4CEB44711BBB9A21BF74B89E9) 在活动或活动期间在您的网页上显示的内容。 在测试网页时，可以使用您所在位置的不同优惠衡量每次体验的成功程度。 优惠可以包含不同类型的内容，包括：
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

### 创建具有地理定位的受众 {#marketer-audience}

1. 导航到您的组织 [Adobe Experience Cloud](https://experiencecloud.adobe.com/) (<https://>`<yourcompany>`.experiencecloud.adobe.com)
2. 使用您的Adobe ID登录，并确保您位于正确的组织中。
3. 从解决方案切换器中，单击 **目标** ，然 **后启动** Adobe Target。

   ![Experience Cloud-Adobe Target](assets/personalization-use-case-1/exp-cloud-adobe-target.png)

4. 导航到 **优惠** 选项卡并搜索“WKND”优惠。 您应该能够看到体验片段变体的列表，从AEM导出为HTML优惠。 每个优惠对应一个状态。 例如，WKND SkateFest *California是从* California向WKND Site访客提供服务的优惠。

   ![Experience Cloud-Adobe Target](assets/personalization-use-case-1/html-offers.png)

5. 在主导航中，单击 **受众**。

   营销人员需要为来自美国各州的WKND站点受众创建50个单独的访客。

6. 要创建受众，请单击“ **创建受众** ”按钮，并为受众提供名称。

   **受众名格式：WKND-\&lt;*state*\>**

   ![Experience Cloud-Adobe Target](assets/personalization-use-case-1/audience-target-1.png)

7. 单击 **添加规则>地理**。
8. 单击 **选择**，然后选择以下选项之一：
   * 国家/地区
   * **状态** ( *为WKND站点滑冰节活动选择状态)*
   * 城市
   * 邮政编码
   * 纬度
   * 经度
   * DMA
   * 移动运营商

   **地理** -根据用户所在的地理位置（包括其国家／地区、省／省、城市、邮政编码、DMA或移动运营商），使用受众目标用户。 地理位置参数允许您根据访客的地理位置目标活动和体验。 此数据随每个目标请求一起发送，并基于访客的IP地址。 像选择任何定位值一样选择这些参数。

   >[!NOTE]
   >访客的IP地址随mbox请求一起传递，每次访问（会话）一次，以解析该访客的地理定位参数。

9. 选择匹配的运 **算符**，提供相应的值(例如：California)并保 **存您** 所做的更改。 在本例中，请提供州名。

   ![Adobe Target-地理规则](assets/personalization-use-case-1/audience-geo-rule.png)

   >[!NOTE]
   >您可以为受众分配多个规则。

10. 重复第6-9步，为其他状态创建受众。

   ![Adobe Target- WKND受众](assets/personalization-use-case-1/adobe-target-audiences-50.png)

此时，我们已成功为美国不同州的所有WKND站点受众创建了访客，并为每个州创建了相应的HTML优惠。 现在，让我们创建一个体验定位活动，用WKND站点目标的相应优惠主页受众。

### 创建具有地理定位的活动

1. 在您的Adobe Target窗口中，导航到 **活动** 选项卡。
2. 单击 **创建活动** ，然后选择 **体验定位** 活动类型。
3. 选择Web **渠道** ，然后选择 **可视体验书写器**。
4. 输入 **活动URL** ，然后 **单击** “下一步”打开Visual Experience Composer。

   WKND站点主页发布URL:http://localhost:4503/content/wknd/en.html
   ![体验定位活动](assets/personalization-use-case-1/target-activity.png)
5. 要加 **载Visual Experience Composer** ，请在浏览器 **上启用“允许加载不安全脚本** ”并重新加载页面。
   ![体验定位活动](assets/personalization-use-case-1/load-unsafe-scripts.png)
6. 请注意，WKND站点主页在Visual Experience Composer编辑器中打开。
   ![VEC](assets/personalization-use-case-1/vec.png)
7. 要向VEC中添加受众，请单击“ **受众”下的** “添加体验定位”，然后选择WKND-California受众并单击“下 **一步”**。
   ![VEC](assets/personalization-use-case-1/vec-select-audience.png)
8. 单击VEC中的WKND站点页，选择HTML元素以添加WKND-California优惠的受众，选择“ **替换为** ”选项，然后选择 **HTML优惠**。
   ![体验定位活动](assets/personalization-use-case-1/vec-selecting-div.png)
9. 从“WKND- **California”优惠** 中选择WKND SkateFest California **HTML优惠,** 使用WKND-California **受众，然后单击“**&#x200B;完成”。
10. 您现在应能看到WKND SkateFest California **** HTML优惠已添加到WKND-California受众的WKND站点页面。
11. 重复第7-10步，为其他状态添加体验定位，然后选择相应的HTML优惠。
12. 单击 **下一** 步继续，您可以看到受众到体验的映射。
13. 单击 **下一** 步，移至目标和设置。
14. 选择报告源并确定活动的主要目标。 对于我们的方案，我们选择报告源作 **为Adobe Target**，将活动度量为转 **化，将操作视为页面**，并将URL指向WKND SkateFest详细信息页面。
   ![目标和定位-目标](assets/personalization-use-case-1/goal-metric-target.png)

   >[!NOTE]
   >您还可以选择Adobe Analytics作为报告源。

15. 将鼠标悬停在当前活动名称上，可将其重命名 **为WKND SkateFest - USA**，然后 **保存并关闭您的更** 改。
16. 从活动详细信息屏幕，确保激 **活** 活动。
   ![激活活动](assets/personalization-use-case-1/activate-activity.png)
17. 您的WKND SkateFest活动现已对所有WKND Site访客实时开放。
18. 导航到WKND [站点主页](http://localhost:4503/content/wknd/en.html)，您应能够根据您的地理位置查看WKND SkateFest优惠(*状态：加州*)。
   ![活动QA](assets/personalization-use-case-1/wknd-california.png)

### 目标活动QA

1. 在“ **活动详细信息** ”>“概述”选项卡下，单 **击“活动QA** ”按钮，您可以获得指向所有体验的直接QA链接。
   ![活动QA](assets/personalization-use-case-1/activity-qa.png)
2. 导航到 [WKND站点主页](http://localhost:4503/content/wknd/en.html)，您应能够根据您的地理位置（状态）查看WKND SkateFest优惠。
3. 观看以下视频，了解优惠如何传送到您的页面、如何自定义响应令牌以及执行质量检查。

>[!VIDEO](https://video.tv.adobe.com/v/28658?quality=12&learn=on)

## 摘要

在本章中，内容编辑器能够创建支持Adobe Experience Manager内WKND SkateFest活动的所有内容，并将其作为HTML优惠导出到Adobe Target，以根据用户地理位置创建体验定位。
