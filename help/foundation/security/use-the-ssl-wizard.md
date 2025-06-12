---
title: 使用 AEM 中的 SSL 向导
description: Adobe Experience Manager 的 SSL 设置向导简化了配置流程，使设置通过 HTTPS 运行的 AEM 实例变得更加便捷。
version: Experience Manager 6.5, Experience Manager as a Cloud Service
jira: KT-13839
doc-type: Technical Video
topic: Security
role: Developer
level: Beginner
exl-id: 4e69e115-12a6-4a57-90da-b91e345c6723
last-substantial-update: 2023-08-08T00:00:00Z
duration: 564
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '448'
ht-degree: 100%

---

# 使用 AEM 中的 SSL 向导

了解如何在 Adobe Experience Manager 中使用内置的 SSL 向导设置 SSL，以使其通过 HTTPS 运行。

>[!VIDEO](https://video.tv.adobe.com/v/17993?quality=12&learn=on)


>[!NOTE]
>
>对于托管环境，IT 部门最好提供 CA 信任的证书和密钥。
>
>自签名证书仅用于开发目的。

## 使用 SSL 配置向导

导航至 __AEM Author > 工具 > 安全性 > SSL 配置__，并打开 __SSL 配置向导__。

![SSL 配置向导](assets/use-the-ssl-wizard/ssl-config-wizard.png)

### 创建存储凭据

要创建与 `ssl-service` 系统用户相关联的&#x200B;_密钥存储区_&#x200B;和全局&#x200B;_信任存储区_，请使用&#x200B;__存储凭据__&#x200B;向导步骤。

1. 输入与 `ssl-service` 系统用户关联的&#x200B;__密钥存储区__&#x200B;的密码并确认密码。
1. 请输入全局&#x200B;__信任存储区__&#x200B;的密码并确认密码。请注意，这是一个系统范围的信任存储区，如果已创建，则输入的密码将会被忽略。

   ![SSL 设置 - 存储凭据](assets/use-the-ssl-wizard/store-credentials.png)

### 上传私钥和证书

要上传&#x200B;_私钥_&#x200B;和 _SSL 证书_，请使用&#x200B;__密钥与证书__&#x200B;向导步骤。

通常，您的 IT 部门会提供受证书颁发机构 (CA) 信任的证书和密钥，但自签名证书也可用于&#x200B;__开发__&#x200B;和&#x200B;__测试__&#x200B;目的。

要创建或下载自签名证书，请参阅[自签名私钥和证书](#self-signed-private-key-and-certificate)。

1. 以DER（区别编码规则）格式上传&#x200B;__私钥__。与 PEM 不同，DER 编码的文件不包含诸如 `-----BEGIN CERTIFICATE-----` 之类的纯文本语句
1. 上传 `.crt` 格式的相关 __SSL 证书__。

   ![SSL 设置 - 私钥和证书](assets/use-the-ssl-wizard/privatekey-and-certificate.png)

### 更新 SSL 连接器详细信息

要更新&#x200B;_主机名_&#x200B;和&#x200B;_端口_，请使用 __SSL 连接器__&#x200B;向导步骤。

1. 更新或验证 __HTTPS 主机名__&#x200B;值，该值应与证书中的 `Common Name (CN)` 相匹配。
1. 更新或验证 __HTTPS 端口__&#x200B;值。

   ![SSL 设置 - SSL 连接器详细信息](assets/use-the-ssl-wizard/ssl-connector-details.png)

### 验证 SSL 设置

1. 要验证 SSL，请点击&#x200B;__转到 HTTPS URL__ 按钮。
1. 如果使用自签名证书，您会看到 `Your connection is not private` 错误。

   ![SSL 设置 - 通过 HTTPS 验证 AEM](assets/use-the-ssl-wizard/verify-aem-over-ssl.png)

## 自签名私钥和证书

以下压缩包包含在本地设置 AEM SSL 所需的 [!DNL DER] 和 [!DNL CRT] 文件，仅供本地开发使用。

[!DNL DER] 和 [!DNL CERT] 文件是为了方便而提供的，并使用下文“生成私钥和自签名证书”部分中概述的步骤生成。

如果需要，证书密码为 **admin**。

此本地主机 - 私钥和自签名证书.zip（2028 年 7 月到期）

[下载证书文件](assets/use-the-ssl-wizard/certificate.zip)

### 私钥和自签名证书生成

上述视频展示了如何使用自签名证书在 AEM 作者实例上设置和配置 SSL。以下使用 [[!DNL OpenSSL]](https://www.openssl.org/) 的命令可以生成一个私钥和证书，以用于向导的第二步。

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
