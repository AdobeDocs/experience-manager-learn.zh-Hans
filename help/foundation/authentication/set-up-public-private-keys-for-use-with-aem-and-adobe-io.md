---
seo: Set up public and private keys for use with AEM and Adobe I/O
description: 'AEM使用公共／私有密钥对与AdobeI/O和其他web服务进行安全通信。 本简短教程说明了如何使用openssl命令行工具生成兼容密钥和密钥库，该工具可同时与AEM和AdobeI/O一起使用。 '
version: 6.4, 6.5
feature: authentication
topics: authentication, integrations
activity: setup
audience: architect, developer, implementer
doc-type: tutorial
kt: 2450
translation-type: tm+mt
source-git-commit: c85a59a8bd180d5affe2a5bf5939dabfb2776d73
workflow-type: tm+mt
source-wordcount: '728'
ht-degree: 0%

---


# 设置用于AdobeI/O的公钥和私钥

AEM使用公共／私有密钥对与AdobeI/O和其他web服务进行安全通信。 此简短教程说明了如何使用命令行工具生成兼 [!DNL openssl] 容密钥和密钥库，该工具同时与AEM和AdobeI/O结合使用。

>[!CAUTION]
>
>本指南创建自签名密钥，这些自签名密钥对于开发和在较低环境中使用非常有用。 在生产场景中，密钥通常由组织的IT安全团队生成和管理。

## 生成公钥／私钥对 {#generate-the-public-private-key-pair}

[ [!DNL openssl]命令](https://www.openssl.org/docs/man1.0.2/man1/openssl.html) 行工具的命 [[!DNL req] 令](https://www.openssl.org/docs/man1.0.2/man1/req.html) ，可用于生成与AdobeI/O和Adobe Experience Manager兼容的密钥对。

```shell
$ openssl req -x509 -sha256 -nodes -days 365 -newkey rsa:2048 -keyout private.key -out certificate.crt
```

要完成命 [!DNL openssl generate] 令，请在请求时提供证书信息。 AdobeI/O和AEM不在乎这些值是什么，但它们应与您的键对齐并描述您的键。

```
Generating a 2048 bit RSA private key
...........................................................+++
...+++
writing new private key to 'private.key'
-----
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) []:US
State or Province Name (full name) []:CA
Locality Name (eg, city) []:San Jose
Organization Name (eg, company) []:Example Co
Organizational Unit Name (eg, section) []:Digital Marketing
Common Name (eg, fully qualified host name) []:com.example
Email Address []:me@example.com
```

## 将密钥对添加到新密钥库 {#add-key-pair-to-a-new-keystore}

密钥对可以添加到新密钥 [!DNL PKCS12] 库。 作为命 [[!DNL openssl]'s [!DNL pcks12] 令的一部](https://www.openssl.org/docs/man1.0.2/man1/pkcs12.html) 分，将定义密钥库的名称( `-  caname`通过)、密钥的名称（通过） `-name`和密钥库的密码（通过） `-  passout`。

将密钥库和密钥加载到AEM时需要这些值。

```shell
$ openssl pkcs12 -export -caname my-keystore -in certificate.crt -name my-key -inkey private.key -out keystore.p12 -passout pass:my-password
```

此命令的输出是文 `keystore.p12` 件。

>[!NOTE]
>
>和的参 **[!DNL my-keystore]**&#x200B;数 **[!DNL my-key]** 值 **[!DNL my-password]** 将替换为您自己的值。

## 验证密钥库内容 {#verify-the-keystore-contents}

Java命 [[!DNL keytool] 令行工具](https://docs.oracle.com/middleware/1213/wls/SECMG/keytool-summary-appx.htm#SECMG818) ，可以查看密钥库，以确保密钥在密钥库文件()中成功加[!DNL keystore.p12]载。

```shell
$ keytool -keystore keystore.p12 -list

Enter keystore password: my-password

Keystore type: jks
Keystore provider: SUN

Your keystore contains 1 entry

my-key, Feb 5, 2019, PrivateKeyEntry,
Certificate fingerprint (SHA1): 7C:6C:25:BD:52:D3:3B:29:83:FD:A2:93:A8:53:91:6A:25:1F:2D:52
```

![验证AdobeI/O中的密钥存储](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/adobe-io--public-keys.png)

## 将密钥库添加到AEM {#adding-the-keystore-to-aem}

AEM使用生成的 **私钥** ，与AdobeI/O和其他web服务进行安全通信。 要使AEM能够访问私钥，必须将其安装到AEM用户的密钥库中。

导航到 **AEM >[!UICONTROL 工具]>安[!UICONTROL 全]>用户[!UICONTROL 和编辑用]** 户密钥 **** ，专用密钥将与其关联。

### 创建AEM密钥库 {#create-an-aem-keystore}

![在AEM中创建KeyStore](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/aem--create-keystore.png)*[!UICONTROL >]Tools[!UICONTROL >]Security[!UICONTROL >]Edit Users*

如果提示创建密钥库，请执行此操作。 此密钥库仅存在于AEM中，不是通过openssl创建的密钥库。 密码可以是任何内容，不必与命令中使用的密码相 [!DNL openssl] 同。

### 通过密钥库安装私钥 {#install-the-private-key-via-the-keystore}

![在AEM User中添加](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/aem--add-private-key.png)*[!UICONTROL 私钥]>[!UICONTROL Keystore]>从[!UICONTROL keystore添加私钥]*

在用户的密钥库控制台中，单击“从 **[!UICONTROL KeyStore文件添加私钥]** ”，并添加以下信息：

* **[!UICONTROL 新别名]**:aem里的钥匙的别名。 这可以是任何内容，并且不必与使用openssl命令创建的密钥库的名称相对应。
* **[!UICONTROL 密钥库文件]**:openssl pkcs12命令(keystore.p12)的输出
* **[!UICONTROL 私钥别名]**:openssl pkcs12命令中通过参数设置的 `-  passout` 口令。

* **[!UICONTROL 私钥密码]**:openssl pkcs12命令中通过参数设置的 `-  passout` 口令。

### 验证私钥已加载到AEM密钥库中 {#verify-the-private-key-is-loaded-into-the-aem-keystore}

![验证AEM User > Keystore中的](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/aem--keystore.png)*[!UICONTROL 私钥][!UICONTROL > Keystore]*

当私钥从提供的密钥库成功加载到AEM密钥库时，私钥的元数据显示在用户的密钥库控制台中。

## 将公钥添加到AdobeI/O {#adding-the-public-key-to-adobe-i-o}

匹配的公钥必须上传到AdobeI/O以允许AEM服务用户进行安全通信，服务用户具有公钥的相应私钥。

### 创建AdobeI/O新集成 {#create-a-adobe-i-o-new-integration}

![创建AdobeI/O新集成](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/adobe-io--create-new-integration.png)

*[[!UICONTROL 创建AdobeI/O集成]](https://console.adobe.io/)>[!UICONTROL 新集成]*

在AdobeI/O中创建新集成需要上传公共证书。 上传 **命令生成** 的certificate. `openssl req` crt。

### 验证公钥是否已加载到AdobeI/O中 {#verify-the-public-keys-are-loaded-in-adobe-i-o}

![验证AdobeI/O中的公钥](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/adobe-io--public-keys.png)

已安装的公钥及其到期日期列在Adobe [!UICONTROL I] /O的集成控制台中。可通过“添加公钥”按 **[!UICONTROL 钮添加多个公钥]** 。

现在，AEM保留私钥，AdobeI/O集成保留相应的公钥，使AEM能够与AdobeI/O安全通信。
