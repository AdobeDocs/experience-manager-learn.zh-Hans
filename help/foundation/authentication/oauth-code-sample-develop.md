---
title: 在AEM中開發OAuth範圍
description: Adobe Experience Manager的可擴充OAuth範圍允許一般使用者授權的使用者端應用程式資源的存取控制。 下圖說明AEM內容中的請求流程。
version: 6.4, 6.5
feature: User and Groups
topic: Development
role: Developer
level: Experienced
exl-id: dd37355e-cfc7-4581-ac22-d89c951c22cf
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '179'
ht-degree: 1%

---

# 開發OAuth範圍

Adobe Experience Manager的可擴充OAuth範圍允許一般使用者授權的使用者端應用程式資源的存取控制。 下圖說明AEM內容中的請求流程。

![Oauth範圍流程](./assets/oauth-code-sample-develop/oauth-scopes-flow.png)

AEM提供三個範圍：

* 配置文件
* 離線存取
* 复制

AEM可延伸的OAuth範圍允許定義其他自訂範圍。 例如，您可以開發自訂範圍並部署至AEM，將透過OAuth授權的行動應用程式限製為讀取而非寫入資產。

OAuth是授權使用者端應用程式的偏好方法，因為它使用存取權杖，而不是要求向該應用程式提供AEM使用者的認證。

* [檢視程式碼](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/legacy/bundle/src/main/java/com/adobe/acs/samples/authentication/oauth/impl/SampleScopeWithPrivileges.java)
