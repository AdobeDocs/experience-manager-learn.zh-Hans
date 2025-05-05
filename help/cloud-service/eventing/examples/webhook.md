---
title: Webhook和AEM活动
description: 了解如何在webhook上接收AEM事件并查看事件详细信息，例如有效负载、标头和元数据。
version: Experience Manager as a Cloud Service
feature: Developing, App Builder
topic: Development, Architecture, Content Management
role: Architect, Developer
level: Beginner
doc-type: Tutorial
duration: 358
last-substantial-update: 2023-01-29T00:00:00Z
jira: KT-14732
thumbnail: KT-14732.jpeg
exl-id: 00954d74-c4c7-4dac-8d23-7140c49ae31f
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '520'
ht-degree: 0%

---

# Webhook和AEM活动

了解如何在webhook上接收AEM事件并查看事件详细信息，例如有效负载、标头和元数据。

>[!VIDEO](https://video.tv.adobe.com/v/3449758?quality=12&learn=on&captions=chi_hans)

在此示例中，利用Adobe提供的&#x200B;_托管的webhook_，您无需设置自己的webhook即可接收AEM事件。 此Adobe提供的webhook托管在[Glitch](https://glitch.com/)上，这是一个众所周知的平台，它提供了有助于构建和部署Web应用程序的基于Web的环境。 但是，如果愿意，也可以使用您自己的webhook选项。

## 先决条件

要完成本教程，您需要：

- 已启用[AEM事件的AEM as a Cloud Service环境](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#enable-aem-events-on-your-aem-cloud-service-environment)。

- 已为Adobe Developer Console事件配置[AEM项目](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#how-to-subscribe-to-aem-events-in-the-adobe-developer-console)。


## 访问webhook

要访问Adobe提供的webhook，请执行以下步骤：

- 验证您是否可以在新的浏览器选项卡中访问[问题 — 托管的webhook](https://lovely-ancient-coaster.glitch.me/)。

  ![问题 — 托管的webhook](../assets/examples/webhook/glitch-hosted-webhook.png)

- 为webhook输入唯一的名称，例如`<YOUR_PETS_NAME>-aem-eventing`，然后单击&#x200B;**连接**。 您应该会看到屏幕上显示`Connected to: ${YOUR-WEBHOOK-URL}`消息。

  ![问题 — 创建webhook](../assets/examples/webhook/glitch-create-webhook.png)

- 记下&#x200B;**Webhook URL**。 在本教程的后面部分，您需要使用该功能。

## 在Adobe Developer Console项目中配置webhook

要在上述webhook URL上接收AEM活动，请执行以下步骤：

- 在[Adobe Developer Console](https://developer.adobe.com)中，导航到您的项目并单击以将其打开。

- 在&#x200B;**产品和服务**&#x200B;部分下，单击应将AEM活动发送到webhook的所需活动信息卡旁边的省略号`...`，然后选择&#x200B;**编辑**。

  ![Adobe Developer Console项目编辑](../assets/examples/webhook/adobe-developer-console-project-edit.png)

- 在新打开的&#x200B;**配置事件注册**&#x200B;对话框中，单击&#x200B;**下一步**&#x200B;以继续执行&#x200B;**如何接收事件**&#x200B;步骤。

  ![Adobe Developer Console项目配置](../assets/examples/webhook/adobe-developer-console-project-configure.png)

- 在&#x200B;**如何接收事件**&#x200B;步骤中，选择&#x200B;**Webhook**&#x200B;选项并粘贴您之前从Glitch托管的webhook复制的&#x200B;**Webhook URL**，然后单击&#x200B;**保存配置的事件**。

  ![Adobe Developer Console项目Webhook](../assets/examples/webhook/adobe-developer-console-project-webhook.png)

- 在Glitch工作簿页面中，您应该看到GET请求，它是Adobe I/O Events发送的验证webhook URL的质询请求。

  ![问题 — 质询请求](../assets/examples/webhook/glitch-challenge-request.png)


## 触发AEM事件

要通过已在上述AEM项目中注册的AEM as a Cloud Service环境触发Adobe Developer Console事件，请执行以下步骤：

- 通过[Cloud Manager](https://my.cloudmanager.adobe.com/)访问并登录AEM as a Cloud Service创作环境。

- 根据您的&#x200B;**订阅事件**，创建、更新、删除、发布或取消发布内容片段。

## 查看事件详细信息

完成上述步骤后，您应该会看到正在交付到webhook的AEM活动。 在Glitch webhook页面中查找POST请求。

![问题 — POST请求](../assets/examples/webhook/glitch-post-request.png)

以下是POST请求的关键详细信息：

- 路径： `/webhook/${YOUR-WEBHOOK-URL}`，例如`/webhook/AdobeTM-aem-eventing`

- 标头：由Adobe I/O Events发送的请求标头，例如：

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

- body/payload：由Adobe I/O Events发送的请求正文，例如：

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

您可以看到，AEM事件详细信息具有在webhook中处理该事件所需的所有信息。 例如，事件类型(`type`)、事件源(`source`)、事件ID (`event_id`)、事件时间(`time`)和事件数据(`data`)。

## 其他资源

- [Glitch webhook源代码](https://glitch.com/edit/#!/lovely-ancient-coaster)可供参考。
