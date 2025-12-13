---
title: 电子邮件服务
description: 了解如何配置AEM as a Cloud Service以使用出口端口连接电子邮件服务。
version: Experience Manager as a Cloud Service
feature: Security
topic: Development, Security
role: Developer
level: Intermediate
jira: KT-9353
thumbnail: KT-9353.jpeg
exl-id: 5f919d7d-e51a-41e5-90eb-b1f6a9bf77ba
duration: 76
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '330'
ht-degree: 0%

---

# 电子邮件服务

通过将AEM as a Cloud Service的`DefaultMailService`配置为使用高级联网出口端口，从AEM发送电子邮件。

由于（大部分）邮件服务不会通过HTTP/HTTPS运行，因此必须代理从AEM as a Cloud Service到邮件服务的连接。

+ `smtp.host`设置为OSGi环境变量`$[env:AEM_PROXY_HOST;default=proxy.tunnel]`，以便通过出口路由。
   + `$[env:AEM_PROXY_HOST]`是AEM as a Cloud Service映射到内部`proxy.tunnel`主机的保留变量。
   + 请勿尝试通过Cloud Manager设置`AEM_PROXY_HOST`。
+ `smtp.port`设置为映射到目标电子邮件服务主机和端口的`portForward.portOrig`端口。 此示例使用映射： `AEM_PROXY_HOST:30465` → `smtp.sendgrid.com:465`。
   + `smpt.port`设置为`portForward.portOrig`端口，而不是SMTP服务器的实际端口。 `smtp.port`和`portForward.portOrig`端口之间的映射由Cloud Manager `portForwards`规则建立（如下所示）。

由于密码不能存储在代码中，因此最好使用[机密OSGi配置变量](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#secret-configuration-values)、使用AIO CLI或Cloud Manager API设置来提供电子邮件服务的用户名和密码。

通常，[灵活端口出口](../flexible-port-egress.md)用于满足与电子邮件服务的集成，除非需要`allowlist` Adobe IP，在这种情况下，可以使用[专用出口IP地址](../dedicated-egress-ip-address.md)。

此外，请查看有关[发送电子邮件](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/development-guidelines.html#sending-email)的AEM文档。

## 高级联网支持

以下高级联网选项支持以下代码示例。

在执行本教程之前，请确保已设置[适当的](../advanced-networking.md#advanced-networking)高级联网配置。

| 无高级联网 | [灵活端口出口](../flexible-port-egress.md) | [专用出口IP地址](../dedicated-egress-ip-address.md) | [虚拟专用网络](../vpn.md) |
|:-----:|:-----:|:------:|:---------:|
| ✘ | ✔ | ✔ | ✔ |

## OSGi配置

此OSGi配置示例将AEM的Mail OSGi服务配置为通过`portForwards`enableEnvironmentAdvancedNetworkingConfiguration[操作的以下Cloud Manager &#x200B;](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/enableEnvironmentAdvancedNetworkingConfiguration)规则使用外部邮件服务。

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

根据电子邮件提供商的要求（例如[等）配置AEM的](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/development-guidelines.html#sending-email)DefaulMailService`smtp.ssl`。

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

可以使用以下任一方式为每个环境设置`EMAIL_USERNAME`和`EMAIL_PASSWORD` OSGi变量和密钥：

+ [Cloud Manager环境配置](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/environment-variables.html)
+ 或使用`aio CLI`命令

  ```shell
  $ aio cloudmanager:set-environment-variables --programId=<PROGRAM_ID> <ENVIRONMENT_ID> --secret EMAIL_USERNAME "myApiKey" --secret EMAIL_PASSWORD "password123"
  ```
