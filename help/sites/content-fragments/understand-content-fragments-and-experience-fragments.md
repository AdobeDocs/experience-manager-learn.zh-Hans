---
title: 了解内容片段和体验片段
description: Adobe Experience Manager的内容片段和体验片段在表面上看似相似，但在不同的用例中，每个片段都起着关键作用。 了解内容片段和体验片段如何相似、不同，以及何时和如何使用它们。
sub-product: 资产，站点，内容服务
feature: content fragments, experience fragments
topics: headless
version: 6.3, 6.4, 6.5
doc-type: article
activity: understand
audience: all
translation-type: tm+mt
source-git-commit: 03db12de4d95ced8fabf36b8dc328581ec7a2749
workflow-type: tm+mt
source-wordcount: '998'
ht-degree: 3%

---


# 了解内容片段和体验片段

Adobe Experience Manager的内容片段和体验片段在表面上看似相似，但在不同的用例中，每个片段都起着关键作用。 了解内容片段和体验片段如何相似、不同，以及何时和如何使用它们。

## 内容片段和体验片段比较

<table>
<tbody><tr><td><strong> </strong></td>
<td><strong>内容片段(CF)</strong></td>
<td><strong>体验片段(XF)</strong></td>
</tr><tr><td><strong>定义</strong></td>
<td><ul>
<li>可重用的、与表示形式无关的<strong>内容</strong>，由结构化数据元素（文本、日期、引用等）组成</li>
</ul>
</td>
<td><ul>
<li>一个或多个AEM组件的可重用组合，这些组件定义了构成<strong>体验</strong>的内容和演示文稿，该体验本身是有意义的</li>
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
<li>以演示文稿为中心</li>
<li>由AEM组件的非结构化组成定义</li>
<li>定义内容的设计和布局</li>
<li>在渠道中使用“原样”</li>
</ul>
</td>
</tr><tr><td><strong>技术详细信息</strong></td>
<td><ul>
<li>已实现为<strong>dam:Asset</strong></li>
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
<li>主控变分是正则变分</li>
<li>变体是特定于用例的，它可能与渠道一致。</li>
</ul>
</td>
<td><ul>
<li>变体是渠道或上下文特定的</li>
<li>变量通过AEM Live Copy保持同步</li>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/experience-fragments.html#BuildingBlocks" target="_blank">构建</a> 块式内容跨变量重用</li>
</ul>
</td>
</tr><tr><td><strong>功能</strong></td>
<td><ul>
<li>变量</li>
<li>版本</li>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/content-fragments-variations.html#SynchronizingwithMaster" target="_blank">跨不同</a> 版本同步内容</li>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/content-fragments-managing.html#ComparingFragmentVersions" target="_blank">内容</a> 片段版本的视觉差异</li>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/content-fragments-variations.html#AnnotatingaContentFragment" target="_blank">多</a> 行文本元素注释</li>
<li>智能<a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/content-fragments-variations.html#SummarizingText" target="_blank">多行文本元素的总结</a>。</li>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/creating-translation-projects-for-content-fragments.html" target="_blank">翻译/本地化</a></li>
</ul>
</td>
<td><ul>
<li>变量</li>
<li>作为Live Copy的变体</li>
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
<li>AEM体验片段组件，用于AEM Sites、AEM Screens或其他体验片段。</li>
<li>导出为<a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/experience-fragments.html#ThePlainHTMLRendition" target="_blank">纯HTML</a>供第三方系统使用</li>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/experience-fragments-target.html" target="_blank">HTML导出到Adobe目</a> 标以实现目标优惠</li>
<li>JSON导出至Adobe Target，面向目标优惠</li>
</ul>
</td>
</tr><tr><td><strong>常见用例</strong></td>
<td><ul>
<li>高度结构化的数据输入／表单内容</li>
<li>长篇编辑内容（多行元素）</li>
<li>在提供内容的渠道的生命周期之外管理的内容</li>
</ul>
</td>
<td><ul>
<li>使用每渠道变量集中管理多渠道促销宣传资料。</li>
<li>内容在网站的多个页面间重复使用。</li>
<li>网站主题(例如 页眉和页脚)</li>
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
<li><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/experience-fragments.html" target="_blank">Adobe体验片段文档</a></li>
</ul>
</td>
</tr></tbody></table>

