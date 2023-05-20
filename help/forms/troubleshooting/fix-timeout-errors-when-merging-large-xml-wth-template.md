---
title: 修正將大型xml資料檔案與xdp範本合併時的逾時錯誤
description: 在AEM Forms中將大型xml檔案與範本合併
type: Troubleshooting
role: Admin
level: Intermediate
version: 6.5
feature: Output Service,Forms Service
topic: Administration
kt: 11091
exl-id: 933ec5f6-3e9c-4271-bc35-4ecaf6dbc434
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '185'
ht-degree: 5%

---

# 如何將大型xml資料檔案與xdp範本合併，以建立pdf檔案

將大型xml資料檔案與xdp範本合併時，您可能會在記錄檔中看到以下錯誤

```txt
POST /services/OutputService/GeneratePdfOutput HTTP/1.1] com.adobe.fd.output.internal.exception.OutputServiceException AEM_OUT_001_003:Unexpected Exception: client timeout reached org.omg.CORBA.TIMEOUT: client timeout reached
```

若要修正上述錯誤，請執行下列動作

## 變更aries逾時

* 停止AEM伺服器
* 建立名為的資料夾 **安裝** 在AEM安裝的crx-quickstart資料夾下
* 建立名為的檔案 **org.apache.aries.transaction.config** 安裝資料夾底下具有下列content aries.transaction.timeout=&quot;1200&quot;。 您可以依需求變更逾時值。 逾時值以秒為單位

>[!NOTE]
> 建立org.apache.aries.transaction設定後，您就可以從 [configMgr](http://localhost:4502/system/console/configMgr) 不要編輯檔案


## 變更Jacorb ORB提供者設定

* [開啟OSGi ConfigMgr](http://localhost:4502/system/console/configMgr)
* 搜尋 **Jacorb ORB提供者**
* 新增下列專案jacorb.connection.client.pending_reply_timeout=600000上述設定會將擱置中的回覆逾時（也稱為CORBA使用者端逾時）設定為600秒。
* 儲存您的變更
