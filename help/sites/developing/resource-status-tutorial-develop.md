---
title: 发展AEM Sites的资源地位
description: 'Adobe Experience Manager的资源状态API是用于在AEM各种编辑器Web UI中显示状态消息的可插拔框架。 '
topics: development
audience: developer
doc-type: tutorial
activity: develop
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 03db12de4d95ced8fabf36b8dc328581ec7a2749
workflow-type: tm+mt
source-wordcount: '446'
ht-degree: 2%

---


# 开发资源状态 {#developing-resource-statuses-in-aem-sites}

Adobe Experience Manager的资源状态API是用于在AEM各种编辑器Web UI中显示状态消息的可插拔框架。

## 概述 {#overview}

编辑器资源状态框架以标准和统一的方式提供服务器端和客户端API，用于显示和与编辑器状态交互。

AEM的页面、体验片段和模板编辑器中本机提供编辑器状态栏。

自定义资源状态提供者的示例用例包括：

* 在页面在计划激活的2小时内通知作者
* 通知作者页面在过去15分钟内已激活
* 通知作者页面在过去5分钟内进行了编辑，以及编辑者

![AEM editor资源状态概述](assets/sample-editor-resource-status-screenshot.png)

## 资源状态提供程序框架 {#resource-status-provider-framework}

在开发自定义资源状态时，开发工作包括：

1. ResourceStatusProvider实现，它负责确定是否需要状态，以及有关状态的基本信息：标题、消息、优先级、变体、图标和可用操作。
2. （可选）实现任何可用操作功能的GraniteUI JavaScript。

   ![资源状态体系](assets/sample-editor-resource-status-application-architecture.png)

3. 作为页面、体验片段和模板编辑器的一部分提供的状态资源通过资源“”属性赋[!DNL statusType]予类型。

   * Page editor: `editor`
   * Experience Fragment editor: `editor`
   * 模板编辑器: `template-editor`

4. 状态资源与已注 `statusType` 册的OSGi配置 `CompositeStatusType` 属性 `name` 匹配。

   对于所有匹配项 `CompositeStatusType's` ，将通过收集类型并用 `ResourceStatusProvider` 于收集具有此类型的实现 `ResourceStatusProvider.getType()`。

5. 匹配项 `ResourceStatusProvider` 在编辑 `resource` 器中传递，并确定是否 `resource` 具有要显示的状态。 如果需要状态，则此实现负责生成0个或多个要返回 `ResourceStatuses` 的组件，每个组件表示要显示的状态。

   通常， `ResourceStatusProvider` 每个返回0 `ResourceStatus` 或1 `resource`。

6. ResourceStatus是可由客户实现的接口，或者有帮助 `com.day.cq.wcm.commons.status.EditorResourceStatus.Builder` 的可用于构建状态。 状态包括：

   * 标题
   * 消息
   * 图标
   * 变量
   * 优先级
   * 操作
   * 数据

7. 或者，如 `Actions` 果为对象提 `ResourceStatus` 供，则需要支持客户端将功能绑定到状态栏中的操作链接。

   ```js
   (function(jQuery, document) {
       'use strict';
   
       $(document).on('click', '.editor-StatusBar-action[data-status-action-id="do-something"]', function () {
           // Do something on the click of the resource status action
   
       });
   })(jQuery, document);
   ```

8. 任何支持这些操作的JavaScript或CSS都必须通过每个编辑器各自的客户端库进行代理，以确保前端代码在编辑器中可用。

   * 页面编辑器类别: `cq.authoring.editor.sites.page`
   * 体验片段编辑器类别: `cq.authoring.editor.sites.page`
   * 模板编辑器类别: `cq.authoring.editor.sites.template`

## 视图代码 {#view-the-code}

[查看GitHub上的代码](https://github.com/Adobe-Consulting-Services/acs-aem-samples/tree/master/bundle/src/main/java/com/adobe/acs/samples/resourcestatus/impl/SampleEditorResourceStatusProvider.java)

## 其他资源 {#additional-resources}

* [`com.adobe.granite.resourcestatus` JavaDocs](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/resourcestatus/package-summary.html)
* [`com.day.cq.wcm.commons.status.EditorResourceStatus` JavaDocs](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/day/cq/wcm/commons/status/EditorResourceStatus.html)
