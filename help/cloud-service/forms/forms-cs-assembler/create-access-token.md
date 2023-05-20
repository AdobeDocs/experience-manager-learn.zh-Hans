---
title: 建立存取Token
description: 將JSON Web權杖(JWT)與Adobe IMS API交換為AEM存取權杖。
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Output Service
topic: Development
kt: 9980
exl-id: 0e4fd0a0-eaa8-490d-b036-713b25974d60
source-git-commit: e925b9fa02dc8d4695b85377c5f7f43fbd45ebc8
workflow-type: tm+mt
source-wordcount: '86'
ht-degree: 0%

---

# 交換JWT以取得存取權杖


在上一步中建立的JWT會透過Adobe IMS API換取存取權杖，然後可以使用它來存取AEMas a Cloud Service。 若要請求存取權杖，請將包含JWT、client_id、client_secret的POST請求傳送至IMS驗證服務。

下列程式碼已用於產生存取Token的Exchange JWT

```java
public String getAccessToken() {
        
        String jwtToken = getJWTToken();
        GetServiceCredentials getCredentials = new GetServiceCredentials();
        System.out.println("Getting Access Token");

        try {
                HttpClient httpClient = HttpClientBuilder.create().build();
                HttpHost authServer = new HttpHost(getCredentials.getIMS_ENDPOINT(), 443, "https");
                HttpPost authPostRequest = new HttpPost("/ims/exchange/jwt");
                List < NameValuePair > nameValuePairs = new ArrayList < NameValuePair > ();
                nameValuePairs.add(new BasicNameValuePair("jwt_token", jwtToken));
                nameValuePairs.add(new BasicNameValuePair("client_id", getCredentials.getCLIENT_ID()));
                nameValuePairs.add(new BasicNameValuePair("client_secret", getCredentials.getCLIENT_SECRET()));
                authPostRequest.setEntity(new UrlEncodedFormEntity(nameValuePairs, Consts.UTF_8));
                HttpResponse response;
                response = httpClient.execute(authServer, authPostRequest);
                StatusLine statusLine = response.getStatusLine();
                System.out.println("The status code is " + statusLine.getStatusCode());
                HttpEntity result = response.getEntity();
                String jsonResponseStr = EntityUtils.toString(result);
                System.out.println(jsonResponseStr);
                JsonReader jsonReader = new JsonReader(new StringReader(jsonResponseStr));
                JsonObject jsonObject = JsonParser.parseReader(jsonReader).getAsJsonObject();
                
                System.out.println("Returning access_token " + jsonObject.get("access_token").getAsString());
                return jsonObject.get("access_token").getAsString();

        } catch (Exception e) {
                System.out.print("Error: " + e.getMessage());
        }
        return "null";

}
```
