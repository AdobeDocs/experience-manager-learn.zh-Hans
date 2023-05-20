---
title: 電子郵件服務
description: 瞭解如何設定AEMas a Cloud Service以使用輸出埠連線電子郵件服務。
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
kt: 9353
thumbnail: KT-9353.jpeg
exl-id: 5f919d7d-e51a-41e5-90eb-b1f6a9bf77ba
source-git-commit: c34c27955dbc084620ac4dd811ba4051ea83f447
workflow-type: tm+mt
source-wordcount: '367'
ht-degree: 0%

---

# 電子郵件服務

透過設定AEM從AEMas a Cloud Service傳送電子郵件 `DefaultMailService` 以使用進階網路輸出埠。

由於（大部分）郵件服務不會透過HTTP/HTTPS執行，因此必須代理從AEMas a Cloud Service連線到郵件服務。

+ `smtp.host` 設為OSGi環境變數 `$[env:AEM_PROXY_HOST;default=proxy.tunnel]` 因此會透過出口進行路由。
   + `$[env:AEM_PROXY_HOST]` 是AEMas a Cloud Service對應至內部的保留變數 `proxy.tunnel` 主機。
   + 請勿嘗試設定 `AEM_PROXY_HOST` 透過Cloud Manager。
+ `smtp.port` 設為 `portForward.portOrig` 對應到目的地電子郵件服務主機和連線埠的連線埠。 此範例使用對應： `AEM_PROXY_HOST:30465` → `smtp.sendgrid.com:465`.
   + 此 `smpt.port` 設為 `portForward.portOrig` 連線埠，而不是SMTP伺服器的實際連線埠。 兩者之間的對應 `smtp.port` 和 `portForward.portOrig` 連線埠由Cloud Manager建立 `portForwards` 規則（如下所示）。

由於密碼不得儲存在程式碼中，因此最好使用下列專案提供電子郵件服務的使用者名稱和密碼： [機密OSGi設定變數](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#secret-configuration-values)，使用AIO CLI或Cloud Manager API設定。

通常， [彈性連線埠輸出](../flexible-port-egress.md) 用於滿足與電子郵件服務的整合，除非有必要 `allowlist` AdobeIP，在此情況下 [專用輸出ip位址](../dedicated-egress-ip-address.md) 可使用。

此外，請檢閱AEM檔案，網址為 [傳送電子郵件](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/development-guidelines.html#sending-email).

## 進階網路支援

下列進階網路選項支援下列程式碼範例。

確保 [適當的](../advanced-networking.md#advanced-networking) 在執行本教學課程之前，已設定進階網路設定。

| 無進階網路 | [彈性的連線埠輸出](../flexible-port-egress.md) | [專用輸出IP位址](../dedicated-egress-ip-address.md) | [虛擬私人網路](../vpn.md) |
|:-----:|:-----:|:------:|:---------:|
| ✘ | ✔ | ✔ | ✔ |

## OSGi設定

此OSGi設定範例會透過下列Cloud Manager將AEM Mail OSGi服務設定為使用外部郵件服務 `portForwards` 的規則 [enableEnvironmentAdvancedNetworkingConfiguration](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/enableEnvironmentAdvancedNetworkingConfiguration) 作業。

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

設定AEM [DefaultmailService](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/development-guidelines.html#sending-email) 根據您的電子郵件提供者的要求(例如 `smtp.ssl`、等)。

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

此 `EMAIL_USERNAME` 和 `EMAIL_PASSWORD` OSGi變數和密碼可透過以下任一方式依環境設定：

+ [Cloud Manager環境設定](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/environment-variables.html)
+ 或使用 `aio CLI` 命令

   ```shell
   $ aio cloudmanager:set-environment-variables --programId=<PROGRAM_ID> <ENVIRONMENT_ID> --secret EMAIL_USERNAME "myApiKey" --secret EMAIL_PASSWORD "password123"
   ```
