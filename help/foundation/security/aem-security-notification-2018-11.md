---
title: AEM安全通知（2018年11月）
seo-title: AEM Security Notification (November 2018)
description: AEMExperience Manager安全通知Dispatcher
seo-description: AEM Experience Manager Security Notification Dispatcher
version: 6.4
feature: Dispatcher
topics: security
activity: understand
audience: all
doc-type: article
uuid: 3ccf7323-4061-49d7-ae95-eb003099fd77
discoiquuid: 9d181b3e-fbd5-476d-9e97-4452176e495c
topic: Security
role: Architect
level: Beginner
exl-id: 1ee11a01-9e49-42f4-aec4-09e51f769f69
source-git-commit: 4b47daf82e27f6bea4be30e3cdd132f497f4c609
workflow-type: tm+mt
source-wordcount: '428'
ht-degree: 14%

---

# AEM安全通知（2018年11月）

## 摘要

本文介绍了最近在AEM中报告的一些新旧漏洞。 请注意，大多数已识别的漏洞是AEM产品的已知问题，并且以前已识别到缓解，新漏洞有可用的新Dispatcher版本。 Adobe还敦促客户完成 [AEM安全核对清单](https://helpx.adobe.com/cn/experience-manager/6-5/sites/administering/using/security-checklist.html) 并遵循相关准则。

## 需要采取操作

* AEM部署应开始使用最新的Dispatcher版本。
* 必须按照建议的配置应用Dispatcher安全规则。
* 此 [AEM安全核对清单](https://helpx.adobe.com/cn/experience-manager/6-5/sites/administering/using/security-checklist.html) 应完成AEM部署。

## 漏洞和解决方案

| 问题 | 解决方法 | 链接 |
|-------|------------|-------|
| 绕过AEM Dispatcher规则 | 安装最新版本的Dispatcher(4.3.1)，并遵循推荐的Dispatcher配置。 | 参见 [AEM Dispatcher发行说明](https://helpx.adobe.com/experience-manager/dispatcher/release-notes.html) 和 [配置Dispatch](https://helpx.adobe.com/cn/experience-manager/dispatcher/using/dispatcher-configuration.html). |
| 用于绕过Dispatcher规则的URL过滤器绕过漏洞 — CVE-2016-0957 | 此问题已在旧版Dispatcher中修复，但现在建议您安装最新版本的Dispatcher (4.3.1)，并遵循推荐的Dispatcher配置。 | 参见 [AEM Dispatcher发行说明](https://helpx.adobe.com/experience-manager/dispatcher/release-notes.html) 和 [配置Dispatch](https://helpx.adobe.com/cn/experience-manager/dispatcher/using/dispatcher-configuration.html). |
| 与存储的SWF文件相关的XSS漏洞 | 之前发布的安全修复程序已解决了此问题。 | 请参阅 [AEM安全公告APSB18-10](https://helpx.adobe.com/security/products/experience-manager/apsb18-10.html). |
| 密码相关漏洞 | 遵循安全检查清单中的建议以获得更安全的密码。 | 参见 [AEM安全核对清单](https://helpx.adobe.com/cn/experience-manager/6-5/sites/administering/using/security-checklist.html) |
| 匿名用户的磁盘使用风险 | 此问题已在AEM 6.1及更高版本中得以解决，对于AEM 6.0，可以将开箱即用权限修改为更具限制性。 | 参见 [发行说明](https://helpx.adobe.com/cn/experience-manager/aem-previous-versions.html)适用于AEM 6.1及更低版本。 |
| 匿名用户公开Open Social Proxy | 从6.0 SP2开始的版本中已解决此问题。 | 参见 [发行说明](https://helpx.adobe.com/cn/experience-manager/aem-previous-versions.html) 适用于AEM 6.1及更低版本。 |
| 在生产实例上访问CRX资源管理器 | 安全检查清单中已涵盖了管理CRX资源管理器访问，应从生产作者和发布中删除CRX资源管理器，并且安全运行状况检查报告未删除的情况。 | 参见 [AEM安全核对清单](https://helpx.adobe.com/experience-manager/6-4/sites/administering/using/security-checklist.html). |
| BGServlet已公开 | 自AEM 6.2起，此问题已得到解决。 | 参见 [AEM 6.2发行说明](https://helpx.adobe.com/cn/experience-manager/6-2/release-notes.html) |

>[!MORELIKETHIS]
>
>* [AEM Dispatcher用户指南](https://helpx.adobe.com/experience-manager/dispatcher/user-guide.html)
>* [AEM Dispatcher 发行说明](https://helpx.adobe.com/experience-manager/dispatcher/release-notes.html)
>* [AEM安全公告](https://helpx.adobe.com/security.html#experience-manager)

