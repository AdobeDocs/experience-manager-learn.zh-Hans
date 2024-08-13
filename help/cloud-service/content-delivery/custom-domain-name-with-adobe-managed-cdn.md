---
title: 具有Adobe托管的CDN的自定义域名
description: 了解如何在使用Adobe托管的CDN的AEM as a Cloud Service网站中实施自定义域名。
version: Cloud Service
feature: Cloud Manager, Operations
topic: Administration, Architecture
role: Admin, Architect, Developer
level: Intermediate
doc-type: Tutorial
duration: 1042
last-substantial-update: 2024-08-12T00:00:00Z
jira: KT-15121
thumbnail: KT-15121.jpeg
exl-id: 8936c3ae-2daf-4d0f-b260-28376ae28087
source-git-commit: 07225f1ae4455e2fa69c8e488851361c725fe9e8
workflow-type: tm+mt
source-wordcount: '728'
ht-degree: 0%

---

# 具有AdobeCDN的自定义域名

了解如何为使用Adobe内容分发网络(CDN)的AEM as a Cloud Service网站实施自定义域名。

在本教程中，通过添加具有传输层安全性(TLS)的HTTPS可寻址自定义域名`wknd.enablementadobe.com`，增强了示例[AEM WKND](https://github.com/adobe/aem-guides-wknd)站点的品牌化。

>[!VIDEO](https://video.tv.adobe.com/v/3427903?quality=12&learn=on)

高级步骤包括：

具有AdobeCDN](./assets/add-custom-domain-name-with-Adobe-CDN.png){width="800" zoomable="yes"}的![自定义域名

## 先决条件

>[!VIDEO](https://video.tv.adobe.com/v/3427909?quality=12&learn=on)

- 本地计算机上安装了[OpenSSL](https://www.openssl.org/)和[dig](https://www.isc.org/blogs/dns-checker/)。
- 访问第三方服务：
   - 证书颁发机构(CA) — 为您的站点域（如[DigitCert](https://www.digicert.com/)）请求已签名的证书
   - 域名系统(DNS)托管服务 — 为您的自定义域添加DNS记录，如Azure DNS或AWS Route 53。
- 以&#x200B;**业务负责人**&#x200B;或&#x200B;**Adobe管理员**&#x200B;角色访问[部署的Cloud Manager](https://my.cloudmanager.adobe.com/)。
- 示例[AEM WKND](https://github.com/adobe/aem-guides-wknd)站点已部署到[生产程序](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-production-programs)类型的AEM as a Cloud Service环境。

如果您无权访问第三方服务，请&#x200B;_与您的安全或托管团队协作以完成步骤_。

## 生成SSL证书

>[!VIDEO](https://video.tv.adobe.com/v/3427908?quality=12&learn=on)

您有两个选项：

1. 使用`openssl`命令行工具为您的站点域生成私钥和证书签名请求(CSR)。 要请求已签名的证书，请将CSR提交到证书颁发机构(CA)。
1. 您的托管团队为您的站点提供所需的私钥和签名证书。

让我们回顾一下第一个选项的步骤。

要生成私钥和CSR，请运行以下命令并在出现提示时提供所需信息：

```bash
# Generate a private key and a CSR
$ openssl req -newkey rsa:2048 -keyout <YOUR-SITE-NAME>.key -out <YOUR-SITE-NAME>.csr -nodes
```

要请求签名证书，请按照CA的文档向CA提供生成的CSR。 CA签署CSR后，您会收到已签名的证书文件。

### 查看签名证书

在将已签名证书添加到Cloud Manager之前，请查看该证书。 使用以下命令查看证书详细信息：

```bash
# Review the certificate details
$ openssl crl2pkcs7 -nocrl -certfile <YOUR-SIGNED-CERT>.crt | openssl pkcs7 -print_certs -noout
```

签名证书可以包含证书链，证书链包括根证书和中间证书以及终端实体证书。

AdobeCloud Manager在单独的表单字段&#x200B;_中接受最终实体证书和证书链_，因此您必须从已签名的证书中提取最终实体证书和证书链。

在本教程中，以`*.enablementadobe.com`域颁发的[DigitCert](https://www.digicert.com/)签名证书为例。 通过在文本编辑器中打开签名证书并复制`-----BEGIN CERTIFICATE-----`和`-----END CERTIFICATE-----`标记之间的内容来提取最终实体和证书链。

## 在Cloud Manager中添加SSL证书

>[!VIDEO](https://video.tv.adobe.com/v/3427906?quality=12&learn=on)

要在Cloud Manager中添加SSL证书，请按照[添加SSL证书](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/manage-ssl-certificates/add-ssl-certificate)文档操作。

## 域名验证

>[!VIDEO](https://video.tv.adobe.com/v/3427905?quality=12&learn=on)

要验证域名，请执行以下步骤：

- 按照[添加自定义域名](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/custom-domain-names/add-custom-domain-name)文档，在Cloud Manager中添加域名。
- 在DNS托管服务中添加特定于AEM的[TXT记录](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/custom-domain-names/add-text-record)。
- 通过使用`dig`命令查询DNS服务器来验证上述步骤。

```bash
# General syntax, the `_aemverification` is prefix provided by Adobe
$ dig _aemverification.[YOUR-DOMAIN-NAME] -t txt

# This tutorial specific example, as the subdomain `wknd.enablementadobe.com` is used
$ dig _aemverification.wknd.enablementadobe.com -t txt
```

成功的响应示例如下所示：

```bash
; <<>> DiG 9.10.6 <<>> _aemverification.wknd.enablementadobe.com -t txt
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 8636
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1220
;; QUESTION SECTION:
;_aemverification.wknd.enablementadobe.com. IN TXT

;; ANSWER SECTION:
_aemverification.wknd.enablementadobe.com. 3600    IN TXT "adobe-aem-verification=wknd.enablementadobe.com/105881/991000/bef0e843-9280-4385-9984-357ed9a4217b"

;; Query time: 81 msec
;; SERVER: 153.32.14.247#53(153.32.14.247)
;; WHEN: Tue Mar 12 15:54:25 EDT 2024
;; MSG SIZE  rcvd: 181
```

本教程使用Azure DNS，但可以使用任何DNS提供程序。 要添加TXT记录，您必须按照DNS托管服务的文档进行操作。

查看[检查域名状态](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/custom-domain-names/check-domain-name-status)文档（如果有问题）。

## 配置DNS记录

>[!VIDEO](https://video.tv.adobe.com/v/3427907?quality=12&learn=on)

要为自定义域配置DNS记录，请执行以下步骤：

1. 根据域类型(如根域(APEX)或子域(CNAME))确定DNS记录类型（CNAME或APEX），并按照[配置DNS设置](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/custom-domain-names/configure-dns-settings)文档操作。
1. 在DNS托管服务中添加DNS记录。
1. 按照[检查DNS记录状态](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/custom-domain-names/check-dns-record-status)文档来触发DNS记录验证。

在本教程中，由于使用了&#x200B;**子域** `wknd.enablementadobe.com`，因此添加了指向`cdn.adobeaemcloud.com`的CNAME记录类型。

但是，如果您使用的是&#x200B;**根域**，则必须添加指向Adobe提供的特定IP地址的APEX记录类型（也称为A、ALIAS或ANAME）。

## 站点验证

>[!VIDEO](https://video.tv.adobe.com/v/3427904?quality=12&learn=on)

要验证可使用自定义域名访问站点，请打开Web浏览器并导航到自定义域URL。 确保站点可访问，并且浏览器显示与挂锁图标的安全连接。

## 端到端视频

您还可以观看端到端视频，其中演示了将自定义域名添加到AEM as a Cloud Service托管的站点的概述、先决条件和上述步骤。

>[!VIDEO](https://video.tv.adobe.com/v/3427817?quality=12&learn=on)
