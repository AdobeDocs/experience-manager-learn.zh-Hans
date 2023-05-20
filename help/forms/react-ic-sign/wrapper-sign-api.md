---
title: 建立要呈現給使用者以供簽署的網頁表單
description: 建立AEM套件組合以公開使用案例所需的Acrobat簽署方法。
feature: Adaptive Forms,Acrobat Sign
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
kt: 13099
last-substantial-update: 2023-04-13T00:00:00Z
exl-id: b7ddc0bd-db93-44e0-83cc-bccde604bd91
source-git-commit: 4709035983a5c6705c4e807d877ee71145f48987
workflow-type: tm+mt
source-wordcount: '267'
ht-degree: 0%

---

# 建立Acrobat Sign REST API的包裝函式

已開發自訂AEM套件組合，以建立網頁表單並傳回給一般使用者

* [建立暫時性檔案](https://secure.na1.echosign.com/public/docs/restapi/v6#!/transientDocuments/createTransientDocument). 透過此呼叫上傳的檔案稱為暫時性，因為它僅在上傳後7天內可用。 傳回的暫時性檔案ID可用於需要參考上傳檔案的API呼叫。 暫時性檔案請求是由三個部分組成的多部分請求 — 檔案名稱、MIME型別和檔案資料流。 在此請求中，您一次只能上傳一個檔案。
* [建立網路表單](https://secure.na1.echosign.com/public/docs/restapi/v6#!/widgets/createWidget)這是用來建立新網頁表單的主要端點。 Web表單建立於ACTIVE狀態，以立即裝載該Web表單。
* [擷取網路表單](https://secure.na1.echosign.com/public/docs/restapi/v6#!/widget/getWidget).Retrieve的Web表單使用者。 然後，此網頁表單會呈現給呼叫的應用程式，以供簽署檔案。

## 建立Acrobat Sign OSGi設定

Acrobat Sign REST API需要整合金鑰以及與整合金鑰相關聯的電子郵件。 這兩個值會以OSGi設定屬性的形式提供，如下所示

![簽署設定](assets/sign-configuration.png)

```java
package com.acrobatsign.core.configuration;

import org.osgi.service.metatype.annotations.AttributeDefinition;
import org.osgi.service.metatype.annotations.ObjectClassDefinition;



@ObjectClassDefinition(name="Acrobat Sign Configuration", description = "Acrobat SignConfiguration")
public @interface AcrobatSignConfiguration
{
    @AttributeDefinition(name="Acrobat Sign Integration Key", description = "Integration key you created in Acrobat Sign ")
        String getIntegrationKey();
    
    @AttributeDefinition(name="X-API-USER", description = "X-API-USER")
    String getApiUser();


}
```

```java
package com.acrobatsign.core.configuration;
import org.osgi.service.component.annotations.Activate;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.metatype.annotations.Designate;

@Component(immediate = true, service = AcrobatSignConfigurationService.class)
@Designate(ocd = AcrobatSignConfiguration.class)
public class AcrobatSignConfigurationService {

  private String IntegrationKey;
  private String apiUserEmail;
  public String getIntegrationKey() {
    return IntegrationKey;
  }
  public String getApiUserEmail() {
    return apiUserEmail;
  }

  @Activate
  protected final void activate(AcrobatSignConfiguration config) {
    IntegrationKey = config.getIntegrationKey();

    apiUserEmail = config.getApiUser();
  }

}
```

## 取得暫時性檔案ID

已寫入下列程式碼以建立暫時性檔案

```java
public String getTransientDocumentID(Document documentForSigning) throws IOException {
  String integrationKey = acrobatSignConfig.getIntegrationKey();
  String apiUser = acrobatSignConfig.getApiUserEmail();
  String url = "https://api.na1.adobesign.com/api/rest/v6/transientDocuments";
  MultipartEntityBuilder builder = MultipartEntityBuilder.create();
  org.apache.http.impl.client.CloseableHttpClient httpClient = HttpClientBuilder.create().build();
  HttpPost httpPost = new HttpPost(url);
  httpPost.addHeader("x-api-user", "email:" + apiUser);
  httpPost.addHeader("Authorization", "Bearer " + integrationKey);
  builder.addBinaryBody("File", documentForSigning.getInputStream(), ContentType.DEFAULT_BINARY, "NDA.PDF");
  builder.addTextBody("File-Name", "NDA.pdf");
  HttpEntity entity = builder.build();
  log.debug("Build the entity");
  httpPost.setEntity(entity);
  CloseableHttpResponse response = httpClient.execute(httpPost);
  log.debug("Sent the request!!!!");
  log.debug("REsponse code " + response.getStatusLine().getStatusCode());
  HttpEntity httpEntity = response.getEntity();
  String transientDocumentId = JsonParser.parseString(EntityUtils.toString(httpEntity)).getAsJsonObject().get("transientDocumentId").getAsString();
  log.debug("Transient ID  " + transientDocumentId);
  return transientDocumentId;

}
```

## 取得Widget ID

```java
public String getWidgetID(String transientDocumentID) {
  String integrationKey = acrobatSignConfig.getIntegrationKey();
  String apiUser = acrobatSignConfig.getApiUserEmail();
  String url = "https://api.na1.adobesign.com:443/api/rest/v6/widgets";
  org.apache.http.impl.client.CloseableHttpClient httpClient = HttpClientBuilder.create().build();
  HttpPost httpPost = new HttpPost(url);
  httpPost.addHeader("x-api-user", "email:" + apiUser);
  httpPost.addHeader("Authorization", "Bearer " + integrationKey);

  String jsonREquest = "{\r\n" +
    "  \"fileInfos\": [\r\n" +
    "    {\r\n" +
    "      \"transientDocumentId\": \"a\"\r\n" +
    "    }\r\n" +
    "  ],\r\n" +
    "  \"name\": \"Release and Waiver Agreement\",\r\n" +
    "  \"state\": \"ACTIVE\",\r\n" +
    "  \"widgetParticipantSetInfo\": {\r\n" +
    "    \"memberInfos\": [\r\n" +
    "      {\r\n" +
    "        \"email\": \"\"\r\n" +
    "      }\r\n" +
    "    ],\r\n" +
    "    \"role\": \"SIGNER\"\r\n" +
    "  }\r\n" +
    "}";
  JsonObject jsonReq = JsonParser.parseString(jsonREquest).getAsJsonObject();
  jsonReq.getAsJsonArray("fileInfos").get(0).getAsJsonObject().addProperty("transientDocumentId", transientDocumentID);
  log.debug("The updated json object is " + jsonReq);
  try {
    StringEntity stringEntity = new StringEntity(jsonReq.toString());
    httpPost.setEntity(stringEntity);
    httpPost.addHeader("Content-Type", "application/json");
    CloseableHttpResponse response = httpClient.execute(httpPost);
    log.debug("The response is  " + response.getStatusLine().getStatusCode());
    String widgetID = JsonParser.parseString(EntityUtils.toString(response.getEntity())).getAsJsonObject().get("id").getAsString();
    log.debug("The widget id is " + widgetID);
    return widgetID;
  } catch (Exception e) {
    log.debug("Error in getting Widget ID:" + e.getMessage());

  }
  return null;
}
```

## 取得Widget URL

```java
public String getWidgetURL(String widgetId) throws ClientProtocolException, IOException {
        
        log.debug("$$$$ in get Widget URL for "+widgetId+ "widget id");
        String url = "https://api.na1.adobesign.com:443/api/rest/v6/widgets";
        org.apache.http.impl.client.CloseableHttpClient httpClient = HttpClientBuilder.create().build();
        HttpGet httpGet = new HttpGet(url);
        httpGet.addHeader("x-api-user", "email:"+acrobatSignConfig.getApiUserEmail());
        httpGet.addHeader("Authorization", "Bearer "+acrobatSignConfig.getIntegrationKey());
        CloseableHttpResponse response = httpClient.execute(httpGet);
        JsonObject jsonResponse = JsonParser.parseString(EntityUtils.toString(response.getEntity())).getAsJsonObject();
         log.debug("The json response from get widgets is "+jsonResponse.toString());
         JsonArray userWidgetList = jsonResponse.get("userWidgetList").getAsJsonArray();
         log.debug("The array size is "+userWidgetList.size());
          for(int i=0;i<userWidgetList.size();i++)
         {
        
             log.debug("Getting widget object "+i);
             JsonObject temp = userWidgetList.get(i).getAsJsonObject();
             log.debug("The widget object "+i+"is "+temp.toString());
             if(temp.get("id").getAsString().equalsIgnoreCase(widgetId))
             {
                 log.debug("Bingo found the matching widget id  "+i);
                 String widgetURL = temp.get("url").getAsString();
                 return widgetURL;
                 
             }
            
         }
        
        return null;
        
    }
```

## 后续步骤

[產生Acrobat Sign Widget URL](./create-servlet-to-expose-endpoint.md)
