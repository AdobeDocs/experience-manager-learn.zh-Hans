---
title: 在最適化表單提交時建立Campaign設定檔
description: 本文會說明在Adobe Campaign Standard中建立最適化表單提交設定檔所需的步驟。 此程式會使用自訂提交機制來處理最適化表單提交。
feature: Adaptive Forms, Form Data Model
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: deef09d9-82ec-4e61-b7ee-e72d1cd4e9e0
source-git-commit: 38e0332ef2ef45a73a81f318975afc25600392a8
workflow-type: tm+mt
source-wordcount: '367'
ht-degree: 0%

---

# 在最適化表單提交時建立Campaign設定檔 {#creating-campaign-profile-on-adaptive-form-submission}

本文會說明在Adobe Campaign Standard中建立最適化表單提交設定檔所需的步驟。 此程式會使用自訂提交機制來處理最適化表單提交。

本教學課程將逐步解說在提交最適化表單時建立Campaign設定檔的步驟。 若要完成此使用案例，我們需要執行下列動作

* 建立AEM服務(CampaignService)以使用REST API建立Adobe Campaign Standard設定檔
* 建立自訂提交動作以處理最適化表單提交
* 叫用CampaignService的createProfile方法

## 建立AEM服務 {#create-aem-service}

建立AEM服務以建立Adobe Campaign設定檔。 此AEM服務將從OSGI設定擷取Adobe Campaign認證。 取得行銷活動認證後，系統會產生存取權杖，並使用存取權杖執行HTTP Post呼叫以在Adobe Campaign中建立設定檔。 以下是建立設定檔的程式碼。

