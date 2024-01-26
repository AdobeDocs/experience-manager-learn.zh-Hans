---
title: AEM事件
description: 了解AEM事件、内容、使用原因和时机以及示例。
version: Cloud Service
feature: Developing, App Builder
topic: Development, Architecture, Content Management
role: Architect, Developer
level: Beginner
doc-type: Tutorial
duration: 573
last-substantial-update: 2023-12-07T00:00:00Z
jira: KT-14649
thumbnail: KT-14649.jpeg
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '841'
ht-degree: 0%

---


# AEM事件

了解AEM事件、内容、使用原因和时机以及示例。

>[!VIDEO](https://video.tv.adobe.com/v/3426686?quality=12&learn=on)

>[!IMPORTANT]
>
>AEMas a Cloud Service事件仅适用于预发行模式下的注册用户。 要在AEMas a Cloud Service环境中启用AEM事件，请联系 [AEM事件团队](mailto:grp-aem-events@adobe.com).

## 内容

AEM Eventing是一个云原生事件系统，支持订阅AEM事件以在外部系统中处理。 AEM事件是每当发生特定操作时AEM发送的状态更改通知。 例如，这可以包括创建、更新或删除内容片段时的事件。

![AEM事件](./assets/aem-eventing.png)

上图显示了AEMas a Cloud Service如何生成事件并将它们发送到Adobe I/O事件，事件事件订阅者进而会看到这些事件。

总之，有三个主要组成部分：

1. **事件提供程序：** AEMas a Cloud Service。
1. **Adobe I/O事件：** 用于基于Adobe的产品和技术集成、扩展和构建应用程序和体验的开发人员平台。
1. **事件消费者：** 客户拥有的订阅AEM事件的系统。 例如，CRM（客户关系管理）、PIM（产品信息管理）、OMS（订单管理系统）或自定义应用程序。

### 它有何不同

此 [Apache Sling事件](https://sling.apache.org/documentation/bundles/apache-sling-eventing-and-job-handling.html)， OSGi事件，以及 [JCR观察](https://jackrabbit.apache.org/oak/docs/features/observation.html) 所有提供订阅和处理事件的机制。 但是，这些事件不同于本文档中讨论的AEM事件。

AEM Eventing的主要区别包括：

- 事件使用者代码在AEM外部执行，而不是在与AEM相同的JVM中运行。
- AEM产品代码负责定义事件并将它们发送到Adobe I/O事件。
- 事件信息经过标准化，并以JSON格式发送。 有关更多详细信息，请参阅 [云端](https://cloudevents.io/).
- 为了与AEM进行通信，事件使用者利用AEMas a Cloud ServiceAPI。


## 使用它的原因和时间

AEM Eventing在系统架构和运营效率方面提供了众多优势。 使用AEM Eventing的主要原因包括：

- **构建事件驱动型体系结构**：促进创建可以独立扩展并抗故障且松散耦合的系统。
- **低代码和较低的运营成本**：避免了AEM中的自定义设置，使系统更易于维护和扩展，从而降低了运营支出。
- **简化AEM与外部系统之间的通信**：通过让Adobe I/O事件管理通信(例如确定哪些AEM事件应交付给特定系统或服务)，消除点对点连接。
- **提高事件持久性**：Adobe I/O事件是一个高度可用和可扩展的系统，旨在处理大量事件并可靠地将其交付给订阅者。
- **并行处理事件**：支持同时向多个订阅者交付事件，从而允许跨不同系统进行分布式事件处理。
- **无服务器应用程序开发**：支持将事件使用者代码部署为无服务器应用程序，从而进一步增强系统灵活性和可扩展性。

### 限制

AEM事件虽然强大，但存在一些需要考虑的限制：

- **可用性仅限于AEMas a Cloud Service**：目前，AEM事件专门适用于AEMas a Cloud Service。
- **有限的事件支持**：截至目前，仅支持AEM内容片段事件。 但是，预计今后会增加更多的活动，扩大活动范围。

## 如何启用

AEM事件是根据AEMas a Cloud Service环境启用的，并且仅适用于预发行模式下的环境。 联系人 [AEM事件团队](mailto:grp-aem-events@adobe.com) 以使用AEM事件启用AEM环境。

如果已启用，请参阅 [在AEM Cloud Service环境中启用AEM事件](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#enable-aem-events-on-your-aem-cloud-service-environment) 以了解后续步骤。

## 如何订阅

要订购AEM Events，您不必在AEM中编写任何代码，而只需在 [Adobe Developer控制台](https://developer.adobe.com/) 项目已配置。 Adobe Developer控制台是AdobeAPI、SDK、事件、运行时和应用程序生成器的网关。

在本例中， _项目_ 在Adobe Developer Console中，您可以订阅从AEMas a Cloud Service环境发出的事件，并配置事件发送到外部系统。

有关更多信息，请参阅 [如何在Adobe Developer Console中订阅AEM事件](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#how-to-subscribe-to-aem-events-in-the-adobe-developer-console).

## 如何消费

使用AEM事件有两种主要方法： _推送_ 方法和 _提取_ 方法。

- **推送方法**：在此方法中，事件使用者会在事件可用时主动收到Adobe I/O事件的通知。 集成选项包括Webhooks、Adobe I/O Runtime和Amazon EventBridge。
- **拉方法**：在这里，事件使用者主动轮询Adobe I/O事件以检查新事件。 此方法的主要集成选项是Adobe I/O日记API。

有关更多信息，请参阅 [通过Adobe I/O事件处理的AEM事件](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#aem-events-processing-via-adobe-io).

## 示例

<table>
  <tr>
    <td>
        <a  href="./examples/webhook.md"><img alt="在webhook上接收AEM事件" src="./assets/examples/webhook/Eventing-webhook.png"/></a>
        <div><strong><a href="./examples/webhook.md">在webhook上接收AEM事件</a></strong></div>
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
</table>
