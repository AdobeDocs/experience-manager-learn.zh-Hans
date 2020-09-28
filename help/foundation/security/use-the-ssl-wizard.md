---
title: 在AEM中使用SSL向导
description: Adobe Experience Manager的SSL设置向导，可更轻松地设置AEM实例以通过HTTPS运行。
seo-description: Adobe Experience Manager的SSL设置向导，可更轻松地设置AEM实例以通过HTTPS运行。
version: 6.3, 6,4, 6.5
feature: null
topics: security, operations
activity: use
audience: administrator
doc-type: technical video
uuid: 82a6962e-3658-427a-bfad-f5d35524f93b
discoiquuid: 9e666741-0f76-43c9-ab79-1ef149884686
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '215'
ht-degree: 0%

---


# 在AEM中使用SSL向导

Adobe Experience Manager的SSL设置向导，可更轻松地设置AEM实例以通过HTTPS运行。

>[!VIDEO](https://video.tv.adobe.com/v/17993/?quality=12&learn=on)

>[!NOTE]
>
>对于托管环境，最好由IT部门提供CA信任的证书和密钥。
>
>自签名证书仅用于开发目的。

## 私钥和自签名证书下载

以下zip文件包 [!DNL DER] 含在 [!DNL CRT] 本地主机上设置AEM SSL所需的文件和文件，仅用于本地开发目的。

为方 [!DNL DER] 便起 [!DNL CERT] 见，提供这些和文件，并使用下面“生成私钥和自签名证书”部分中概述的步骤生成。

如果需要，证书密码短语为 **admin**。

localhost —— 私钥和自签名证书。zip（到2028年7月为止）

[下载证书文件](assets/use-the-ssl-wizard/certificate.zip)

## 私钥和自签名证书生成

上述视频描述了使用自签名证书在AEM作者实例上设置和配置SSL的过程。 使用[!DNL [OpenSSL]的以下命令](https://www.openssl.org/) ，可以生成要在向导的步骤2中使用的私钥和证书。

```shell
### Create Private Key
$ openssl genrsa -aes256 -out localhostprivate.key 4096

### Generate Certificate Signing Request using private key
$ openssl req -sha256 -new -key localhostprivate.key -out localhost.csr -subj '/CN=localhost'

### Generate the SSL certificate and sign with the private key, will expire one year from now
$ openssl x509 -req -days 365 -in localhost.csr -signkey localhostprivate.key -out localhost.crt

### Convert Private Key to DER format - SSL wizard requires key to be in DER format
$ openssl pkcs8 -topk8 -inform PEM -outform DER -in localhostprivate.key -out localhostprivate.der -nocrypt
```
