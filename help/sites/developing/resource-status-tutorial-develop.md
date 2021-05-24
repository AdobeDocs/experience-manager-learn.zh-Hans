---
title: 在AEM Sites开发资源状态
description: 'Adobe Experience Manager的资源状态API是一个可插拔的框架，用于在AEM各种编辑器Web UI中公开状态消息。 '
topics: development
audience: developer
doc-type: tutorial
activity: develop
version: 6.3, 6.4, 6.5
source-git-commit: 03db12de4d95ced8fabf36b8dc328581ec7a2749
workflow-type: tm+mt
source-wordcount: '446'
ht-degree: 2%

---


# 开发资源状态{#developing-resource-statuses-in-aem-sites}

Adobe Experience Manager的资源状态API是一个可插拔的框架，用于在AEM各种编辑器Web UI中公开状态消息。

## 概述 {#overview}

编辑器资源状态框架提供了服务器端和客户端API，用于以标准和统一的方式显示和与编辑器状态交互。

编辑器状态栏在AEM的页面、体验片段和模板编辑器中本地可用。

自定义资源状态提供程序的示例用例包括：

* 在计划激活后2小时内通知作者页面
* 通知作者页面在过去15分钟内被激活
* 通知作者页面在过去5分钟内进行了编辑，并由谁进行编辑

![AEM编辑器资源状态概述](assets/sample-editor-resource-status-screenshot.png)

## 资源状态提供程序框架{#resource-status-provider-framework}

开发自定义资源状态时，开发工作包括：

1. ResourceStatusProvider实施，负责确定是否需要状态，以及有关状态的基本信息：标题、消息、优先级、变体、图标和可用操作。
2. （可选）用于实施任何可用操作功能的GraniteUI JavaScript。

   ![资源状态架构](assets/sample-editor-resource-status-application-architecture.png)

3. 作为页面、体验片段和模板编辑器的一部分提供的状态资源通过资源“[!DNL statusType]”属性来提供一个类型。

   * 页面编辑器：`editor`
   * 体验片段编辑器：`editor`
   * 模板编辑器: `template-editor`

4. 状态资源的`statusType`与已注册的`CompositeStatusType` OSGi配置的`name`属性匹配。

   对于所有匹配项，将收集`CompositeStatusType's`类型，并通过`ResourceStatusProvider.getType()`用于收集具有此类型的`ResourceStatusProvider`实施。

5. 匹配的`ResourceStatusProvider`在编辑器中传递到`resource`，并确定`resource`是否具有要显示的状态。 如果需要状态，此实施将负责生成0个或多个要返回的`ResourceStatuses`，每个都表示要显示的状态。

   通常， `ResourceStatusProvider`会为每个`resource`返回0或1个`ResourceStatus`。

6. ResourceStatus是可由客户实现的接口，或者有用的`com.day.cq.wcm.commons.status.EditorResourceStatus.Builder`可用于构建状态。 状态包括：

   * 标题
   * 消息
   * 图标
   * 变量
   * 优先级
   * 操作
   * 数据

7. 或者，如果为`ResourceStatus`对象提供了`Actions` ，则需要支持的clientlibs将功能绑定到状态栏中的操作链接。

   ```js
   (function(jQuery, document) {
       'use strict';
   
       $(document).on('click', '.editor-StatusBar-action[data-status-action-id="do-something"]', function () {
           // Do something on the click of the resource status action
   
       });
   })(jQuery, document);
   ```

8. 任何支持JavaScript或CSS的支持操作都必须通过每个编辑器各自的客户端库来代理，以确保前端代码在编辑器中可用。

   * 页面编辑器类别：`cq.authoring.editor.sites.page`
   * 体验片段编辑器类别：`cq.authoring.editor.sites.page`
   * 模板编辑器类别：`cq.authoring.editor.sites.template`

## 查看代码{#view-the-code}

[查看GitHub上的代码](https://github.com/Adobe-Consulting-Services/acs-aem-samples/tree/master/bundle/src/main/java/com/adobe/acs/samples/resourcestatus/impl/SampleEditorResourceStatusProvider.java)

## 其他资源 {#additional-resources}

* [`com.adobe.granite.resourcestatus` JavaDocs](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/resourcestatus/package-summary.html)
* [`com.day.cq.wcm.commons.status.EditorResourceStatus` JavaDocs](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/day/cq/wcm/commons/status/EditorResourceStatus.html)