```java
package aemformwithcampaign.core.services.impl;

import static io.jsonwebtoken.SignatureAlgorithm.RS256;

import java.io.BufferedInputStream;
import java.io.ByteArrayOutputStream;
import java.io.IOException;
import java.io.InputStream;
import java.io.UnsupportedEncodingException;
import java.security.KeyFactory;
import java.security.NoSuchAlgorithmException;
import java.security.interfaces.RSAPrivateKey;
import java.security.spec.InvalidKeySpecException;
import java.security.spec.KeySpec;
import java.security.spec.PKCS8EncodedKeySpec;
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;

import org.apache.http.Consts;
import org.apache.http.HttpEntity;
import org.apache.http.HttpHost;
import org.apache.http.HttpResponse;
import org.apache.http.NameValuePair;
import org.apache.http.client.ClientProtocolException;
import org.apache.http.client.HttpClient;
import org.apache.http.client.entity.UrlEncodedFormEntity;
import org.apache.http.client.methods.HttpPost;
import org.apache.http.entity.StringEntity;
import org.apache.http.impl.client.HttpClientBuilder;
import org.apache.http.message.BasicNameValuePair;
import org.apache.http.util.EntityUtils;
import org.apache.sling.api.resource.Resource;
import org.apache.sling.api.resource.ResourceResolver;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import com.google.gson.JsonObject;
import com.google.gson.JsonParser;
import com.mergeandfuse.getserviceuserresolver.GetResolver;

import aemforms.campaign.core.CampaignService;
import formsandcampaign.demo.CampaignConfigurationService;
import io.jsonwebtoken.Jwts;

@Component(service = CampaignService.class, immediate = true)
public class CampaignServiceImpl implements CampaignService {
private final Logger log = LoggerFactory.getLogger(getClass());

@Reference
CampaignConfigurationService config;
@Reference
GetResolver getResolver;
private static final String SERVER_FQDN = "mc.adobe.io";
private static final String AUTH_SERVER_FQDN = "ims-na1.adobelogin.com";
private static final String AUTH_ENDPOINT = "/ims/exchange/jwt/";
private static final String CREATE_PROFILE_ENDPOINT = "/campaign/profileAndServicesExt/profile/";

@SuppressWarnings("unused")
@Override
public String getAccessToken() throws IOException, NoSuchAlgorithmException, InvalidKeySpecException {
// TODO Auto-generated method stub
log.info("JWT: Generating Token");

String apikey = config.getApiKey();
log.debug("The API Key i got was " + apikey);
String techact = config.getTechAcct();
String orgid = config.getOrgId();
String clientsecret = config.getClientSecret();
String realm = config.getDomainRealm();

HttpClient httpClient = HttpClientBuilder.create().build();
Long expirationTime = System.currentTimeMillis() / 1000 + 86400L;
try {
ResourceResolver rr = getResolver.getServiceResolver();
Resource privateKeyRes = rr.getResource("/etc/key/campaign/private.key");
InputStream is = privateKeyRes.adaptTo(InputStream.class);
BufferedInputStream bis = new BufferedInputStream(is);
ByteArrayOutputStream buf = new ByteArrayOutputStream();
int result = bis.read();
while (result != -1) {
byte b = (byte) result;
buf.write(b);
result = bis.read();
}

String privatekeyString = buf.toString();
privatekeyString = privatekeyString.replaceAll("\\n", "").replace("-----BEGIN PRIVATE KEY-----", "")
.replace("-----END PRIVATE KEY-----", "");
log.debug("The sanitized private key string is " + privatekeyString);
// Create the private key
KeyFactory keyFactory = KeyFactory.getInstance("RSA");
log.debug("The key factory algorithm is " + keyFactory.getAlgorithm());
byte[] byteArray = privatekeyString.getBytes();
log.debug("The array length is " + byteArray.length);
// KeySpec ks = new
// PKCS8EncodedKeySpec(Base64.getDecoder().decode(keyString));
byte[] encodedBytes = javax.xml.bind.DatatypeConverter.parseBase64Binary(privatekeyString);
// KeySpec ks = new PKCS8EncodedKeySpec(byteArray);
KeySpec ks = new PKCS8EncodedKeySpec(encodedBytes);
String metascopes[] = new String[] { "ent_campaign_sdk" };
RSAPrivateKey privateKey = (RSAPrivateKey) keyFactory.generatePrivate(ks);
HashMap<String, Object> jwtClaims = new HashMap<>();
jwtClaims.put("iss", orgid);
jwtClaims.put("sub", techact);
jwtClaims.put("exp", expirationTime);
jwtClaims.put("aud", "https://" + AUTH_SERVER_FQDN + "/c/" + apikey);
// jwtClaims.put("https://" + AUTH_SERVER_FQDN + "/s/" + realm,
// true);
for (String metascope : metascopes) {
jwtClaims.put("https://" + AUTH_SERVER_FQDN + "/s/" + metascope, java.lang.Boolean.TRUE);
}

// Create the final JWT token
String jwtToken = Jwts.builder().setClaims(jwtClaims).signWith(RS256, privateKey).compact();
log.debug("#####The jwtToken is  ####" + jwtToken + "#######");
HttpHost authServer = new HttpHost(AUTH_SERVER_FQDN, 443, "https");
HttpPost authPostRequest = new HttpPost(AUTH_ENDPOINT);
authPostRequest.addHeader("Cache-Control", "no-cache");
List<NameValuePair> params = new ArrayList<NameValuePair>();
params.add(new BasicNameValuePair("client_id", apikey));
params.add(new BasicNameValuePair("client_secret", clientsecret));
params.add(new BasicNameValuePair("jwt_token", jwtToken));
authPostRequest.setEntity(new UrlEncodedFormEntity(params, Consts.UTF_8));
HttpResponse response = httpClient.execute(authServer, authPostRequest);
if (200 != response.getStatusLine().getStatusCode()) {
HttpEntity ent = response.getEntity();
String content = EntityUtils.toString(ent);
log.error("JWT: Server Returned Error\n", response.getStatusLine().getReasonPhrase());
log.error("ERROR DETAILS: \n", content);
throw new IOException("Server returned error: " + response.getStatusLine().getReasonPhrase());
}
HttpEntity entity = response.getEntity();
JsonObject jo = new JsonParser().parse(EntityUtils.toString(entity)).getAsJsonObject();

log.debug("Returning access_token " + jo.get("access_token").getAsString());
return jo.get("access_token").getAsString();


} catch (Exception e) {
e.printStackTrace();
}
return null;
}

@Override
public String createProfile(JsonObject profile) {
// TODO Auto-generated method stub
String jwtToken = null;
try {
jwtToken = getAccessToken();
} catch (NoSuchAlgorithmException e2) {
// TODO Auto-generated catch block
e2.printStackTrace();
} catch (InvalidKeySpecException e2) {
// TODO Auto-generated catch block
e2.printStackTrace();
} catch (IOException e2) {
// TODO Auto-generated catch block
e2.printStackTrace();
}
String tenant = config.getTenant();
String apikey = config.getApiKey();
String path = "/" + tenant + CREATE_PROFILE_ENDPOINT;
log.debug("The API Key is " + apikey);
log.debug("###The Path is " + path);
HttpHost server = new HttpHost(SERVER_FQDN, 443, "https");
HttpPost postReq = new HttpPost(path);
postReq.addHeader("Cache-Control", "no-cache");
postReq.addHeader("Content-Type", "application/json");
postReq.addHeader("X-Api-Key", apikey);
postReq.addHeader("Authorization", "Bearer " + jwtToken);
StringEntity se = null;
log.debug("Creating profile for" + profile.toString());
try {
se = new StringEntity(profile.toString());

} catch (UnsupportedEncodingException e1) {
// TODO Auto-generated catch block
e1.printStackTrace();
}
postReq.setEntity(se);
HttpClient httpClient = HttpClientBuilder.create().build();
HttpResponse result;
try {
result = httpClient.execute(server, postReq);
// JSONObject responseJson = new
// JSONObject(EntityUtils.toString(result.getEntity()));
JsonObject responseJson = new JsonParser().parse(EntityUtils.toString(result.getEntity()))
.getAsJsonObject();
log.debug("The response on creating profile is " + responseJson.toString());
return responseJson.get("PKey").getAsString();
// return responseJson.getString("PKey");

} catch (ClientProtocolException e) {
// TODO Auto-generated catch block
e.printStackTrace();
} catch (IOException e) {
// TODO Auto-generated catch block
e.printStackTrace();
}

return null;
}

}
```

