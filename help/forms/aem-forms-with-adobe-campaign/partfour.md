---
title: 使用表單資料模型建立Campaign設定檔
description: 使用Adobe Campaign Standard表單資料模型建立AEM Forms設定檔的相關步驟
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 59d5ba6d-91c1-48c7-8c87-8e0caf4f2d7e
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '426'
ht-degree: 3%

---

# 使用表單資料模型建立Campaign設定檔 {#create-campaign-profile-using-form-data-model}

使用Adobe Campaign Standard表單資料模型建立AEM Forms設定檔的相關步驟

## 建立自訂驗證 {#create-custom-authentication}

使用Swagger檔案建立資料來源時，AEM Forms支援下列型別的驗證型別

* 无
* OAuth 2.0
* 基本身份验证
* API 密钥
* 自定义身份验证

![campaignfdm](assets/campaignfdm.gif)

我們必須使用自訂驗證，才能對Adobe Campaign Standard進行REST呼叫。

若要使用自訂驗證，我們必須開發實作IAuthentication介面的OSGi元件

需要實作方法getAuthDetails。 此方法將傳回AuthenticationDetails物件。 此AuthenticationDetails物件將設定進行Adobe Campaign REST API呼叫所需的必要HTTP標頭。

以下是建立自訂驗證時所使用的程式碼。 getAuthDetails方法會完成所有工作。 我們將建立AuthenticationDetails物件。 然後，我們將適當的HttpHeaders新增至此物件並傳回此物件。

```java
package aemfd.campaign.core;

import java.io.IOException;
import java.security.NoSuchAlgorithmException;
import java.security.spec.InvalidKeySpecException;

import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import com.adobe.aemfd.dermis.authentication.api.IAuthentication;
import com.adobe.aemfd.dermis.authentication.exception.AuthenticationException;
import com.adobe.aemfd.dermis.authentication.model.AuthenticationDetails;
import com.adobe.aemfd.dermis.authentication.model.Configuration;

import aemforms.campaign.core.CampaignService;
import formsandcampaign.demo.CampaignConfigurationService;
@Component(service=IAuthentication.class,immediate=true)

public class CampaignAuthentication implements IAuthentication {
 @Reference
 CampaignService campaignService;
  @Reference
     CampaignConfigurationService config;
private Logger log = LoggerFactory.getLogger(CampaignAuthentication.class);
 @Override
 public AuthenticationDetails getAuthDetails(Configuration arg0) throws AuthenticationException {
 try {
   AuthenticationDetails auth = new AuthenticationDetails();
   auth.addHttpHeader("Cache-Control", "no-cache");
   auth.addHttpHeader("Content-Type", "application/json");
   auth.addHttpHeader("X-Api-Key",config.getApiKey() );
         auth.addHttpHeader("Authorization", "Bearer "+campaignService.getAccessToken());
         log.debug("Returning auth");
         return auth;
   
  } catch (NoSuchAlgorithmException e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  } catch (InvalidKeySpecException e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  } catch (IOException e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  }
  return null;
  
 }

 @Override
 public String getAuthenticationType() {
  // TODO Auto-generated method stub
  return "Campaign Custom Authentication";
 }

}
```

## 建立資料來源 {#create-data-source}

第一步是建立swagger檔案。 swagger檔案會定義REST API，此API將用於在Adobe Campaign Standard中建立設定檔。 swagger檔案會定義REST API的輸入引數和輸出引數。

使用swagger檔案建立資料來源。 建立「資料來源」時，您可以指定驗證型別。 在此情況下，我們將使用自訂驗證來針對Adobe Campaign進行驗證。上方列出的程式碼已用於針對Adobe Campaign進行驗證。

在與本文相關的資產中，會提供範例Swagger檔案給您。**請務必變更swagger檔案中的主機和basePath，以符合您的ACS執行個體**

## 測試解決方案 {#test-the-solution}

若要測試解決方案，請遵循下列步驟：
* [請確定您已依照此處所述的步驟進行](aem-forms-with-campaign-standard-getting-started-tutorial.md)
* [下載並解壓縮此檔案以取得swagger檔案](assets/create-acs-profile-swagger-file.zip)
* 使用swagger檔案建立表單資料模型建立資料來源，並將其以上一步驟中建立的資料來源為基礎
* 根據先前步驟中建立的表單資料模型建立最適化表單。
* 將下列元素從資料來源標籤拖放至最適化表單

   * 电子邮件
   * 名字
   * 姓氏
   * 行動電話

* 將提交動作設定為「使用表單資料模型提交」。
* 設定資料模型以適當地提交。
* 預覽表單。 填寫欄位並提交。
* 確認已在Adobe Campaign Standard中建立設定檔。
