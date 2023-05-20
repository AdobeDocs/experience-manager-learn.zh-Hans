---
title: AEM安全性通知（2018年11月）
seo-title: AEM Security Notification (November 2018)
description: AEMExperience Manager安全性通知Dispatcher
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

# AEM安全性通知（2018年11月）

## 摘要

本文說明AEM最近回報的幾項近期和舊有漏洞。 請注意，最常識別的漏洞是AEM產品的已知問題，而且先前已識別出緩解問題，新的漏洞有新的Dispatcher版本可用。 Adobe也敦促客戶完成 [AEM安全性檢查清單](https://helpx.adobe.com/cn/experience-manager/6-5/sites/administering/using/security-checklist.html) 並遵循相關准則。

## 需要采取操作

* AEM部署應開始使用最新的Dispatcher版本。
* 必須根據建議的設定套用Dispatcher安全性規則。
* 此 [AEM安全性檢查清單](https://helpx.adobe.com/cn/experience-manager/6-5/sites/administering/using/security-checklist.html) 應完成AEM部署。

## 弱點和解決方法

| 问题 | 解决方法 | 链接 |
|-------|------------|-------|
| 略過AEM Dispatcher規則 | 安裝最新版Dispatcher (4.3.1)，並遵循建議的Dispatcher設定。 | 另請參閱 [AEM Dispatcher發行說明](https://helpx.adobe.com/experience-manager/dispatcher/release-notes.html) 和 [設定Dispatcher](https://helpx.adobe.com/cn/experience-manager/dispatcher/using/dispatcher-configuration.html). |
| URL篩選器略過可用來迴避Dispatcher規則的漏洞 — CVE-2016-0957 | 舊版Dispatcher已修正此問題，但現在建議您安裝最新版Dispatcher (4.3.1)，並遵循建議的Dispatcher設定。 | 另請參閱 [AEM Dispatcher發行說明](https://helpx.adobe.com/experience-manager/dispatcher/release-notes.html) 和 [設定Dispatcher](https://helpx.adobe.com/cn/experience-manager/dispatcher/using/dispatcher-configuration.html). |
| 與儲存的SWF檔案相關的XSS漏洞 | 這個問題已透過先前發行的安全性修正解決。 | 請參閱 [AEM安全性公告APSB18-10](https://helpx.adobe.com/security/products/experience-manager/apsb18-10.html). |
| 密碼相關利用漏洞 | 請依照安全性檢查清單中的建議，取得更強大的密碼。 | 另請參閱 [AEM安全性檢查清單](https://helpx.adobe.com/cn/experience-manager/6-5/sites/administering/using/security-checklist.html) |
| 匿名使用者的磁碟使用量暴露 | AEM 6.1及更新版本已解決此問題，AEM 6.0的現成可用許可權可修改為更具限制性。 | 另請參閱 [發行說明](https://helpx.adobe.com/cn/experience-manager/aem-previous-versions.html)適用於AEM 6.1和更舊版本。 |
| 匿名使用者公開Open Social Proxy | 6.0 SP2以上的版本已解決此問題。 | 另請參閱 [發行說明](https://helpx.adobe.com/cn/experience-manager/aem-previous-versions.html) 適用於AEM 6.1和更舊版本。 |
| 在生產執行個體上存取CRX總管 | 管理CRX Explorer存取權已包含在安全性檢查清單中，CRX Explorer應從生產作者和發佈中移除，且安全性健康情況檢查會報告它（如果未移除）。 | 另請參閱 [AEM安全性檢查清單](https://helpx.adobe.com/experience-manager/6-4/sites/administering/using/security-checklist.html). |
| BGServlets已公開 | 自AEM 6.2起，此問題便已解決。 | 另請參閱 [AEM 6.2發行說明](https://helpx.adobe.com/cn/experience-manager/6-2/release-notes.html) |

>[!MORELIKETHIS]
>
>* [AEM Dispatcher使用手冊](https://helpx.adobe.com/experience-manager/dispatcher/user-guide.html)
>* [AEM Dispatcher 发行说明](https://helpx.adobe.com/experience-manager/dispatcher/release-notes.html)
>* [AEM安全性公告](https://helpx.adobe.com/security.html#experience-manager)

