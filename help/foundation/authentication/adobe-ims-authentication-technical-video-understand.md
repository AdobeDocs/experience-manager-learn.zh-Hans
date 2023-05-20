---
title: 瞭解Adobe Managed Services上使用AEM的Adobe IMS驗證
description: Adobe Experience Manager推出適用於AEM執行個體的Admin Console支援，以及適用於Managed Services上AEM的Adobe IMS (Identity Management System)驗證功能。   這項整合可讓AEM Managed Services客戶在單一統一的Web主控台中管理所有Experience Cloud使用者。 可將使用者和群組指派給與AEM執行個體相關聯的產品設定檔，授與對特定AEM執行個體的集中管理存取權。
version: 6.4, 6.5
feature: User and Groups
topics: authentication, security
activity: understand
audience: administrator, architect, developer, implementer
doc-type: technical video
kt: 781
topic: Architecture
role: Architect
level: Experienced
exl-id: 52dd8a3f-6461-4acb-87ca-5dd9567d15a6
last-substantial-update: 2022-10-01T00:00:00Z
thumbnail: KT-781.jpg
source-git-commit: 53af8fbc20ff21abf8778bbc165b5ec7fbdf8c8f
workflow-type: tm+mt
source-wordcount: '448'
ht-degree: 3%

---

# 瞭解Adobe Managed Services上使用AEM的Adobe IMS驗證{#understanding-adobe-ims-authentication-with-aem-on-adobe-managed-services}

Adobe Experience Manager推出AEM例項的Admin Console支援，以及針對Managed Services上的AEMAdobeIdentity Management System (IMS)式驗證。   這項整合可讓AEM Managed Services客戶在單一統一的Web主控台中管理所有Experience Cloud使用者。 可將使用者和群組指派給與AEM執行個體相關聯的產品設定檔，授與對特定AEM執行個體的集中管理存取權。

>[!VIDEO](https://video.tv.adobe.com/v/26170?quality=12&learn=on)

* Adobe Experience Manager IMS驗證支援僅適用於「內部」使用者（作者、檢閱者、管理員、開發人員等），不適用於網站訪客等外部一般使用者。
* [Admin Console](https://adminconsole.adobe.com/) 將AEM Managed Services客戶表示為IMS組織，並將AEM執行個體表示為產品內容。 Admin Console系統和產品管理員可以定義和管理。
* AEM Managed Services會將您的拓撲與Admin Console同步，在產品內容和AEM執行個體之間建立1對1的對應。
* Admin Console中的產品設定檔決定使用者可存取的AEM執行個體。
* 驗證支援包括適用於SSO的客戶SAML2相容IDP。
* 僅支援Enterprise ID或Federated ID （適用於客戶SSO） (不支援個人AdobeID)。

*&#42;Adobe Managed Services客戶的AEM 6.4 SP3及更新版本支援此功能。*

## 最佳實務 {#best-practices}

### 在Admin Console中套用許可權

在Admin Console和Adobe Experience Manager中，都應避免在使用者層級套用許可權和存取權。

在中，應透過「產品內容」層級的「使用者群組」授予Admin Console使用者存取權。 使用者群組通常最好以組織內的邏輯角色表示，以促進群組在Adobe Experience Cloud產品中的重複使用性。

>[!NOTE]
>
> 如果使用AEMas a Cloud Service，請將Admin Console使用者直接指派給產品設定檔。 AEMas a Cloud Service不支援透過Admin Console使用者群組將Admin Console使用者之間的許可權轉換為產品設定檔。

### 在Adobe Experience Manager中套用許可權

在Adobe Experience Manager中，與Adobe IMS同步的使用者群組應以期限新增至 [AEM提供的使用者群組](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security.html)，已預先設定適當的許可權，以便在AEM中執行一組特定工作。 從Adobe IMS同步的使用者不應直接新增至 [AEM提供的使用者群組](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security.html).
