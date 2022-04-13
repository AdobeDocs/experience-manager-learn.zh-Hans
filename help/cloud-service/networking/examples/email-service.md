---
title: 电子邮件服务
description: 了解如何配置AEMas a Cloud Service以使用出口端连接电子邮件服务。
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
kt: 9353
thumbnail: KT-9353.jpeg
exl-id: 5f919d7d-e51a-41e5-90eb-b1f6a9bf77ba
source-git-commit: 8da6d5470c702620ee1121fd2688eb8756f0cebd
workflow-type: tm+mt
source-wordcount: '351'
ht-degree: 0%

---

# 电子邮件服务

通过配置AEM从AEMas a Cloud Service发送电子邮件 `DefaultMailService` 使用高级网络出口端口。

由于（大多数）邮件服务不会通过HTTP/HTTPS运行，因此必须代理从AEMas a Cloud Service连接到邮件服务。

+ `smtp.host` 设置为OSGi环境变量 `$[env:AEM_PROXY_HOST;default=proxy.tunnel]` 所以它被从出口路过。
   + `$[env:AEM_PROXY_HOST]` 是AEMas a Cloud Service映射到内部的保留变量 `proxy.tunnel` 主机。
   + 请勿尝试设置 `AEM_PROXY_HOST` 通过Cloud Manager。
+ `smtp.port` 设置为 `portForward.portOrig` 映射到目标电子邮件服务的主机和端口的端口。 此示例使用映射： `AEM_PROXY_HOST:30002` → `smtp.sendgrid.com:465`.
   + 的 `smpt.port` 设置为 `portForward.portOrig` 端口，而不是SMTP服务器的实际端口。 在 `smtp.port` 和 `portForward.portOrig` 端口由Cloud Manager建立 `portForwards` 规则（如下所示）。

由于密钥不得存储在代码中，因此最好使用 [密钥OSGi配置变量](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#secret-configuration-values)，使用AIO CLI或Cloud Manager API进行设置。

通常， [灵活的端口出口](../flexible-port-egress.md) 用于满足与电子邮件服务的集成，除非 `allowlist` AdobeIP，在这种情况下 [专用出口ip地址](../dedicated-egress-ip-address.md) 中。

## 高级网络支持

以下高级网络选项支持以下代码示例。

确保 [适当](../advanced-networking.md#advanced-networking) 在完成本教程之前，已设置高级网络配置。

| 无高级网络 | [灵活的端口出口](../flexible-port-egress.md) | [专用出口IP地址](../dedicated-egress-ip-address.md) | [虚拟专用网](../vpn.md) |
|:-----:|:-----:|:------:|:---------:|
| ✘ | ✔ | ✔ | ✔ |

## OSGi配置

此OSGi配置示例通过以下Cloud Manager将AEM Mail OSGi服务配置为使用外部邮件服务 `portForwards` 规则 [enableEnvironmentAdvancedNetworkingConfiguration](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/enableEnvironmentAdvancedNetworkingConfiguration) 操作。

```json
...
"portForwards": [{
    "name": "smtp.mymail.com",
    "portDest": 465,
    "portOrig": 30002
}]
...
```

+ `ui.config/src/jcr_root/apps/wknd-examples/osgiconfig/config/com.day.cq.mailer.DefaultMailService.cfg.json`

配置AEM [DefaulMailService](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/development-guidelines.html#sending-email) 电子邮件提供商(例如 `smtp.ssl`等)。

```json
{
    "smtp.host": "$[env:AEM_PROXY_HOST;default=proxy.tunnel]",
    "smtp.port": "30002",
    "smtp.user": "$[env:EMAIL_USERNAME;default=myApiKey]",
    "smtp.password": "$[secret:EMAIL_PASSWORD]",
    "from.address": "noreply@wknd.site",
    "smtp.ssl": true,
    "smtp.starttls": false, 
    "smtp.requiretls": false,
    "debug.email": false,
    "oauth.flow": false
}
```

的 `EMAIL_USERNAME` 和 `EMAIL_PASSWORD` 可使用以下任一方式为每个环境设置OSGi变量和密钥：

+ [Cloud Manager环境配置](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/environment-variables.html)
+ 或使用 `aio CLI` 命令

   ```shell
   $ aio cloudmanager:set-environment-variables --programId=<PROGRAM_ID> <ENVIRONMENT_ID> --secret EMAIL_USERNAME "myApiKey" --secret EMAIL_PASSWORD "password123"
   ```
