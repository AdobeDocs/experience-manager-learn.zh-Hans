---
title: AEM安全通知（2018年11月）
seo-title: AEM安全通知（2018年11月）
description: AEMExperience Manager安全通知Dispatcher
seo-description: AEMExperience Manager安全通知Dispatcher
version: 6.4
feature: Dispatcher
topics: security
activity: understand
audience: all
doc-type: article
uuid: 3ccf7323-4061-49d7-ae95-eb003099fd77
discoiquuid: 9d181b3e-fbd5-476d-9e97-4452176e495c
topic: 安全
role: Architect
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '440'
ht-degree: 8%

---


# AEM安全通知（2018年11月）

## 摘要

本文介绍了AEM中最近报告的一些最新和旧的漏洞。 请注意，大多数已识别的漏洞是AEM产品的已知问题，且以前已识别出缓解问题，新的调度程序版本可用于这些新漏洞。 Adobe还敦促客户完成[AEM安全检查表](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-checklist.html)并遵循相关准则。

## 需要采取操作

* AEM部署应开始使用最新的Dispatcher版本。
* 必须根据建议的配置应用调度程序安全规则。
* 应为AEM部署完成[AEM安全检查表](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-checklist.html)。

## 漏洞和解决方案

| 带有 OS 剪贴板 | 分辨率 | 链接 |
|-------|------------|-------|
| 绕过AEM Dispatcher规则 | 安装最新版本的Dispatcher(4.3.1)并遵循建议的Dispatcher配置。 | 请参阅[AEM Dispatcher发行说明](https://helpx.adobe.com/experience-manager/dispatcher/release-notes.html)和[配置Dispatcher](https://helpx.adobe.com/cn/experience-manager/dispatcher/using/dispatcher-configuration.html)。 |
| 可用于规避调度程序规则的URL过滤器绕过漏洞 — CVE-2016-0957 | 已在旧版Dispatcher中修复此问题，但现在建议您安装最新版本的Dispatcher(4.3.1)并遵循建议的Dispatcher配置。 | 请参阅[AEM Dispatcher发行说明](https://helpx.adobe.com/experience-manager/dispatcher/release-notes.html)和[配置Dispatcher](https://helpx.adobe.com/experience-manager/dispatcher/using/dispatcher-configuration.html)。 |
| 与存储的SWF文件相关的XSS漏洞 | 已通过之前发布的安全修复来解决此问题。 | 请参阅[AEM安全公告APSB18-10](https://helpx.adobe.com/security/products/experience-manager/apsb18-10.html)。 |
| 与密码相关的漏洞利用 | 遵循安全检查列表中的建议以设置更强密码。 | 请参阅[AEM安全检查列表](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-checklist.html) |
| 匿名用户的磁盘使用暴露 | 此问题已在AEM 6.1及更高版本中解决，对于AEM 6.0，可以对现成的权限进行修改，以增加限制。 | 请参阅[发行说明](https://helpx.adobe.com/cn/experience-manager/aem-previous-versions.html)，了解AEM 6.1及更低版本。 |
| 面向匿名用户的开放式Social代理曝光 | 在从6.0 SP2开始的版本中，此问题已得到解决。 | 请参阅[适用于AEM 6.1及更低版本的发行说明](https://helpx.adobe.com/experience-manager/aem-previous-versions.html)。 |
| 生产实例上的CRX资源管理器访问 | 管理CRX Explorer访问权限已在安全检查列表中涵盖，应从生产作者中删除CRX Explorer并发布，如果未删除，安全运行状况检查会报告它。 | 请参阅[AEM安全检查表](https://helpx.adobe.com/experience-manager/6-4/sites/administering/using/security-checklist.html)。 |
| BGServlet已公开 | 自AEM 6.2以来，此问题已得到解决。 | 请参阅[AEM 6.2发行说明](https://helpx.adobe.com/cn/experience-manager/6-2/release-notes.html) |

>[!MORELIKETHIS]
>
>* [AEM Dispatcher用户指南](https://helpx.adobe.com/experience-manager/dispatcher/user-guide.html)
>* [AEM Dispatcher 发行说明](https://helpx.adobe.com/experience-manager/dispatcher/release-notes.html)
>* [AEM安全公告](https://helpx.adobe.com/security.html#experience-manager)

