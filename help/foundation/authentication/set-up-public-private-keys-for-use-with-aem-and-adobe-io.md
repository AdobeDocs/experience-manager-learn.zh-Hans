---
seo: Set up public and private keys for use with AEM and Adobe I/O
description: 'AEM使用公共／私有密钥对与Adobe I/O和其他web服务进行安全通信。 此简短教程说明如何使用openssl命令行工具生成兼容密钥和密钥库，该工具同时支持AEM和Adobe I/O。 '
version: 6.4, 6.5
feature: authentication
topics: authentication, integrations
activity: setup
audience: architect, developer, implementer
doc-type: tutorial
kt: 2450
translation-type: tm+mt
source-git-commit: 3f973e36531a2d04cbaf6bb8dd70b39fef7d8b2f
workflow-type: tm+mt
source-wordcount: '768'
ht-degree: 0%

---


# 设置用于Adobe I/O的公共和私钥

AEM使用公共／私有密钥对与Adobe I/O和其他web服务进行安全通信。 本简短教程说明了如何使用[!DNL openssl]命令行工具生成兼容密钥和密钥库，该工具同时支持AEM和Adobe I/O。

>[!CAUTION]
>
>本指南创建自签名密钥，这些自签名密钥对于开发和在较低环境中使用非常有用。 在生产场景中，密钥通常由组织的IT安全团队生成和管理。

## 生成公钥／私钥对{#generate-the-public-private-key-pair}

[[!DNL openssl]](https://www.openssl.org/docs/man1.0.2/man1/openssl.html)命令行工具的[[!DNL req] 命令](https://www.openssl.org/docs/man1.0.2/man1/req.html)可用于生成与Adobe I/O和Adobe Experience Manager兼容的密钥对。

```shell
$ openssl req -x509 -sha256 -nodes -days 365 -newkey rsa:2048 -keyout private.key -out certificate.crt
```

要完成[!DNL openssl generate]命令，请在请求时提供证书信息。 Adobe I/O和AEM并不关心这些值是什么，但它们应该与这些值保持一致，并描述您的密钥。

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

## 将密钥对添加到新密钥库{#add-key-pair-to-a-new-keystore}

密钥对可以添加到新的[!DNL PKCS12]密钥库。 作为[[!DNL openssl]'s [!DNL pcks12] 命令的一部分，](https://www.openssl.org/docs/man1.0.2/man1/pkcs12.html)密钥库的名称（通过`-  caname`）、密钥的名称（通过`-name`）和密钥库的密码（通过`-  passout`）被定义。

将密钥库和密钥加载到AEM时需要这些值。

```shell
$ openssl pkcs12 -export -caname my-keystore -in certificate.crt -name my-key -inkey private.key -out keystore.p12 -passout pass:my-password
```

此命令的输出为`keystore.p12`文件。

>[!NOTE]
>
>**[!DNL my-keystore]**、**[!DNL my-key]**&#x200B;和&#x200B;**[!DNL my-password]**&#x200B;的参数值将替换为您自己的值。

## 验证密钥库内容{#verify-the-keystore-contents}

Java [[!DNL keytool] 命令行工具](https://docs.oracle.com/middleware/1213/wls/SECMG/keytool-summary-appx.htm#SECMG818)提供对密钥库的可见性，以确保密钥在密钥库文件([!DNL keystore.p12])中成功加载。

```shell
$ keytool -keystore keystore.p12 -list

Enter keystore password: my-password

Keystore type: jks
Keystore provider: SUN

Your keystore contains 1 entry

my-key, Feb 5, 2019, PrivateKeyEntry,
Certificate fingerprint (SHA1): 7C:6C:25:BD:52:D3:3B:29:83:FD:A2:93:A8:53:91:6A:25:1F:2D:52
```

![验证Adobe I/O的密钥存储](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/adobe-io--public-keys.png)

## 将密钥库添加到AEM {#adding-the-keystore-to-aem}

AEM使用生成的&#x200B;**私钥**&#x200B;与Adobe I/O和其他web服务进行安全通信。 要使AEM能够访问私钥，必须将其安装到AEM用户的密钥库中。

导航到&#x200B;**AEM > [!UICONTROL 工具] > [!UICONTROL 安全] > [!UICONTROL 用户]**&#x200B;和&#x200B;**编辑用户**，私钥将与其关联。

### 创建AEM密钥库{#create-an-aem-keystore}

![在AEMAEM > ](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/aem--create-keystore.png)
*Tools >  [!UICONTROL Security]  >  [!UICONTROL Users]  >  [!UICONTROL Edit users] 中创建KeyStore*

如果提示创建密钥库，请执行此操作。 此密钥库仅存在于AEM中，不是通过openssl创建的密钥库。 密码可以是任何内容，不必与[!DNL openssl]命令中使用的密码相同。

### 通过密钥库{#install-the-private-key-via-the-keystore}安装私钥

![在AEMUser> Keystore中](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/aem--add-private-key.png)
*[!UICONTROL 添加] 私钥 [!UICONTROL >从密]  [!UICONTROL 钥库添加私钥]*

在用户的密钥库控制台中，单击&#x200B;**[!UICONTROL 从KeyStore文件]**&#x200B;添加私钥，并添加以下信息：

* **[!UICONTROL 新别名]**:aem里的钥匙的别名。这可以是任何内容，并且不必与使用openssl命令创建的密钥库的名称相对应。
* **[!UICONTROL 密钥存储文件]**:openssl pkcs12命令(keystore.p12)的输出
* **[!UICONTROL 密钥存储文件密码]**:openssl pkcs12命令中通过参数设置的 `-passout` 口令。
* **[!UICONTROL 私钥别名]**:上面openssl pkcs12 `-name` 命令中为参数提供的值(即 `my-key`)。
* **[!UICONTROL 私钥密码]**:openssl pkcs12命令中通过参数设置的 `-passout` 口令。

>[!CAUTION]
>
>KeyStore文件密码和私钥密码对于这两个输入都是相同的。 输入不匹配的密码将导致无法导入密钥。

### 验证私钥是否已加载到AEM keystore {#verify-the-private-key-is-loaded-into-the-aem-keystore}中

![验证AEMUser>密钥](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/aem--keystore.png)
*[!UICONTROL 库中] 的私 [!UICONTROL 钥]*

当私钥从提供的密钥库成功加载到AEM密钥库时，私钥的元数据显示在用户的密钥库控制台中。

## 将公钥添加到Adobe I/O{#adding-the-public-key-to-adobe-i-o}

匹配的公钥必须上传到Adobe I/O，以允许AEM服务用户进行安全通信，该用户具有公钥的相应私钥。

### 创建Adobe I/O新集成{#create-a-adobe-i-o-new-integration}

![创建Adobe I/O新集成](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/adobe-io--create-new-integration.png)

*[[!UICONTROL 创建Adobe I/O集成]](https://console.adobe.io/) > [!UICONTROL 新集成]*

在Adobe I/O创建新集成需要上传公共证书。 上传由`openssl req`命令生成的&#x200B;**certificate.crt**。

### 验证公钥是否已加载到Adobe I/O{#verify-the-public-keys-are-loaded-in-adobe-i-o}

![验证Adobe I/O的公钥](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/adobe-io--public-keys.png)

已安装的公钥及其到期日期列在Adobe I/O的[!UICONTROL 集成]控制台中。可通过&#x200B;**[!UICONTROL 添加公钥]**&#x200B;按钮添加多个公钥。

现在AEM持有私钥，而Adobe I/O集成则持有相应的公钥，使AEM能够与Adobe I/O进行安全通信。
