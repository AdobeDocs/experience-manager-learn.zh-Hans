---
title: 电子邮件服务
description: 了解如何将AEMas a Cloud Service配置为使用出口端口与电子邮件服务连接。
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
jira: KT-9353
thumbnail: KT-9353.jpeg
exl-id: 5f919d7d-e51a-41e5-90eb-b1f6a9bf77ba
duration: 116
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '334'
ht-degree: 0%

---

# 电子邮件服务

通过配置AEM从AEMas a Cloud Service发送电子邮件 `DefaultMailService` 使用高级联网出口端口。

由于（大部分）邮件服务不会通过HTTP/HTTPS运行，因此必须代理从AEMas a Cloud Service到邮件服务的连接。

+ `smtp.host` 设置为OSGi环境变量 `$[env:AEM_PROXY_HOST;default=proxy.tunnel]` 所以它通过出口。
   + `$[env:AEM_PROXY_HOST]` 是一个保留变量，AEMas a Cloud Service会将它映射到内部 `proxy.tunnel` 主机。
   + 不要尝试设置 `AEM_PROXY_HOST` 通过Cloud Manager。
+ `smtp.port` 设置为 `portForward.portOrig` 映射到目标电子邮件服务主机和端口的端口。 此示例使用映射： `AEM_PROXY_HOST:30465` → `smtp.sendgrid.com:465`.
   + 此 `smpt.port` 设置为 `portForward.portOrig` 端口，而不是SMTP服务器的实际端口。 之间的映射 `smtp.port` 和 `portForward.portOrig` 端口由Cloud Manager建立 `portForwards` 规则（如下所示）。

由于密码不能存储在代码中，因此最好使用提供电子邮件服务的用户名和密码 [密码OSGi配置变量](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#secret-configuration-values)，使用AIO CLI或Cloud Manager API设置。

通常， [灵活端口出口](../flexible-port-egress.md) 用于满足与电子邮件服务的集成，除非需要 `allowlist` AdobeIP，在这种情况下 [专用出口ip地址](../dedicated-egress-ip-address.md) 可以使用。

此外，请查阅AEM文档 [发送电子邮件](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/development-guidelines.html#sending-email).

## 高级联网支持

以下高级联网选项支持以下代码示例。

确保 [适当](../advanced-networking.md#advanced-networking) 在执行本教程之前，已设置高级联网配置。

| 无高级联网 | [灵活端口出口](../flexible-port-egress.md) | [专用出口IP地址](../dedicated-egress-ip-address.md) | [虚拟专用网络](../vpn.md) |
|:-----:|:-----:|:------:|:---------:|
| ✘ | ✔ | ✔ | ✔ |

## OSGi配置

此OSGi配置示例将AEM Mail OSGi服务配置为通过以下Cloud Manager使用外部邮件服务 `portForwards` 的规则 [enableEnvironmentAdvancedNetworkingConfiguration](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/enableEnvironmentAdvancedNetworkingConfiguration) 操作。

```json
...
"portForwards": [{
    "name": "smtp.mymail.com",
    "portDest": 465,
    "portOrig": 30465
}]
...
```

+ `ui.config/src/jcr_root/apps/wknd-examples/osgiconfig/config/com.day.cq.mailer.DefaultMailService.cfg.json`

配置AEM [默认邮件服务](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/development-guidelines.html#sending-email) 根据您的电子邮件提供商的要求(例如 `smtp.ssl`、等)。

```json
{
    "smtp.host": "$[env:AEM_PROXY_HOST;default=proxy.tunnel]",
    "smtp.port": "30465",
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

此 `EMAIL_USERNAME` 和 `EMAIL_PASSWORD` 可以使用以下任一方式为每个环境设置OSGi变量和密钥：

+ [Cloud Manager环境配置](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/environment-variables.html)
+ 或使用 `aio CLI` 命令

  ```shell
  $ aio cloudmanager:set-environment-variables --programId=<PROGRAM_ID> <ENVIRONMENT_ID> --secret EMAIL_USERNAME "myApiKey" --secret EMAIL_PASSWORD "password123"
  ```
