---
seo: Set up public and private keys for use with AEM and Adobe I/O
description: 'AEM使用公钥/私钥对与Adobe I/O和其他web服务进行安全通信。 本简短教程说明了如何使用与AEM和Adobe I/O同时使用的openssl命令行工具生成兼容的键值和密钥库。 '
version: 6.4, 6.5
feature: 用户和群组
topics: authentication, integrations
activity: setup
audience: architect, developer, implementer
doc-type: tutorial
kt: 2450
topic: 开发
role: Developer
level: Experienced
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '772'
ht-degree: 0%

---


# 设置公钥和私钥以用于Adobe I/O

AEM使用公钥/私钥对与Adobe I/O和其他web服务进行安全通信。 本简短教程说明了如何使用[!DNL openssl]命令行工具生成兼容的键值和密钥库，该工具可同时用于AEM和Adobe I/O。

>[!CAUTION]
>
>本指南创建自签名密钥，这些自签名密钥对于在较低环境中开发和使用非常有用。 在生产方案中，密钥通常由组织的IT安全团队生成和管理。

## 生成公钥/私钥对 {#generate-the-public-private-key-pair}

[[!DNL openssl]](https://www.openssl.org/docs/man1.0.2/man1/openssl.html)命令行工具的[[!DNL req] 命令](https://www.openssl.org/docs/man1.0.2/man1/req.html)可用于生成与Adobe I/O和Adobe Experience Manager兼容的键对。

```shell
$ openssl req -x509 -sha256 -nodes -days 365 -newkey rsa:2048 -keyout private.key -out certificate.crt
```

要完成[!DNL openssl generate]命令，请在请求时提供证书信息。 Adobe I/O和AEM不关心这些值是什么，但它们应该与保持一致，并描述您的键值。

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

密钥对可以添加到新的[!DNL PKCS12]密钥库。 在[[!DNL openssl]'s [!DNL pcks12] 命令中，](https://www.openssl.org/docs/man1.0.2/man1/pkcs12.html)密钥库的名称（通过`-  caname`）、密钥的名称（通过`-name`）和密钥库的密码（通过`-  passout`）。

将密钥库和密钥加载到AEM中时需要这些值。

```shell
$ openssl pkcs12 -export -caname my-keystore -in certificate.crt -name my-key -inkey private.key -out keystore.p12 -passout pass:my-password
```

此命令的输出是`keystore.p12`文件。

>[!NOTE]
>
>**[!DNL my-keystore]**、**[!DNL my-key]**&#x200B;和&#x200B;**[!DNL my-password]**&#x200B;的参数值将替换为您自己的值。

## 验证密钥库内容 {#verify-the-keystore-contents}

Java [[!DNL keytool] 命令行工具](https://docs.oracle.com/middleware/1213/wls/SECMG/keytool-summary-appx.htm#SECMG818)提供对密钥库的可见性，以确保密钥已成功加载到密钥库文件([!DNL keystore.p12])中。

```shell
$ keytool -keystore keystore.p12 -list

Enter keystore password: my-password

Keystore type: jks
Keystore provider: SUN

Your keystore contains 1 entry

my-key, Feb 5, 2019, PrivateKeyEntry,
Certificate fingerprint (SHA1): 7C:6C:25:BD:52:D3:3B:29:83:FD:A2:93:A8:53:91:6A:25:1F:2D:52
```

![验证密钥存储在Adobe I/O中](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/adobe-io--public-keys.png)

## 将密钥库添加到AEM {#adding-the-keystore-to-aem}

AEM使用生成的&#x200B;**私钥**&#x200B;与Adobe I/O和其他web服务进行安全通信。 要使AEM能够访问私钥，必须将其安装到AEM用户的密钥库中。

导航至&#x200B;**AEM > [!UICONTROL 工具] > [!UICONTROL 安全] > [!UICONTROL 用户]**&#x200B;和&#x200B;**编辑用户**，私钥将与其关联。

### 创建AEM密钥库 {#create-an-aem-keystore}

![在AEM >工](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/aem--create-keystore.png)
*具 [!UICONTROL  > ] 安全 [!UICONTROL  > ] 用户   >编辑用户中创建KeyStore*

如果提示创建密钥库，请执行此操作。 此密钥库将仅存在于AEM中，并且不是通过openssl创建的密钥库。 密码可以是任何内容，并且不必与[!DNL openssl]命令中使用的密码相同。

### 通过密钥库安装私钥 {#install-the-private-key-via-the-keystore}

![在AEMUser](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/aem--add-private-key.png)
*[!UICONTROL > ] Keystore [!UICONTROL  > ]  [!UICONTROL 从keystore添加私钥]*

在用户的密钥库控制台中，单击&#x200B;**[!UICONTROL 从KeyStore文件添加私钥]**&#x200B;并添加以下信息：

* **[!UICONTROL 新别名]**:键在AEM中的别名。这可以是任何内容，并且不必与使用openssl命令创建的密钥库的名称相对应。
* **[!UICONTROL KeyStore文件]**:openssl pkcs12命令的输出(keystore.p12)
* **[!UICONTROL KeyStore文件密码]**:在openssl pkcs12命令中通过参数设置的 `-passout` 密码。
* **[!UICONTROL 私钥别名]**:上面openssl pkcs12命 `-name` 令中为参数提供的值(即 `my-key`)。
* **[!UICONTROL 私钥密码]**:在openssl pkcs12命令中通过参数设置的 `-passout` 密码。

>[!CAUTION]
>
>对于这两个输入，KeyStore文件密码和私钥密码是相同的。 输入不匹配的密码将导致无法导入密钥。

### 验证私钥是否已加载到AEM密钥库中 {#verify-the-private-key-is-loaded-into-the-aem-keystore}

![验证AEMUser](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/aem--keystore.png)
*[!UICONTROL > ] Keystore中的私 [!UICONTROL 钥]*

将私钥从提供的密钥库成功加载到AEM密钥库中后，私钥的元数据将显示在用户的密钥库控制台中。

## 将公钥添加到Adobe I/O {#adding-the-public-key-to-adobe-i-o}

必须将匹配的公钥上传到Adobe I/O，以允许AEM服务用户安全通信，该用户具有公钥的相应私钥。

### 创建Adobe I/O新集成 {#create-a-adobe-i-o-new-integration}

![创建Adobe I/O新集成](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/adobe-io--create-new-integration.png)

*[[!UICONTROL 创建Adobe I/O集成]](https://console.adobe.io/) >新 [!UICONTROL 集成]*

在Adobe I/O中创建新集成需要上传公共证书。 上传由`openssl req`命令生成的&#x200B;**certificate.crt**。

### 验证公钥是否已加载到Adobe I/O中 {#verify-the-public-keys-are-loaded-in-adobe-i-o}

![验证公共密钥在Adobe I/O中](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/adobe-io--public-keys.png)

已安装的公钥及其过期日期列在Adobe I/O的[!UICONTROL Integrations]控制台中。可通过&#x200B;**[!UICONTROL Add a public key]**&#x200B;按钮添加多个公钥。

现在，AEM保留私钥，Adobe I/O集成保留相应的公钥，从而允许AEM与Adobe I/O安全通信。
