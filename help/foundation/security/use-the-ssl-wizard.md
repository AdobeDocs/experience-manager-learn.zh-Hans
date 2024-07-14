---
title: 在AEM中使用“SSL向导”
description: Adobe Experience Manager的SSL设置向导，以便更轻松地设置AEM实例以通过HTTPS运行。
version: 6.5, Cloud Service
jira: KT-13839
doc-type: Technical Video
topic: Security
role: Developer
level: Beginner
exl-id: 4e69e115-12a6-4a57-90da-b91e345c6723
last-substantial-update: 2023-08-08T00:00:00Z
duration: 564
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '448'
ht-degree: 0%

---

# 在AEM中使用“SSL向导”

了解如何在Adobe Experience Manager中设置SSL，以使其使用内置的SSL向导通过HTTPS运行。

>[!VIDEO](https://video.tv.adobe.com/v/17993?quality=12&learn=on)


>[!NOTE]
>
>对于托管环境，最好由IT部门提供CA信任的证书和密钥。
>
>自签名证书仅用于开发目的。

## 使用SSL配置向导

导航到&#x200B;__AEM Author > Tools > Security > SSL Configuration__，然后打开&#x200B;__SSL Configuration Wizard__。

![SSL配置向导](assets/use-the-ssl-wizard/ssl-config-wizard.png)

### 创建商店凭据

要创建与`ssl-service`系统用户和全局&#x200B;_信任存储_&#x200B;关联的&#x200B;_密钥存储_，请使用&#x200B;__存储凭据__&#x200B;向导步骤。

1. 输入与`ssl-service`系统用户关联的&#x200B;__密钥库__&#x200B;的密码并确认密码。
1. 输入全局&#x200B;__信任存储区__&#x200B;的密码并确认密码。 请注意，它是系统范围的Trust Store，如果已创建，则忽略输入的密码。

   ![SSL安装程序 — 存储凭据](assets/use-the-ssl-wizard/store-credentials.png)

### 上传私钥和证书

要上传&#x200B;_私钥_&#x200B;和&#x200B;_SSL证书_，请使用&#x200B;__密钥和证书__&#x200B;向导步骤。

通常情况下，您的IT部门会提供CA信任的证书和密钥，但自签名证书可用于&#x200B;__开发__&#x200B;和&#x200B;__测试__&#x200B;目的。

若要创建或下载自签名证书，请参阅[自签名私钥和证书](#self-signed-private-key-and-certificate)。

1. 以DER（可分辨编码规则）格式上传&#x200B;__私钥__。 与PEM不同，DER编码文件不包含纯文本语句，如`-----BEGIN CERTIFICATE-----`
1. 以`.crt`格式上载关联的&#x200B;__SSL证书__。

   ![SSL设置 — 私钥和证书](assets/use-the-ssl-wizard/privatekey-and-certificate.png)

### 更新SSL连接器详细信息

要更新&#x200B;_主机名_&#x200B;和&#x200B;_端口_，请使用&#x200B;__SSL连接器__&#x200B;向导步骤。

1. 更新或验证&#x200B;__HTTPS主机名__&#x200B;值，该值应与证书中的`Common Name (CN)`匹配。
1. 更新或验证&#x200B;__HTTPS端口__&#x200B;值。

   ![SSL安装程序 — SSL连接器详细信息](assets/use-the-ssl-wizard/ssl-connector-details.png)

### 验证SSL设置

1. 要验证SSL，请单击&#x200B;__转到HTTPS URL__&#x200B;按钮。
1. 如果使用自签名证书，您会看到`Your connection is not private`错误。

   ![SSL设置 — 通过HTTPS验证AEM](assets/use-the-ssl-wizard/verify-aem-over-ssl.png)

## 自签名私钥和证书

以下zip文件包含本地设置AEM SSL所需的[!DNL DER]和[!DNL CRT]文件，这些文件仅用于本地开发。

为方便起见，提供了[!DNL DER]和[!DNL CERT]文件，这些文件是使用下面的“生成私钥和自签名证书”一节中所述的步骤生成的。

如果需要，证书密码短语为&#x200B;**admin**。

此localhost — 私钥和自签名的certificate.zip（2028年7月到期）

[下载证书文件](assets/use-the-ssl-wizard/certificate.zip)

### 私钥和自签名证书生成

上视频描述了AEM创作实例上使用自签名证书的SSL的设置和配置。 使用[[!DNL OpenSSL]](https://www.openssl.org/)的以下命令可以生成私钥和证书以在向导的步骤2中使用。

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
