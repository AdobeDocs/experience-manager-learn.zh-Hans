---
title: AEM Forms與Marketo（第2部分）
description: 使用AEM Forms表單資料模型將AEM Forms與Marketo整合的教學課程。
feature: Adaptive Forms, Form Data Model
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: f8ba3d5c-0b9f-4eb7-8609-3e540341d5c2
source-git-commit: 38e0332ef2ef45a73a81f318975afc25600392a8
workflow-type: tm+mt
source-wordcount: '363'
ht-degree: 2%

---

# Marketo驗證服務

Marketo的REST API已使用2-legged OAuth 2.0驗證。我們需要建立自訂驗證，以針對Marketo進行驗證。 此自訂驗證通常會寫入OSGI套件組合中。 下列程式碼顯示本教學課程中使用的自訂驗證器。

## 自訂驗證服務

下列程式碼會建立AuthenticationDetails物件，此物件具有針對Marketo進行驗證所需的access_token

```java
package com.marketoandforms.core;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
 
import com.adobe.aemfd.dermis.authentication.api.IAuthentication;
import com.adobe.aemfd.dermis.authentication.exception.AuthenticationException;
import com.adobe.aemfd.dermis.authentication.model.AuthenticationDetails;
import com.adobe.aemfd.dermis.authentication.model.Configuration;
@Component(service={IAuthentication.class}, immediate=true)
public class MarketoAuthenticationService implements IAuthentication {
@Reference
MarketoService marketoService;
    @Override
    public AuthenticationDetails getAuthDetails(Configuration arg0) throws AuthenticationException
    {
        AuthenticationDetails auth = new AuthenticationDetails();
        auth.addHttpHeader("Cache-Control", "no-cache");
        auth.addHttpHeader("Authorization", "Bearer " + marketoService.getAccessToken());
        return auth
    }
 
    @Override
    public String getAuthenticationType() {
        // TODO Auto-generated method stub
        return "AemForms With Marketo";
    }
}
```

MarketoAuthenticationService會實作IAuthentication介面。 此介面是AEM Forms使用者端SDK的一部分。 此服務取得存取權杖並將權杖插入AuthenticationDetails的HttpHeader。 填入AuthenticationDetails物件的HttpHeaders後，AuthenticationDetails物件會傳回至表單資料模型的Dermis層。

請留意getAuthenticationType方法傳回的字串。 當您設定資料來源時，會使用此字串。

### 取得存取Token

定義簡單介面時，會使用傳回access_token的方法。 實作此介面的類別的程式碼會列在頁面的下方。

```java
package com.marketoandforms.core;
public interface MarketoService {
    String getAccessToken();
}
```

以下程式碼為服務的，其會傳回用於進行REST API呼叫的access_token。 此服務中的程式碼會存取進行GET呼叫所需的設定引數。 如您所見，我們在GETURL中傳遞client_id、client_secret以產生access_token。 然後，會將此access_token傳回至呼叫的應用程式。

```java
package com.marketoandforms.core.impl;
import java.io.IOException;
import org.apache.http.HttpEntity;
import org.apache.http.HttpResponse;
import org.apache.http.ParseException;
import org.apache.http.client.ClientProtocolException;
import org.apache.http.client.HttpClient;
import org.apache.http.client.methods.HttpGet;
import org.apache.http.impl.client.HttpClientBuilder;
import org.apache.http.util.EntityUtils;
import org.json.JSONException;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import com.marketoandforms.core.*; 
@Component(service=MarketoService.class,immediate = true)
public class MarketoServiceImpl implements MarketoService {
    private final Logger log = LoggerFactory.getLogger(getClass());
@Reference
MarketoConfigurationService config;
    @Override
    public String getAccessToken()
    {
        String AUTH_URL = config.getAUTH_URL();
        String CLIENT_ID = config.getCLIENT_ID();
        String CLIENT_SECRET = config.getCLIENT_SECRET();
        String AUTH_PATH = config.getAUTH_PATH();
        HttpClient httpClient = HttpClientBuilder.create().build();
        String getURL = AUTH_URL+AUTH_PATH+"&client_id="+CLIENT_ID+"&client_secret="+CLIENT_SECRET;
        log.debug("The url to get the access token is "+getURL);
        HttpGet httpGet = new HttpGet(getURL);
        httpGet.addHeader("Cache-Control","no-cache");
        try {
            HttpResponse httpResponse = httpClient.execute(httpGet);
            HttpEntity httpEntity = httpResponse.getEntity();
            org.json.JSONObject responseJSON = new org.json.JSONObject(EntityUtils.toString(httpEntity))
            return (String)responseJSON.get("access_token");
        } catch (ClientProtocolException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        } catch (IOException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        } catch (ParseException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        } catch (JSONException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
        return null;
    }
}
```

以下熒幕擷取畫面顯示需要設定的設定屬性。 您可在上述程式碼中讀取這些設定屬性，以取得access_token

![config](assets/configuration-settings.png)

### 配置

下列程式碼已用於建立設定屬性。 這些屬性專用於您的Marketo執行個體

```java
package com.marketoandforms.core;
 
import org.osgi.service.metatype.annotations.AttributeDefinition;
import org.osgi.service.metatype.annotations.ObjectClassDefinition;
 
@ObjectClassDefinition(name="Marketo Credentials Service Configuration", description = "Connect Form With Marketo")
public @interface MarketoConfiguration {
     @AttributeDefinition(name="Identity Endpoint", description="URL of Marketo Identity Endpoint")
     String identityEndpoint() default "";
      @AttributeDefinition(name="Authentication path", description="Marketo authentication path")
      String authPath() default "";
      @AttributeDefinition(name="Client ID", description="Client ID")
      String clientID() default "";
      @AttributeDefinition(name="Client Secret", description="Client Secret")
      String clientSecret() default "";
}
```

下列程式碼會讀取設定屬性，並透過getter方法傳回相同的屬性

```java
package com.marketoandforms.core;
 
import org.osgi.service.component.annotations.Activate;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.metatype.annotations.Designate;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
@Component(immediate=true, service={MarketoConfigurationService.class})
@Designate(ocd=MarketoConfiguration.class)
public class MarketoConfigurationService {
    private final Logger log = LoggerFactory.getLogger(getClass());
    private MarketoConfiguration config;
    private String AUTH_URL;
    private String  AUTH_PATH;
    private String CLIENT_ID ;
    private String CLIENT_SECRET;
     @Activate
      protected final void activate(MarketoConfiguration config) {
        System.out.println("####In my marketo activating auth service");
        AUTH_URL = config.identityEndpoint();
        AUTH_PATH = config.authPath();
        CLIENT_ID = config.clientID();
        CLIENT_SECRET = config.clientSecret();
        log.info("clientID:" + CLIENT_ID);
        System.out.println("The client id is "+CLIENT_ID+"AUTH PATH"+AUTH_PATH);
      }
    public String getAUTH_URL() {
        return AUTH_URL;
    }
   public String getAUTH_PATH() {
        return AUTH_PATH;
    }
    public String getCLIENT_ID() {
        return CLIENT_ID;
    }

    public String getCLIENT_SECRET() {
        return CLIENT_SECRET;
    }
}
```

1. 將套件組合建置並部署至您的AEM伺服器。
1. [將瀏覽器指向configMgr](http://localhost:4502/system/console/configMgr) 和搜尋「Marketo憑證服務設定」
1. 指定您的Marketo執行個體特定的適當屬性

## 后续步骤

[建立以RESTful服務為基礎的資料來源](./part3.md)

