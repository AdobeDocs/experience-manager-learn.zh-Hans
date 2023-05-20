---
title: AEM Assets中的封閉式使用者群組
description: 封閉式使用者群組(CUG)是一項功能，用來將內容的存取許可權製為已發佈網站上的選定使用者群組。 此影片說明如何將封閉式使用者群組與Adobe Experience Manager資產搭配使用，以限制對資產的特定資料夾的存取權。
version: 6.4, 6.5, Cloud Service
topic: Administration, Security
feature: User and Groups
role: Admin
level: Intermediate
kt: 649
thumbnail: 22155.jpg
last-substantial-update: 2022-06-06T00:00:00Z
exl-id: a2bf8a82-15ee-478c-b7c3-de8a991dfeb8
source-git-commit: f37483f90f2a707c906e1e206795fdebb5f698e9
workflow-type: tm+mt
source-wordcount: '377'
ht-degree: 0%

---

# 已關閉的使用者群組{#using-closed-user-groups-with-aem-assets}

封閉式使用者群組(CUG)是一項功能，用來將內容的存取許可權製為已發佈網站上的選定使用者群組。 此影片說明如何將封閉式使用者群組與Adobe Experience Manager資產搭配使用，以限制對資產的特定資料夾的存取權。 AEM 6.4首先推出了透過AEM Assets支援封閉式使用者群組。

>[!VIDEO](https://video.tv.adobe.com/v/22155?quality=12&learn=on)

## 具AEM Assets的封閉式使用者群組(CUG)

* 旨在限制對AEM Publish執行個體上資產的存取。
* 授予一組使用者/群組的讀取存取權。
* CUG只能在資料夾層級設定。 無法在個別資產上設定CUG。
* 任何子資料夾和套用的資產都會自動繼承CUG原則。
* 藉由設定新的CUG原則，子資料夾可以覆寫CUG原則。 請謹慎使用，這並非最佳作法。

## 已關閉的使用者群組與存取控制清單 {#closed-user-groups-vs-access-control-lists}

封閉式使用者群組(CUG)和存取控制清單(ACL)都是用來根據AEM Security使用者和群組來控制對AEM中內容的存取。 不過，這些功能的應用與實作差異極大。 下表總結了這兩項功能之間的差異。

|  | ACL | CUG |
| ----------------- | -------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| 預期用途 | 為上的內容設定並套用許可權 **目前** AEM執行個體。 | 為AEM上的內容設定CUG原則 **作者** 執行個體。 針對AEM上的內容套用CUG原則 **發佈** 執行個體。 |
| 許可權層級 | 為所有層級定義使用者/群組授予/拒絕的許可權：讀取、修改、建立、刪除、讀取ACL、編輯ACL、復寫。 | 授予一組使用者/群組的讀取存取權。 拒絕對的讀取存取權 *所有其他* 使用者/群組。 |
| 发布 | ACL是 *not* 已隨內容發佈。 | CUG原則 *是* 已隨內容發佈。 |

## 支援連結 {#supporting-links}

* [管理資產和封閉式使用者群組](https://experienceleague.adobe.com/docs/experience-manager-65/assets/managing/manage-assets.html?lang=en#closed-user-group)
* [建立已關閉的使用者群組](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/cug.html)
* [Oak已關閉使用者群組檔案](https://jackrabbit.apache.org/oak/docs/security/authorization/cug.html)
