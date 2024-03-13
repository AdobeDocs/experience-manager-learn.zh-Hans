---
title: 添加自定义域名
description: 了解如何将自定义域名添加到AEM as a Cloud Service托管的站点。
version: Cloud Service
feature: Cloud Manager, Operations
topic: Administration, Architecture
role: Admin, Developer
level: Intermediate
doc-type: Tutorial
duration: null
last-substantial-update: 2024-03-12T00:00:00Z
jira: KT-15121
thumbnail: KT-15121.jpeg
source-git-commit: 466a19a30dd5f81d50c28cb57034800494255d4b
workflow-type: tm+mt
source-wordcount: '669'
ht-degree: 0%

---


# 添加自定义域名

了解如何将自定义域名添加到AEMas a Cloud Service的网站。

在本教程中，示例的品牌化 [AEM WKND](https://github.com/adobe/aem-guides-wknd) 站点通过添加HTTPS可寻址自定义域名而得到增强 `wknd.enablementadobe.com` 具有传输层安全性(TLS)。

>[!VIDEO](https://video.tv.adobe.com/v/3427817?quality=12&learn=on)

高级步骤包括：

![高自定义域名](./assets/add-custom-domain-name-steps.png){width="800" zoomable="yes"}

## 先决条件

- [OpenSSL](https://www.openssl.org/) 和 [挖掘](https://www.isc.org/blogs/dns-checker/) 已安装在本地计算机上。
- 访问第三方服务：
   - 证书颁发机构(CA) — 为您的站点域请求已签名的证书，如 [数字证书](https://www.digicert.com/)
   - 域名系统(DNS)托管服务 — 为您的自定义域添加DNS记录，如Azure DNS或AWS Route 53。
- 访问 [AdobeCloud Manager](https://my.cloudmanager.adobe.com/) 作为业务负责人或部署管理员角色。
- 示例 [AEM WKND](https://github.com/adobe/aem-guides-wknd) 站点部署到的AEMCS环境 [生产程序](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-production-programs.html) 类型。

如果您无权访问第三方服务， _与您的安全或托管团队协作以完成这些步骤_.

## 生成SSL证书

您有两个选项：

- 使用 `openssl` 命令行工具 — 您可以为站点域生成私钥和证书签名请求(CSR)。 要请求已签名的证书，请将CSR提交到证书颁发机构(CA)。

- 您的托管团队为您的站点提供所需的私钥和签名证书。

让我们回顾一下第一个选项的步骤。

要生成私钥和CSR，请运行以下命令并在出现提示时提供所需信息：

```bash
# Generate a private key and a CSR
$ openssl req -newkey rsa:2048 -keyout <YOUR-SITE-NAME>.key -out <YOUR-SITE-NAME>.csr -nodes
```

要请求已签名的证书，请按照相应文档向CA提供生成的CSR。 CA签署CSR后，您会收到已签名的证书文件。

### 查看签名证书

好的做法是在将签名证书添加到Cloud Manager之前对其进行审查。 您可以使用以下命令查看证书详细信息：

```bash
# Review the certificate details
$ openssl crl2pkcs7 -nocrl -certfile <YOUR-SIGNED-CERT>.crt | openssl pkcs7 -print_certs -noout
```

签名证书可以包含证书链，证书链包括根证书和中间证书以及终端实体证书。

AdobeCloud Manager接受最终实体证书和证书链 _在单独的表单字段中_，因此您必须从已签名的证书中提取最终实体证书和证书链。

在本教程中， [数字证书](https://www.digicert.com/) 签发的证书 `*.enablementadobe.com` 域为例。 通过在文本编辑器中打开签名证书并在证书链之间复制内容，提取最终实体和证书链。 `-----BEGIN CERTIFICATE-----` 和 `-----END CERTIFICATE-----` 标记。

## 在Cloud Manager中添加SSL证书

要在Cloud Manager中添加SSL证书，请按照 [添加SSL证书](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/manage-ssl-certificates/add-ssl-certificate.html) 文档。

## 域名验证

要验证域名，请执行以下步骤：

- 按照以下步骤在Cloud Manager中添加域名 [添加自定义域名](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/custom-domain-names/add-custom-domain-name.html) 文档。
- 添加特定于AEM [TXT记录](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/custom-domain-names/add-text-record.html) 在您的DNS托管服务中。
- 通过使用查询DNS服务器来验证上述步骤 `dig` 命令。

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

在本教程中，以Azure DNS为例。 要添加TXT记录，您必须按照DNS托管服务的文档进行操作。

查看 [检查域名状态](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/custom-domain-names/check-domain-name-status.html) 文档。

## 配置DNS记录

要为自定义域配置DNS记录，请执行以下步骤，

- 根据域类型(如根域(APEX)或子域(CNAME))确定DNS记录类型（CNAME或APEX），并遵循 [配置DNS设置](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/custom-domain-names/configure-dns-settings.html) 文档。
- 在DNS托管服务中添加DNS记录。
- 按照以下步骤触发DNS记录验证 [检查DNS记录状态](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/custom-domain-names/check-dns-record-status.html) 文档。

在本教程中， as a **子域** `wknd.enablementadobe.com` 使用，指向的CNAME记录类型 `cdn.adobeaemcloud.com` 添加了。

但是，如果您使用 **根域**，则必须添加指向Adobe提供的特定IP地址的APEX记录类型（也称为A、ALIAS或ANAME）。

## 站点验证

要验证可使用自定义域名访问站点，请打开Web浏览器并导航到自定义域URL。 确保站点可访问，并且浏览器显示与挂锁图标的安全连接。


