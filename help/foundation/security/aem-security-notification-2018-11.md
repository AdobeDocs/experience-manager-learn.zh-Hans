---
title: AEM安全通知（2018年11月）
seo-title: AEM安全通知（2018年11月）
description: AEM Experience Manager Security Notification Dispatcher
seo-description: AEM Experience Manager Security Notification Dispatcher
version: 6.4
feature: dispatcher
topics: security
activity: understand
audience: all
doc-type: article
uuid: 3ccf7323-4061-49d7-ae95-eb003099fd77
discoiquuid: 9d181b3e-fbd5-476d-9e97-4452176e495c
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '439'
ht-degree: 7%

---


# AEM安全通知（2018年11月）

## 摘要

本文解决了AEM中最近报告的几个旧版本的最新漏洞。 请注意，大多数已识别的漏洞是AEM产品的已知问题，且先前已识别出这些漏洞，新的调度程序版本可用于这些新漏洞。 Adobe还敦促客户填写[AEM安全核对清单](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-checklist.html)并遵循相关准则。

## 需要采取操作

* AEM部署应使用最新的Dispatcher版本进行开始。
* 必须根据建议的配置应用调度程序安全规则。
* 应为AEM部署完成[AEM安全核对清单](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-checklist.html)。

## 漏洞和解决方案

| 带有 OS 剪贴板 | 分辨率 | 链接 |
|-------|------------|-------|
| 绕过AEM Dispatcher规则 | 安装最新版Dispatcher(4.3.1)并遵循建议的调度程序配置。 | 请参阅[AEM Dispatcher发行说明](https://helpx.adobe.com/experience-manager/dispatcher/release-notes.html)和[配置Dispatcher](https://helpx.adobe.com/cn/experience-manager/dispatcher/using/dispatcher-configuration.html)。 |
| 可能被用来绕过调度程序规则的URL过滤器绕过漏洞 — CVE-2016-0957 | 这在旧版Dispatcher中已修复，但现在建议您安装最新版Dispatcher(4.3.1)并遵循建议的Dispatcher配置。 | 请参阅[AEM Dispatcher发行说明](https://helpx.adobe.com/experience-manager/dispatcher/release-notes.html)和[配置Dispatcher](https://helpx.adobe.com/experience-manager/dispatcher/using/dispatcher-configuration.html)。 |
| 与存储的SWF文件相关的XSS漏洞 | 此问题已通过先前发布的安全修复解决。 | 请参阅[AEM安全公告APSB18-10](https://helpx.adobe.com/security/products/experience-manager/apsb18-10.html)。 |
| 与密码相关的漏洞利用 | 按照安全核对清单中的建议设置更强的密码。 | 请参阅[AEM安全核对清单](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-checklist.html) |
| 匿名用户的磁盘使用暴露 | 对于AEM 6.1和更高版本，此问题已得到解决，对于AEM 6.0，现成权限可以修改为更具限制性。 | 有关AEM 6.1及更早版本，请参阅[发行说明](https://helpx.adobe.com/cn/experience-manager/aem-previous-versions.html)。 |
| 匿名用户的Open Social代理曝光 | 在从6.0 SP2开始的版本中已解决此问题。 | 请参阅AEM 6.1及更早版本的[发行说明](https://helpx.adobe.com/experience-manager/aem-previous-versions.html)。 |
| 生产实例上的CRX Explorer Access | 安全核对清单中已涵盖管理CRX Explorer访问权限，应从生产作者中删除CRX Explorer并发布，如果未删除，还应报告安全状况检查报告。 | 请参阅[AEM安全清单](https://helpx.adobe.com/experience-manager/6-4/sites/administering/using/security-checklist.html)。 |
| BGServlet已公开 | 自AEM 6.2以来已解决此问题。 | 请参阅[AEM 6.2发行说明](https://helpx.adobe.com/cn/experience-manager/6-2/release-notes.html) |

>[!MORELIKETHIS]
>
>* [AEM Dispatcher用户指南](https://helpx.adobe.com/experience-manager/dispatcher/user-guide.html)
>* [AEM Dispatcher 发行说明](https://helpx.adobe.com/experience-manager/dispatcher/release-notes.html)
>* [AEM安全公告](https://helpx.adobe.com/security.html#experience-manager)

