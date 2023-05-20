---
title: 在AEM中使用SSL精靈
description: Adobe Experience Manager的SSL設定精靈，讓您更輕鬆地設定AEM執行個體以透過HTTPS執行。
seo-description: Adobe Experience Manager's SSL setup wizard to make it easier to set up an AEM instance to run over HTTPS.
version: 6.4, 6.5
topics: security, operations
activity: use
audience: administrator
doc-type: technical video
uuid: 82a6962e-3658-427a-bfad-f5d35524f93b
discoiquuid: 9e666741-0f76-43c9-ab79-1ef149884686
topic: Security
role: Developer
level: Beginner
exl-id: 4e69e115-12a6-4a57-90da-b91e345c6723
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '211'
ht-degree: 0%

---

# 在AEM中使用SSL精靈

Adobe Experience Manager的SSL設定精靈，讓您更輕鬆地設定AEM執行個體以透過HTTPS執行。

>[!VIDEO](https://video.tv.adobe.com/v/17993?quality=12&learn=on)

開啟 __SSL設定精靈__ 可透過導覽至「 」直接開啟 __AEM作者>工具>安全性> SSL設定__.

>[!NOTE]
>
>對於受管理的環境，最好由IT部門提供CA信任的憑證和金鑰。
>
>自我簽署憑證只能用於開發目的。

## 私密金鑰和自簽憑證下載

下列zip包含 [!DNL DER] 和 [!DNL CRT] 在localhost上設定AEM SSL所需的檔案，且僅供本機開發使用。

此 [!DNL DER] 和 [!DNL CERT] 檔案的提供是為了方便起見，並使用以下「產生私密金鑰和自簽憑證」一節中概述的步驟產生。

如有需要，憑證密語為 **管理員**.

localhost — 私密金鑰和自簽的certificate.zip （2028年7月到期）

[下載憑證檔案](assets/use-the-ssl-wizard/certificate.zip)

## 私密金鑰和自簽署憑證產生

上述影片說明如何使用自我簽署憑證，在AEM編寫執行個體上設定SSL。 以下命令使用 [[!DNL OpenSSL]](https://www.openssl.org/) 可產生私密金鑰和憑證，以用於精靈的步驟2。

```shell
### Create Private Key
$ openssl genrsa -aes256 -out localhostprivate.key 4096

### Generate Certificate Signing Request using private key
$ openssl req -sha256 -new -key localhostprivate.key -out localhost.csr -subj '/CN=localhost'

### Generate the SSL certificate and sign with the private key, will expire one year from now
$ openssl x509 -req -extfile <(printf "subjectAltName=DNS:localhost") -days 365 -in localhost.csr -signkey localhostprivate.key -out localhost.crt

### Convert Private Key to DER format - SSL wizard requires key to be in DER format
$ openssl pkcs8 -topk8 -inform PEM -outform DER -in localhostprivate.key -out localhostprivate.der -nocrypt
```
