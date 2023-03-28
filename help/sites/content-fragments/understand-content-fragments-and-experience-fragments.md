---
title: 内容片段和体验片段
description: 了解内容片段和体验片段之间的相似之处和差异，以及使用每种类型的时间和方式。
sub-product: Experience Manager Assets, Experience Manager Sites
feature: Content Fragments, Experience Fragments
topics: headless
version: 6.4, 6.5
doc-type: article
activity: understand
audience: all
topic: Content Management
role: User
level: Beginner
exl-id: ccbc68d1-a83e-4092-9a49-53c56c14483e
source-git-commit: 84fdbaa173a929ae7467aecd031cacc4ce73538a
workflow-type: tm+mt
source-wordcount: '1044'
ht-degree: 5%

---

# 内容片段和体验片段

Adobe Experience Manager的内容片段和体验片段在表面上看似相似，但在不同的用例中，每个片段都扮演着关键角色。 了解内容片段和体验片段如何相似、不同，以及何时以及如何使用它们。

## 比较

<table>
<tbody><tr><td><strong> </strong></td>
<td><strong>内容片段(CF)</strong></td>
<td><strong>体验片段(XF)</strong></td>
</tr><tr><td><strong>定义</strong></td>
<td><ul>
<li>可重复使用，与演示无关 <strong>内容</strong>，由结构化数据元素（文本、日期、引用等）组成</li>
</ul>
</td>
<td><ul>
<li>一个或多个AEM组件的可重用组合，这些组件定义了 <strong>体验</strong> 它本身就有意义</li>
</ul>
</td>
</tr><tr><td><strong>核心租户</strong></td>
<td><ul>
<li>以内容为中心</li>
<li>由 <a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-models.html?lang=en" target="_blank">结构化、基于表单的数据模型。</a></li>
<li>与设计和布局无关。</li>
<li>渠道拥有内容片段内容的表示形式（布局和设计）</li>
</ul>
</td>
<td><ul>
<li>以演示为中心</li>
<li>由AEM组件的非结构化组合定义</li>
<li>定义内容的设计和布局</li>
<li>在渠道中按“原样”使用</li>
</ul>
</td>
</tr><tr><td><strong>技术详细信息</strong></td>
<td><ul>
<li>实施为 <strong>dam:Asset</strong></li>
<li>由 <a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-models.html?lang=en" target="_blank">内容片段模型</a></li>
</ul>
</td>
<td><ul>
<li>实施为 <strong>cq:Page</strong></li>
<li>由可编辑的模板定义</li>
<li>本机HTML呈现</li>
</ul>
</td>
</tr><tr><td><strong>变体</strong></td>
<td><ul>
<li>主控变分是正则变分</li>
<li>变体是特定于用例的，它们可能与渠道保持一致。</li>
</ul>
</td>
<td><ul>
<li>变体是特定于渠道或上下文的</li>
<li>变量通过AEM Live Copy保持同步</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html" target="_blank">构建基块</a> 允许在不同变量中重复使用内容</li>
</ul>
</td>
</tr><tr><td><strong>功能</strong></td>
<td><ul>
<li>变体</li>
<li>版本</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-variations.html?lang=en#synchronizing-with-master" target="_blank">同步</a> 各个变量中的内容</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-managing.html?lang=en#comparing-fragment-versions" target="_blank">可视化差异</a> 内容片段版本的</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-variations.html?lang=en#annotating-a-content-fragment" target="_blank">批注</a> 多行文本元素的</li>
<li>智能 <a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-variations.html?lang=en#summarizing-text" target="_blank">总结</a> 多行文本元素。</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/creating-translation-projects-for-content-fragments.html?lang=en" target="_blank">翻译/本地化</a></li>
</ul>
</td>
<td><ul>
<li>变体</li>
<li>作为Live Copy的变体</li>
<li>版本</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html?lang=en#building-blocks" target="_blank">构建基块</a></li>
<li>注释</li>
<li>响应式布局和预览</li>
<li>翻译/本地化</li>
<li>通过内容片段引用的复杂数据模型</li>
<li>应用程序内预览</li>
</ul>
</td>
</tr><tr><td><strong>使用</strong></td>
<td><ul>
<li>通过导出JSON <a href="https://experienceleague.adobe.com/landing/experience-manager/headless/developer.html?lang=zh-Hans">AEM Headless GraphQL API</a></li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html?lang=zh-Hans" target="_blank">AEM核心组件内容片段组件</a> ，以在AEM Sites、AEM Screens或体验片段中使用。</li>
<li>通过导出JSON <a href="https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/content-services/overview.html?lang=en" target="_blank">AEM Content Services</a> 用于第三方消费</li>
<li>将JSON导出到Adobe Target以获取目标选件</li>
<li>通过AEM HTTP Assets API进行JSON以用于第三方使用</li>
</ul>
</td>
<td><ul>
<li>AEM体验片段组件，用于AEM Sites、AEM Screens或其他体验片段。</li>
<li>导出为 <a href="https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html?lang=en" target="_blank">纯HTML</a> 供第三方系统使用</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/administering/integration/experience-fragments-target.html?lang=zh-Hans" target="_blank">HTML导出到Adobe Target</a> 针对定位选件</li>
<li>将JSON导出到Adobe Target以获取目标选件</li>
</ul>
</td>
</tr><tr><td><strong>常见用例</strong></td>
<td><ul>
<li>通过GraphQL提供无头用例</li>
<li>结构化数据输入/基于表单的内容</li>
<li>长格式编辑内容（多行元素）</li>
<li>在提供内容的渠道的生命周期之外管理的内容</li>
</ul>
</td>
<td><ul>
<li>使用每个渠道变量对多渠道促销宣传资料进行集中管理。</li>
<li>在网站的多个页面中重复使用的内容。</li>
<li>网站Chrome(例如 页眉和页脚)</li>
<li>在提供该体验的渠道的生命周期之外管理的体验</li>
</ul>
</td>
</tr><tr><td><strong>文档</strong></td>
<td><ul>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/home.html?lang=en&amp;topic=/experience-manager/6-5/assets/morehelp/content-fragments.ug.js" target="_blank">AEM内容片段用户指南</a></li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-learn/sites/content-fragments/content-fragments-feature-video-use.html?lang=en" target="_blank">在AEM中使用内容片段</a></li>
</ul>
</td>
<td><ul>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html?lang=en" target="_blank">Adobe体验片段文档</a></li>
</ul>
</td>
</tr></tbody></table>

