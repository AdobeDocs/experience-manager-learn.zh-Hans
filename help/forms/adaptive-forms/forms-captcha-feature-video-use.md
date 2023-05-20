---
title: 搭配AEM Adaptive Forms使用驗證碼
description: 透過AEM Adaptive Forms新增及使用驗證碼。
feature: Adaptive Forms,Workflow
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 7e5dcc6e-fe56-49af-97e3-7dfaa9c8738f
last-substantial-update: 2019-06-09T00:00:00Z
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '204'
ht-degree: 0%

---

# 搭配AEM Adaptive Forms使用驗證碼{#using-captchas-with-aem-adaptive-forms}

透過AEM Adaptive Forms新增及使用驗證碼。

>[!VIDEO](https://video.tv.adobe.com/v/18336?quality=12&learn=on)

*本影片會逐步解說使用內建AEM CAPTCHA服務及Google reCAPTCHA服務，將CAPTCHA新增至AEM最適化表單的程式。*

>[!NOTE]
>
>此功能僅適用於AEM 6.3以後版本。

>[!NOTE]
>
>**若要在發佈執行個體上設定reCaptcha，請依照步驟操作**
>
>在作者執行個體上設定reCaptach
>
>開啟Felix [網頁主控台](http://localhost:4502/system/console/bundles) 在作者執行個體上
>
>搜尋com.adobe.granite.crypto.file套件組合
>
>記下套件組合ID。 在我的執行個體上，這個數字是20
>
>導覽至作者執行個體上檔案系統上的套件組合ID
>
>* &lt;author-aem-install-dir>/crx-quickstart/launchpad/felix/bundle20/data
* 複製HMAC和主檔案
>
開啟 [felix web主控台](http://localhost:4502/system/console/bundles) 發佈執行個體上的。 搜尋com.adobe.granite.crypto.file套件組合。 記下套件組合ID
導覽至發佈執行個體檔案系統上的套件組合ID
* &lt;publish-aem-install-dir>/crx-quickstart/launchpad/felix/bundle20/data
* 刪除現有的HMAC和主檔案。
* 貼上從製作執行個體複製的HMAC和主檔案
>
重新啟動AEM發佈伺服器

## 支援材料 {#supporting-materials}

* [Google reCAPTCHA](https://www.google.com/recaptcha)
