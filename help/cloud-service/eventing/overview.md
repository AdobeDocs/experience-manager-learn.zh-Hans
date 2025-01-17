---
title: AEM事件
description: 了解AEM事件、内容、使用原因和时机以及示例。
version: Cloud Service
feature: Developing, App Builder
topic: Development, Architecture, Content Management
role: Architect, Developer
level: Beginner
doc-type: Tutorial
duration: 540
last-substantial-update: 2023-12-07T00:00:00Z
jira: KT-14649
thumbnail: KT-14649.jpeg
exl-id: 142ed6ae-1659-4849-80a3-50132b2f1a86
source-git-commit: 99aa43460a76460175123a5bfe5138767491252b
workflow-type: tm+mt
source-wordcount: '860'
ht-degree: 0%

---

# AEM事件

了解AEM事件、内容、使用原因和时机以及示例。

>[!VIDEO](https://video.tv.adobe.com/v/3426686?quality=12&learn=on)

## 内容

AEM Eventing是一个云原生事件系统，支持订阅AEM事件以在外部系统中处理。 AEM事件是每当发生特定操作时AEM发送的状态更改通知。 例如，这可以包括创建、更新或删除内容片段时的事件。

![AEM事件](./assets/aem-eventing.png)

上图显示了AEM as a Cloud Service如何生成事件并将它们发送到Adobe I/O事件，事件事件订阅者进而会看到这些事件。

总之，有三个主要组成部分：

1. **事件提供程序：** AEM as a Cloud Service。
1. **Adobe I/O活动：**&#x200B;用于集成、扩展和构建基于Adobe产品和技术的应用程序和体验的开发人员平台。
1. **事件使用者：**&#x200B;由订阅AEM事件的客户拥有的系统。 例如，CRM （客户关系管理）、PIM （产品信息管理）、OMS (Order Management System)或自定义应用程序。

### 它有何不同

[Apache Sling事件](https://sling.apache.org/documentation/bundles/apache-sling-eventing-and-job-handling.html)、OSGi事件和[JCR观察](https://jackrabbit.apache.org/oak/docs/features/observation.html)所有用于订阅和处理事件的提供机制。 但是，这些事件不同于本文档中讨论的AEM事件。

AEM Eventing的主要区别包括：

- 事件使用者代码在AEM外部执行，而不是在与AEM相同的JVM中运行。
- AEM产品代码负责定义事件并将它们发送到Adobe I/O事件。
- 事件信息经过标准化，并以JSON格式发送。 有关详细信息，请参阅[cloudevents](https://cloudevents.io/)。
- 为了与AEM进行通信，事件消费者使用AEM as a Cloud Service API。


## 使用它的原因和时间

AEM Eventing在系统架构和运营效率方面提供了众多优势。 使用AEM Eventing的主要原因包括：

- **构建事件驱动型体系结构**：有助于创建可独立扩展且能够抵御故障的松耦合系统。
- **代码低且运营成本较低**：避免了AEM中的自定义设置，使系统更容易维护和扩展，从而降低运营成本。
- **简化AEM与外部系统之间的通信**：通过让Adobe I/O事件管理通信(如确定哪些AEM事件应传送到特定系统或服务)，消除点对点连接。
- **事件的持久性更高**：Adobe I/O事件是一个高度可用且可扩展的系统，旨在处理大量事件并将它们可靠地交付给订阅者。
- **并行处理事件**：允许同时向多个订阅者交付事件，从而允许跨不同系统执行分布式事件处理。
- **无服务器应用程序开发**：支持将事件使用者代码部署为无服务器应用程序，从而进一步提高系统灵活性和可扩展性。

### 限制

AEM事件虽然强大，但存在一些需要考虑的限制：

- **可用性限制为AEM as a Cloud Service**：当前，AEM Eventing仅可用于AEM as a Cloud Service。

- **可用的事件类型**：在[此处](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#available-event-types)查看当前可用的事件类型列表。

## 如何启用

有关后续步骤，请参阅[在AEM Cloud Service环境中启用AEM事件](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#enable-aem-events-on-your-aem-cloud-service-environment)。

## 如何订阅

要订阅AEM Events，您不必在AEM中编写任何代码，而是配置了[Adobe Developer Console](https://developer.adobe.com/)项目。 Adobe Developer Console是AdobeAPI、SDK、事件、运行时和App Builder的网关。

在这种情况下，通过Adobe Developer Console中的&#x200B;_项目_，您可以订阅从AEM as a Cloud Service环境发出的事件，并配置事件传递到外部系统。

有关详细信息，请参阅[如何在Adobe Developer Console中订阅AEM事件](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#how-to-subscribe-to-aem-events-in-the-adobe-developer-console)。

## 如何消费

使用AEM事件有两种主要方法：_推送_&#x200B;方法和&#x200B;_拉取_&#x200B;方法。

- **推送方法**：在此方法中，事件使用者会在事件可用时主动收到Adobe I/O事件的通知。 集成选项包括Webhooks、Adobe I/O Runtime和Amazon EventBridge。
- **Pull方法**：在此处，事件使用者主动轮询Adobe I/O事件以检查新事件。 此方法的主要集成选项是Adobe Developer日记API。

有关详细信息，请参阅[通过Adobe I/O事件处理的AEM事件](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#aem-events-processing-via-adobe-io)。

## 示例

<table>
  <tr>
    <td>
        <a  href="./examples/webhook.md"><img alt="在webhook上接收AEM事件" src="./assets/examples/webhook/webhook-example.png"/></a>
        <div><strong><a href="./examples/webhook.md">在webhook上接收AEM活动</a></strong></div>
        <p>
          使用Adobe提供的webhook接收AEM事件并查看事件详细信息。
        </p>
      </td>
      <td>
        <a  href="./examples/journaling.md"><img alt="加载AEM事件日志" src="./assets/examples/journaling/eventing-journal.png"/></a>
        <div><strong><a href="./examples/journaling.md">加载AEM事件日志</a></strong></div>
        <p>
          使用Adobe提供的Web应用程序从日记账中加载AEM Events并查看事件详细信息。
        </p>
      </td>
    </tr>
  <tr>
    <td>
        <a  href="./examples/runtime-action.md"><img alt="在Adobe I/O Runtime操作中接收AEM事件" src="./assets/examples/runtime-action/eventing-runtime.png"/></a>
        <div><strong><a href="./examples/runtime-action.md">接收Adobe I/O Runtime操作的AEM事件</a></strong></div>
        <p>
          接收AEM事件并查看事件详细信息。
        </p>
      </td>
      <td>
        <a  href="./examples/event-processing-using-runtime-action.md"><img alt="使用Adobe I/O Runtime操作处理AEM事件" src="./assets/examples/event-processing-using-runtime-action/event-processing.png"/></a>
        <div>使用Adobe I/O Runtime操作处理的<strong><a href="./examples/event-processing-using-runtime-action.md">AEM事件</a></strong></div>
        <p>
          了解如何使用Adobe I/O Runtime操作处理收到的AEM事件。 事件处理包括AEM回调、事件数据持久性并在SPA中显示它们。
        </p>
      </td>
  </tr>
  <tr>
    <td>
        <a  href="./examples/assets-pim-integration.md"><img alt="用于PIM集成的AEM Assets事件" src="./assets/examples/assets-pim-integration/PIM-integration-tile.png"/></a>
        <div>用于PIM集成的<strong><a href="./examples/assets-pim-integration.md">AEM Assets事件</a></strong></div>
        <p>
          了解如何集成AEM Assets和产品信息管理(PIM)系统以进行元数据更新。
        </p>
      </td>
  </tr> 
</table>