## 内容片段架构

下图说明了AEM内容片段的整体架构

![内容片段架构](./assets/content-fragments-architecture.png)

+ **内容片段模型** 定义用于定义内容片段可捕获和显示的内容的元素（或字段）。
+ 的 **内容片段** 是表示逻辑内容实体的内容片段模型的实例。
+ 内容片段 **变量** 但是，内容片段模型中存在变量。
+ 内容片段可由以下人员公开/使用：
   + 在上使用内容片段 **AEM Sites** (或AEM Screens)。
   + 消费 **内容片段** 从使用AEM Headless GraphQL API的无头应用程序。
   + 通过将内容片段变量内容公布为JSON **AEM Content Services** 和API页面。
   + 通过直接调用（通过）直接将内容片段内容（所有变量）公布为JSON **AEM Assets HTTP API** 用于CRUD用例。

## 体验片段架构

![体验片段架构](./assets/experience-fragments-architecture.png)

+ **可编辑的模板**，而定义者又为 **可编辑的模板类型** 和 **AEM页面组件实施**，定义可用于撰写体验片段的允许的AEM组件。
+ 的 **体验片段** 是可编辑模板的一个实例，表示逻辑体验。
+ 体验片段 **变量** 但是，应当遵循可编辑模板，该模板在体验（内容和设计）方面有所不同。
+ 体验片段可由以下人员公开/使用：
   + 在AEM Sites(或AEM Screens)上通过AEM体验片段组件使用体验片段。
   + 通过将体验片段变量内容公布为JSON(具有嵌入的HTML) **AEM Content Services** 和API页面。
   + 直接将体验片段变量公布为 **&quot;纯HTML&quot;**.
   + 将体验片段导出到 **Adobe Target** 作为HTML或JSON选件。
   + AEM Sites本身支持HTML选件，但是，JSON选件需要自定义开发。

## 内容片段的支持资源

+ [内容片段用户指南](https://experienceleague.adobe.com/docs/experience-manager-65/assets/home.html?lang=en&amp;topic=/experience-manager/6-5/assets/morehelp/content-fragments.ug.js)
+ [Adobe Experience Manager as a Headless CMS 简介](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/introduction.html?lang=zh-Hans)
+ [在AEM中使用内容片段](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/content-fragments/content-fragments-feature-video-use.html?lang=en)
+ [AEM WCM核心组件的内容片段组件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html?lang=zh-Hans)
+ [使用内容片段和AEM Headless](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/overview.html?lang=en)
+ [AEM Content Services快速入门](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/content-services/overview.html?lang=en)

## 体验片段的支持资源

+ [Adobe体验片段文档](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html?lang=en)
+ [了解AEM体验片段](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html?lang=en)
+ [使用AEM体验片段](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html?lang=en)
+ [将AEM体验片段与Adobe Target结合使用](https://medium.com/adobetech/experience-fragments-and-adobe-target-d8d74381b9b2)
