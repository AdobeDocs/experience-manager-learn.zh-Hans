---
title: 了解内容片段和体验片段
description: Adobe Experience Manager的内容片段和体验片段表面上看似相似，但在不同的用例中，每个片段都起着关键作用。 了解内容片段和体验片段如何相似、不同，以及何时以及如何使用。
sub-product: 资产，站点，内容服务
feature: Content Fragments, Experience Fragments
topics: headless
version: 6.3, 6.4, 6.5
doc-type: article
activity: understand
audience: all
topic: Content Management
role: Business Practitioner
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1007'
ht-degree: 3%

---


# 了解内容片段和体验片段

Adobe Experience Manager的内容片段和体验片段表面上看似相似，但在不同的用例中，每个片段都起着关键作用。 了解内容片段和体验片段如何相似、不同，以及何时以及如何使用。

## 内容片段和体验片段比较

<table>
<tbody><tr><td><strong> </strong></td>
<td><strong>内容片段(CF)</strong></td>
<td><strong>体验片段(XF)</strong></td>
</tr><tr><td><strong>定义</strong></td>
<td><ul>
<li>可重用的、不可知表示的<strong>内容</strong>，由结构化数据元素（文本、日期、引用等）组成</li>
</ul>
</td>
<td><ul>
<li>一个或多个AEM组件的可重用组合，用于定义构成<strong>体验</strong>的内容和演示，该体验本身具有意义</li>
</ul>
</td>
</tr><tr><td><strong>核心租户</strong></td>
<td><ul>
<li>以内容为中心</li>
<li>由<a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/content-fragments-models.html" target="_blank">结构化、基于表单的数据模型定义。</a></li>
<li>与设计和布局无关。</li>
<li>渠道拥有内容片段内容（布局和设计）的演示文稿</li>
</ul>
</td>
<td><ul>
<li>以演示为中心</li>
<li>由AEM组件的非结构化组合定义</li>
<li>定义内容的设计和布局</li>
<li>在渠道中使用“原样”</li>
</ul>
</td>
</tr><tr><td><strong>技术详细信息</strong></td>
<td><ul>
<li>已实施为<strong>dam:Asset</strong></li>
<li>由<a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/content-fragments-models.html" target="_blank">内容片段模型</a>定义</li>
</ul>
</td>
<td><ul>
<li>实现为<strong>cq:Page</strong></li>
<li>由可编辑模板定义</li>
<li>本机HTML再现</li>
</ul>
</td>
</tr><tr><td><strong>变量</strong></td>
<td><ul>
<li>主控变量是正则变量</li>
<li>变量是特定用例，可能与渠道对齐。</li>
</ul>
</td>
<td><ul>
<li>变量是特定于渠道或上下文的</li>
<li>变量通过AEM Live Copy保持同步</li>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/experience-fragments.html#BuildingBlocks" target="_blank">构建</a> 块化内容，跨各种变量重复使用</li>
</ul>
</td>
</tr><tr><td><strong>功能</strong></td>
<td><ul>
<li>变量</li>
<li>版本</li>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/content-fragments-variations.html#SynchronizingwithMaster" target="_blank">跨</a> 不同的内容同步</li>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/content-fragments-managing.html#ComparingFragmentVersions" target="_blank">内容</a> 片段版本的可视差异</li>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/content-fragments-variations.html#AnnotatingaContentFragment" target="_blank"></a> 多行文本元素注释</li>
<li>智能<a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/content-fragments-variations.html#SummarizingText" target="_blank">多行文本元素的总结</a>。</li>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/creating-translation-projects-for-content-fragments.html" target="_blank">翻译/本地化</a></li>
</ul>
</td>
<td><ul>
<li>变量</li>
<li>Variations as Live Copy</li>
<li>版本</li>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/experience-fragments.html#BuildingBlocks" target="_blank">构建基块</a></li>
<li>注释</li>
<li>响应式布局和预览</li>
<li>翻译/本地化</li>
</ul>
</td>
</tr><tr><td><strong>用法</strong></td>
<td><ul>
<li><a href="https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/content-fragment-component.html" target="_blank">AEM核心组件内容片</a> 段组件，用于AEM Sites、AEM Screens或体验片段。</li>
<li>通过<a href="https://helpx.adobe.com/experience-manager/kt/sites/using/content-services-tutorial-use.html" target="_blank">AEM Content Services</a>导出JSON，用于第三方使用</li>
<li>通过AEM HTTP Assets API实现JSON，用于第三方使用。</li>
</ul>
</td>
<td><ul>
<li>AEM Experience Fragment组件，用于AEM Sites、AEM Screens或其他体验片段。</li>
<li>导出为<a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/experience-fragments.html#ThePlainHTMLRendition" target="_blank">纯HTML</a>供第三方系统使用</li>
<li><a href="https://helpx.adobe.com/cn/experience-manager/6-5/sites/administering/using/experience-fragments-target.html" target="_blank">HTML导出到Adobe目</a> 标以实现目标优惠</li>
<li>将JSON导出到Adobe Target以用于目标优惠</li>
</ul>
</td>
</tr><tr><td><strong>常见用例</strong></td>
<td><ul>
<li>高度结构化的数据输入/表单内容</li>
<li>长篇编辑内容（多行元素）</li>
<li>在提供内容的渠道的生命周期之外管理的内容</li>
</ul>
</td>
<td><ul>
<li>使用每个渠道变量集中管理多渠道促销宣传品。</li>
<li>在网站的多个页面中重复使用内容。</li>
<li>网站主题(例如 页眉和页脚</li>
<li>在提供体验的渠道的生命周期之外管理的体验</li>
</ul>
</td>
</tr><tr><td><strong>文档</strong></td>
<td><ul>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/assets/user-guide.html?topic=/experience-manager/6-5/assets/morehelp/content-fragments.ug.js" target="_blank">AEM内容片段用户指南</a></li>
<li><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/content-fragments-feature-video-use.html" target="_blank">在AEM中使用内容片段</a></li>
</ul>
</td>
<td><ul>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/experience-fragments.html" target="_blank">Adobe有关Experience Fragments的文档</a></li>
</ul>
</td>
</tr></tbody></table>

## 内容片段架构

下图说明了AEM内容片段的整体架构

!![内容片段架构](./assets/content-fragments-architecture.png)

+ **内容片** 段模型定义元素（或字段），这些元素（或字段）定义内容片段可能捕获和公开的内容。
+ **内容片段**&#x200B;是表示逻辑内容实体的内容片段模型的实例。
+ 内容片段&#x200B;**变量**&#x200B;与内容片段模型相符，但内容中存在变量。
+ 内容片段可由以下人员公开/使用：
   + 通过AEM WCM核心组件的内容片段组件在&#x200B;**AEM Sites**(或AEM Screens)上使用内容片段。
   + 通过AEM WCM核心组件的内容片段组件将内容片段嵌入&#x200B;**体验片段**&#x200B;中，以用于任何体验片段用例。
   + 通过&#x200B;**AEM Content Services**&#x200B;和API页面将内容片段变体内容公开为JSON，以用于只读用例。
   + 通过通过&#x200B;**AEM Assets HTTP API**&#x200B;直接调用AEM Assets（针对CRUD用例）将内容片段内容（所有变体）直接公开为JSON。

## 体验片段架构

!![体验片段架构](./assets/experience-fragments-architecture.png)

+ **可编辑模板**(又由可编辑模 **板类型** 和AEM页面组 **件实施定义)定义**，定义允许的可用于组成体验片段的AEM组件。
+ **体验片段**&#x200B;是可编辑模板的一个实例，它表示逻辑体验。
+ 体验片段&#x200B;**变量**&#x200B;符合可编辑模板，但体验（内容和设计）存在差异。
+ 体验片段可由以下人员公开/使用：
   + 在AEM Sites(或AEM Screens)上通过AEM Experience Fragment组件使用体验片段。
   + 通过&#x200B;**AEM Content Services**&#x200B;和API页面将体验片段变量内容公开为JSON（带有嵌入式HTML）。
   + 直接将体验片段变体公开为&#x200B;**“普通HTML”**。
   + 将体验片段导出到&#x200B;**Adobe Target**&#x200B;作为HTML或JSON优惠。
   + AEM Sites本身支持HTML优惠，但JSON优惠需要自定义开发。

## 内容片段的支持材料

+ [内容片段用户指南](https://helpx.adobe.com/experience-manager/6-5/assets/user-guide.html?topic=/experience-manager/6-5/assets/morehelp/content-fragments.ug.js)
+ [在AEM中使用内容片段](https://helpx.adobe.com/experience-manager/kt/sites/using/content-fragments-feature-video-use.html)
+ [AEM WCM核心组件的内容片段组件](https://docs.adobe.com/content/help/zh-Hans/experience-manager-core-components/using/components/content-fragment-component.html)
+ [使用内容片段和AEM内容服务](https://helpx.adobe.com/experience-manager/kt/sites/using/structured-fragments-content-services-feature-video-use.html)
+ [AEM Content Services入门](https://helpx.adobe.com/experience-manager/kt/sites/using/content-services-tutorial-use.html)

## 体验片段的支持材料

+ [Adobe有关Experience Fragments的文档](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/experience-fragments.html)
+ [了解AEM Experience Fragments](https://helpx.adobe.com/experience-manager/kt/sites/using/experience-fragments-feature-video-understand.html)
+ [使用AEM Experience Fragments](https://helpx.adobe.com/experience-manager/kt/sites/using/experience-fragments-feature-video-use.html)
+ [将AEM Experience Fragments与Adobe Target结合使用](https://medium.com/adobetech/experience-fragments-and-adobe-target-d8d74381b9b2)