## 自訂提交 {#custom-submit}

建立自訂提交處理常式來處理最適化表單提交。 在此自訂提交處理常式中，我們將呼叫CampaignService的createProfile方法。 createProfile方法接受代表需要建立之設定檔的JSONObject。

若要進一步瞭解AEM Forms中的自訂提交處理常式，請遵循以下步驟 [連結](/help/forms/adaptive-forms/custom-submit-aem-forms-article.md)

以下是自訂提交中的程式碼

```java
aemforms.campaign.core.CampaignService addNewProfile = sling.getService(aemforms.campaign.core.CampaignService.class);
com.google.gson.JsonObject profile = new com.google.gson.JsonObject();
profile.addProperty("email",request.getParameter("email"));
profile.addProperty("firstName",request.getParameter("fname"));
profile.addProperty("lastName",request.getParameter("lname"));
profile.addProperty("mobilePhone",request.getParameter("phone"));

String pkey = addNewProfile.createProfile(profile);
```

## 測試解決方案 {#test-the-solution}

定義好服務和自訂提交動作後，我們就可以測試解決方案了。 若要測試解決方案，請執行以下步驟


* [請確定您已依照此處所述的步驟進行](aem-forms-with-campaign-standard-getting-started-tutorial.md)
* [使用封裝管理程式匯入最適化表單和自訂提交處理常式](assets/create-acs-profile-on-af-submission.zip).此套件包含已設定為提交至自訂提交動作的最適化表單。
* 預覽 [表單](http://localhost:4502/content/dam/formsanddocuments/createcampaignprofile/jcr:content?wcmmode=disabled)
* 填寫所有欄位並提交
* 在您的ACS執行個體中建立新的設定檔

## 后续步骤

[使用ACS設定檔資訊預先填寫最適化表單](./partthree.md)