## 内容片段架构

下图说明了AEM内容片段的总体架构

!![内容片段架构](./assets/content-fragments-architecture.png)

+ **内容片** 段模型定义元素（或字段），这些元素（或字段）定义内容片段可能捕获和显示的内容。
+ **内容片段**&#x200B;是内容片段模型的一个实例，它表示逻辑内容实体。
+ 内容片段&#x200B;**变量**&#x200B;与内容片段模型相符，但内容中有变量。
+ 内容片段可由以下人员公开／使用：
   + 通过AEM WCM核心组件的内容片段组件在&#x200B;**AEM Sites**(或AEM Screens)上使用内容片段。
   + 通过AEM WCM核心组件的内容片段组件将内容片段嵌入&#x200B;**体验片段**&#x200B;中，以用于任何体验片段用例。
   + 通过&#x200B;**AEM Content Services**&#x200B;和API页面将内容片段变体内容公开为JSON，以供只读用例使用。
   + 通过直接调用(通过&#x200B;**AEM AssetsHTTP API**)将内容片段内容（所有变体）公开为JSON，用于CRUD用例。

## 体验片段体系结构

!![体验片段体系结构](./assets/experience-fragments-architecture.png)

+ **可编辑模板**(又由可编辑模板 **类型和AEM** 页面组件实 **现定义)定义**，可定义允许的AEM组件，这些组件可用于构建体验片段。
+ **体验片段**&#x200B;是可编辑模板的一个实例，它表示逻辑体验。
+ 体验片段&#x200B;**变量**&#x200B;符合可编辑模板，但体验（内容和设计）存在差异。
+ 体验片段可以由以下人员公开／使用：
   + 通过AEM体验片段组件在AEM Sites(或AEM Screens)使用体验片段。
   + 通过&#x200B;**AEM Content Services**&#x200B;和API页面将体验片段变量内容公开为JSON（带有嵌入式HTML）。
   + 直接将体验片段变体公开为&#x200B;**“普通HTML”**。
   + 将体验片段导出到&#x200B;**Adobe Target**&#x200B;作为HTML或JSON优惠。
   + AEM Sites本机支持HTML优惠，但JSON优惠需要自定义开发。

## 内容片段的支持材料

+ [内容片段用户指南](https://helpx.adobe.com/experience-manager/6-5/assets/user-guide.html?topic=/experience-manager/6-5/assets/morehelp/content-fragments.ug.js)
+ [在AEM中使用内容片段](https://helpx.adobe.com/experience-manager/kt/sites/using/content-fragments-feature-video-use.html)
+ [AEM WCM核心组件的内容片段组件](https://docs.adobe.com/content/help/zh-Hans/experience-manager-core-components/using/components/content-fragment-component.html)
+ [使用内容片段和AEM内容服务](https://helpx.adobe.com/experience-manager/kt/sites/using/structured-fragments-content-services-feature-video-use.html)
+ [AEM Content Services入门](https://helpx.adobe.com/experience-manager/kt/sites/using/content-services-tutorial-use.html)

## 体验片段的支持材料

+ [Adobe体验片段文档](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/experience-fragments.html)
+ [了解AEM体验片段](https://helpx.adobe.com/experience-manager/kt/sites/using/experience-fragments-feature-video-understand.html)
+ [使用AEM Experience Fragments](https://helpx.adobe.com/experience-manager/kt/sites/using/experience-fragments-feature-video-use.html)
+ [将AEM Experience Fragments与Adobe Target一起使用](https://medium.com/adobetech/experience-fragments-and-adobe-target-d8d74381b9b2)
