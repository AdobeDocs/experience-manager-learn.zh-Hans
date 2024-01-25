---
title: Webhook和AEM活动
description: 了解如何在webhook上接收AEM事件并查看事件详细信息，例如有效负载、标头和元数据。
version: Cloud Service
feature: Developing, App Builder
topic: Development, Architecture, Content Management
role: Architect, Developer
level: Beginner
doc-type: Tutorial
duration: 152
last-substantial-update: 2023-12-07T00:00:00Z
jira: KT-14732
thumbnail: KT-14732.jpeg
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '520'
ht-degree: 0%

---


# Webhook和AEM活动

了解如何在webhook上接收AEM事件并查看事件详细信息，例如有效负载、标头和元数据。

在此示例中，利用Adobe提供的 _托管webhook_ 允许您接收AEM事件，而无需设置自己的webhook。 此Adobe提供的webhook托管在 [故障](https://glitch.com/)，该平台以提供基于Web的环境而闻名，有助于构建和部署Web应用程序。 但是，如果愿意，也可以使用您自己的webhook选项。

## 前提条件

要完成本教程，您需要：

- AEMas a Cloud Service环境与 [已启用AEM事件](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#enable-aem-events-on-your-aem-cloud-service-environment).

- [为AEM事件配置的Adobe Developer控制台项目](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#how-to-subscribe-to-aem-events-in-the-adobe-developer-console).

## 访问webhook

要访问Adobe提供的webhook，请执行以下步骤：

- 验证您是否可访问 [问题 — 托管的webhook](https://lovely-ancient-coaster.glitch.me/) 在新的浏览器选项卡中。

  ![问题 — 托管的webhook](../assets/examples/webhook/glitch-hosted-webhook.png)

- 输入webhook的唯一名称，例如 `<YOUR_PETS_NAME>-aem-eventing` 并单击 **连接**. 您应该看到 `Connected to: ${YOUR-WEBHOOK-URL}` 屏幕上显示的消息。

  ![问题 — 创建webhook](../assets/examples/webhook/glitch-create-webhook.png)

- 记下 **Webhook URL**. 在本教程的后面部分，您需要使用该功能。

## 在Adobe Developer控制台项目中配置webhook

要在上述webhook URL上接收AEM事件，请执行以下步骤：

- 在 [Adobe Developer控制台](https://developer.adobe.com)，导航到您的项目，然后单击以将其打开。

- 下 **产品和服务** 部分，单击省略号 `...` ，位于应将AEM事件发送到webhook的所需事件卡片旁边，然后选择 **编辑**.

  ![Adobe Developer控制台项目编辑](../assets/examples/webhook/adobe-developer-console-project-edit.png)

- 在新打开的 **配置事件注册** 对话框，请单击 **下一个** 以继续访问 **如何接收事件** 步骤。

  ![Adobe Developer控制台项目配置](../assets/examples/webhook/adobe-developer-console-project-configure.png)

- 在 **如何接收事件** 步骤，选择 **Webhook** 选项并粘贴 **Webhook URL** 您之前从Glitch托管的webhook中复制了数据，然后单击 **保存配置的事件**.

  ![Adobe Developer控制台项目Webhook](../assets/examples/webhook/adobe-developer-console-project-webhook.png)

- 在Glitch Webook页面中，您应该会看到GET请求，它是Adobe I/O事件发送的质询请求，用于验证webhook URL。

  ![问题 — 质询请求](../assets/examples/webhook/glitch-challenge-request.png)


## 触发AEM事件

要通过已在上述AEM Console项目中注册的AEMas a Cloud Service环境触发Adobe Developer事件，请执行以下步骤：

- 通过以下方式访问和登录您的AEMas a Cloud Service创作环境 [Cloud Manager](https://my.cloudmanager.adobe.com/).

- 根据您的 **订阅的事件**、创建、更新、删除、发布或取消发布内容片段。

## 查看事件详细信息

完成上述步骤后，您应该会看到正在交付到webhook的AEM事件。 在Glitch webhook页面中查找POST请求。

![问题 — POST请求](../assets/examples/webhook/glitch-post-request.png)

以下是POST请求的关键详细信息：

- 路径： `/webhook/${YOUR-WEBHOOK-URL}`例如 `/webhook/AdobeTM-aem-eventing`

- 标头：由Adobe I/O事件发送的请求标头，例如：

```json
{
"connection": "close",
"x-forwarded-for": "34.205.178.127,::ffff:10.10.10.136,::ffff:10.10.84.114",
"x-forwarded-proto": "https,http,http",
"x-forwarded-port": "443,80,80",
"host": "lovely-ancient-coaster.glitch.me",
"content-length": "826",
"x-adobe-public-key2-path": "/prod/keys/pub-key-IkpzhSpTw0.pem",
"x-adobe-delivery-id": "18abfb47-d24a-4684-ade8-f442a3444033",
"x-adobe-provider": "aemsites_7ABB3E6A5A7491460A495D61@AdobeOrg_acct-aem-p46652-e1074060@adobe.com",
"x-adobe-public-key1-path": "/prod/keys/pub-key-Ptc2pD9vT9.pem",
"x-adobe-event-id": "a0f3fb7d-b02c-4612-aac6-e472b80af793",
"x-adobe-event-code": "aem.sites.contentFragment.modified",
"user-agent": "Adobe/1.0",
"x-adobe-digital-signature-2": "zGLso15+6PV6X6763/x6WqgxDlEXpkv5ty8q4njaq3aUngAI9VCcYonbScEjljRluzjZ05uMJmRfNxwjj60syxEJPuc0dpmMU635gfna7I4T7IaHs496wx4m2E5mvCM+aKbNQ+NPOutyTqI8Ovq29P2P87GIgMlGhAtOaxRVGNc6ksBxc2tCWbrKUhW8hPJ0sHphU499dN4TT32xrZaiRw4akT3M/hYydsA8dcWpJ7S4dpuDS21YyDHAB8s9Dawtr3fyPEyLgZzpwZDfCqQ8gdSCGqKscE4pScwqPkKOYCHDnBvDZVe583jhcZbHGjk7Ncp/FrgQk7avWsk5XlzcuA==",
"x-adobe-digital-signature-1": "QD7THFJ1vmJqD/BatIpzO6+ACQ9cSKPR7XVaW0LI7cN/xs7ucyri6dmkerOPe9EJpjGoqCg8rxWedrIRQB3lgVskChbHH3Ujx5YG0aTQLSd1Lsn5CFbW1U0l0GqId9Cnd6MccrqSznZXcdW1rMFuRk8+gqwabBifSaLbu3r30G5hmqQd72VtiYTE4m23O3jYIMiv62pRP+a+p4NjNj1XG320uRSry+BPniTjDJ6oN/Ng7aUEKML8idZ/ZTqeh/rJSrVO95UryUolFDRwDkRn5zKonbvhSLAeXzaPhvimWUHtldq9M1WTyRMpsBk8BRzaklxlq+woJ2UjYPUIEzjotw==",
"accept-encoding": "deflate,compress,identity",
"content-type": "application/cloudevents+json; charset=UTF-8",
"x-forwarded-host": "lovely-ancient-coaster.glitch.me",
"traceparent": "00-c27558588d994f169186ca6a3c6607d4-a7e7ee36625488d4-01"
}
```

- body/payload：由Adobe I/O事件发送的请求正文，例如：

```json
{
  "specversion": "1.0",
  "type": "aem.sites.contentFragment.modified",
  "source": "acct:aem-p46652-e1074060@adobe.com",
  "id": "bf922a49-9db4-4377-baf4-70e96e15c45f",
  "time": "2023-12-12T20:36:43.583228Z",
  "dataschema": "https://ns.adobe.com/xdm/aem/sites/events/content-fragment-modified.json",
  "datacontenttype": "application/json",
  "data": {
    "user": {
      "imsUserId": "933E1F8A631CAA0F0A495E53@80761f6e631c0c7d495fb3.e",
      "principalId": "xxx@adobe.com",
      "displayName": "First LastName",
    },
    "path": "/content/dam/wknd-shared/en/adventures/beervana-portland/beervana-in-portland",
    "model": {
      "id": "/conf/wknd-shared/settings/dam/cfm/models/adventure"
    },
    "id": "9a2d3e6a-efda-4079-a86e-0ef2ede692da",
    "properties": [
      {
        "name": "groupSize",
        "changeType": "modified"
      }
    ]
  },
  "event_id": "a0f3fb7d-b02c-4612-aac6-e472b80af793",
  "recipient_client_id": "f51ea733ba404db299fefbf285dc1c42"
}
```

您可以看到，AEM事件详细信息中提供了在webhook中处理该事件所需的所有信息。 例如，事件类型(`type`)，事件源(`source`)，事件ID (`event_id`)，事件时间(`time`)和事件数据(`data`)。

## 其他资源

- [Glitch webhook源代码](https://glitch.com/edit/#!/lovely-ancient-coaster) 可供参考。
