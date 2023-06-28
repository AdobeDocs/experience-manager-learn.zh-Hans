---
title: 在AEM中使用SSL向导
description: Adobe Experience Manager的SSL设置向导，以便更轻松地设置AEM实例以通过HTTPS运行。
seo-description: Adobe Experience Manager's SSL setup wizard to make it easier to set up an AEM instance to run over HTTPS.
version: 6.4, 6.5
topics: security, operations
feature: Security
activity: use
audience: administrator
doc-type: technical video
uuid: 82a6962e-3658-427a-bfad-f5d35524f93b
discoiquuid: 9e666741-0f76-43c9-ab79-1ef149884686
topic: Security
role: Developer
level: Beginner
exl-id: 4e69e115-12a6-4a57-90da-b91e345c6723
source-git-commit: eecc275e38390b9330464c8ac0750efa2c702c82
workflow-type: tm+mt
source-wordcount: '211'
ht-degree: 0%

---

# 在AEM中使用SSL向导

Adobe Experience Manager的SSL设置向导，以便更轻松地设置AEM实例以通过HTTPS运行。

>[!VIDEO](https://video.tv.adobe.com/v/17993?quality=12&learn=on)

打开 __SSL配置向导__ 可以直接通过导航到 __AEM创作>工具>安全> SSL配置__.

>[!NOTE]
>
>对于托管环境，最好由IT部门提供CA信任的证书和密钥。
>
>自签名证书仅用于开发目的。

## 私钥和自签名证书下载

以下zip文件包含 [!DNL DER] 和 [!DNL CRT] 在本地主机上设置AEM SSL所需的文件，并且仅用于本地开发。

此 [!DNL DER] 和 [!DNL CERT] 为方便起见，提供了文件，并使用下面的“生成私钥和自签名证书”部分中概述的步骤生成文件。

如果需要，证书密码短语为 **管理员**.

localhost — 私钥和自签名certificate.zip（2028年7月到期）

[下载证书文件](assets/use-the-ssl-wizard/certificate.zip)

## 私钥和自签名证书生成

上视频描述了AEM创作实例上使用自签名证书的SSL的设置和配置。 以下命令使用 [[!DNL OpenSSL]](https://www.openssl.org/) 可以生成要在向导的步骤2中使用的私钥和证书。

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
