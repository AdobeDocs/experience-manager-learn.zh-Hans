---
title: AEM安全通知（2018年11月）
seo-title: AEM安全通知（2018年11月）
description: AEMExperience Manager安全通知调度程序
seo-description: AEMExperience Manager安全通知调度程序
version: 6.4
feature: dispatcher
topics: security
activity: understand
audience: all
doc-type: article
uuid: 3ccf7323-4061-49d7-ae95-eb003099fd77
discoiquuid: 9d181b3e-fbd5-476d-9e97-4452176e495c
translation-type: tm+mt
source-git-commit: 1e615d1c51fa0c4c0db335607c29a8c284874c8d
workflow-type: tm+mt
source-wordcount: '438'
ht-degree: 5%

---


# AEM安全通知（2018年11月）

## 摘要

本文解决了AEM中最近报告的几个新漏洞和旧漏洞。 请注意，大多数已识别的漏洞是AEM产品的已知问题，并且以前已识别出缓解方法，新的调度程序版本可用于这些新漏洞。 Adobe还敦促客户填写AEM安 [全核对清单](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-checklist.html) ，并遵循相关准则。

## 需要采取操作

* AEM部署应使用最新的Dispatcher版本进行开始。
* 必须根据建议的配置应用调度程序安全规则。
* AEM [安全核对清单](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-checklist.html) (针对AEM部署)应完成。

## 漏洞和解决方案

| 带有 OS 剪贴板 | 分辨率 | 链接 |
|-------|------------|-------|
| 绕过AEM Dispatcher规则 | 安装最新版Dispatcher(4.3.1)并遵循建议的调度程序配置。 | 请参 [阅AEM Dispatcher发行说明](https://helpx.adobe.com/experience-manager/dispatcher/release-notes.html) 和 [配置Dispatcher](https://helpx.adobe.com/cn/experience-manager/dispatcher/using/dispatcher-configuration.html)。 |
| 可用于规避调度程序规则的URL过滤器绕过漏洞- CVE-2016-0957 | 这在旧版Dispatcher中已得到修复，但现在建议您安装最新版本的Dispatcher(4.3.1)，并遵循建议的Dispatcher配置。 | 请参 [阅AEM Dispatcher发行说明](https://helpx.adobe.com/experience-manager/dispatcher/release-notes.html) 和 [配置Dispatcher](https://helpx.adobe.com/cn/experience-manager/dispatcher/using/dispatcher-configuration.html)。 |
| 与存储的SWF文件相关的XSS漏洞 | 此问题已通过先前发布的安全修复解决。 | 请参 [阅AEM安全公告APSB18-10](https://helpx.adobe.com/security/products/experience-manager/apsb18-10.html)。 |
| 密码相关漏洞利用 | 遵循安全检查清单中的建议，确保使用更强的密码。 | 请参 [阅AEM安全核对清单](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-checklist.html) |
| 匿名用户的磁盘使用情况曝光 | 对于AEM 6.1和更高版本，此问题已得到解决，对于AEM 6.0，现成权限可以修改为限制性更强。 | 请参 [阅AEM](https://helpx.adobe.com/experience-manager/aem-previous-versions.html)6.1及更早版本的发行说明。 |
| 匿名用户的开放社交代理曝光 | 在从6.0 SP2开始的版本中已解决此问题。 | 请参 [阅AEM](https://helpx.adobe.com/experience-manager/aem-previous-versions.html) 6.1及更早版本的发行说明。 |
| 生产实例上的CRX Explorer Access | 安全核对清单中已涵盖管理CRX Explorer访问权限，应从生产作者中删除CRX Explorer并发布，如果未删除，安全运行状况检查会报告它。 | 请参 [阅AEM安全核对清单](https://helpx.adobe.com/experience-manager/6-4/sites/administering/using/security-checklist.html)。 |
| BGServlets已公开 | 自AEM 6.2以来已解决此问题。 | See [AEM 6.2 Release Notes](https://helpx.adobe.com/cn/experience-manager/6-2/release-notes.html) |

>[!MORELIKETHIS]
>
>* [AEM Dispatcher用户指南](https://helpx.adobe.com/experience-manager/dispatcher/user-guide.html)
>* [AEM Dispatcher 发行说明](https://helpx.adobe.com/experience-manager/dispatcher/release-notes.html)
>* [AEM安全公告](https://helpx.adobe.com/security.html#experience-manager)

