---
title: 表單資料模型的內容感知設定覆寫支援
description: 設定表單資料模型，根據環境與不同端點通訊。
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
kt: 10423
exl-id: 2ce0c07b-1316-4170-a84d-23430437a9cc
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '386'
ht-degree: 10%

---

# 內容感知雲端設定

當您在本機環境中建立雲端設定並在成功測試時，您會想要在上游環境中使用相同的雲端設定，但無須變更端點、秘密金鑰/密碼和/或使用者名稱。 為了達成此使用案例，Cloud Service上的AEM Forms引進了定義上下文感知雲端設定的功能。
例如，Azure儲存體帳戶雲端設定可透過對使用不同的連線字串和金鑰，在開發、暫存和生產環境中重複使用。

需要以下步驟來建立內容感知雲端設定

## 建立環境變數

可以通过 Cloud Manager 配置和管理标准环境变量。 这些变量提供给运行时环境，可以在 OSGi 配置中使用。 [根据所更改的内容，环境变量可以是特定于环境的值或环境密钥。](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/environment-variables.html?lang=en)



以下熒幕擷取畫面顯示定義的azure_key和azure_connection_string環境變數
![environment_variable](assets/environment-variables.png)

這些環境變數隨後可以在要用於適當環境的設定檔案中指定。例如，如果您希望所有作者執行個體都使用這些環境變數，您將在config.author資料夾中定義設定檔案，如下所示

## 建立設定檔

在IntelliJ中開啟您的專案。 導覽至config.author並建立名為的檔案

```java
org.apache.sling.caconfig.impl.override.OsgiConfigurationOverrideProvider-integrationTest.cfg.json
```

![config.author](assets/config-author.png)

將下列文字複製到您在上一步建立的檔案中。 此檔案中的程式碼正在使用環境變數覆寫accountName和accountKey屬性的值 **azure_connection_string** 和 **azure_key**.

```json
{
  "enabled":true,
  "description":"dermisITOverrideConfig",
  "overrides":[
   "cloudconfigs/azurestorage/FormsCSAndAzureBlob/accountName=\"$[env:azure_connection_string]\"",
   "cloudconfigs/azurestorage/FormsCSAndAzureBlob/accountKey=\"$[secret:azure_key]\""

  ]
}
```

>[!NOTE]
>
>此設定將套用至您的雲端服務執行個體中的所有作者環境。 若要將設定套用至發佈環境，您必須將相同的設定檔案置於intelliJ專案的config.publish資料夾中
>[!NOTE]
> 請確定被覆寫的屬性是雲端設定的有效屬性。 導覽至雲端設定，以尋找您要覆寫的屬性，如下所示。

![cloud-config-property](assets/cloud-config-properties.png)

對於具有基本驗證的REST雲端設定，您通常想要為serviceEndPoint、userName和密碼屬性建立環境變數。
