---
title: 生成JSON Web令牌和访问令牌
description: 本文说明了生成JWT所需的代码和对Adobe Campaign Standard进行REST调用所需的访问令牌
feature: Adaptive Forms, Form Data Model
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: a5e5aad4-064f-4638-a53a-88dfb1d27c8f
source-git-commit: 38e0332ef2ef45a73a81f318975afc25600392a8
workflow-type: tm+mt
source-wordcount: '243'
ht-degree: 0%

---

# 生成JSON Web令牌和访问令牌 {#generating-json-web-token-and-access-token}

本文说明了生成JWT所需的代码和对Adobe Campaign Standard进行REST调用所需的访问令牌

## 生成JSON Web令牌 {#generate-json-web-token}

使用Adobe Campaign API的第一步是生成JWT。 有关如何为ACS生成JWT的代码示例有很多。 您可以关注此 [Java代码示例](https://github.com/AdobeDocs/adobeio-auth/tree/stage/JWT/samples/adobe-jwt-java) 以生成JWT。

为了将ACS API与AEM Forms结合使用，我们需要在OSGi捆绑包中创建JWT。 以下代码片段用于在此示例OSGI捆绑包中生成JWT。 从如上所示设置的OSGI配置属性中获取有关ACS实例的详细信息。

![配置](assets/campaignconfiguration.gif)

**答：** 此处显示的值是虚值

以下代码从OSGI配置中获取有关Adobe Campaign Server的详细信息。 我们创建一个从80行到104行的私钥。

获得私钥后，我们会创建JSON Web令牌。

```java
package aemformwithcampaign.core.services.impl;
import static io.jsonwebtoken.SignatureAlgorithm.RS256;
import java.io.BufferedInputStream;
import java.io.BufferedReader;
import java.io.ByteArrayOutputStream;
import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
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

import javax.jcr.Node;

import org.apache.commons.compress.utils.IOUtils;
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
import org.json.JSONObject;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import com.mergeandfuse.getserviceuserresolver.GetResolver;

import aemforms.campaign.core.CampaignService;
import aemformwithcampaign.core.*;
import formsandcampaign.demo.CampaignConfigurationService;
import io.jsonwebtoken.Jwts;
@Component(service=CampaignService.class, immediate = true)
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
        log.debug("The API Key i got was "+apikey);
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
            privatekeyString  = privatekeyString.replaceAll("\\n", "").replace("-----BEGIN PRIVATE KEY-----", "").replace("-----END PRIVATE KEY-----", "");
            log.debug("The sanitized private key string is "+privatekeyString);
            // Create the private key
            KeyFactory keyFactory = KeyFactory.getInstance("RSA");
            log.debug("The key factory algorithm is "+keyFactory.getAlgorithm());
            byte []byteArray = privatekeyString.getBytes();
            log.debug("The array length is "+byteArray.length);
            //KeySpec ks = new PKCS8EncodedKeySpec(Base64.getDecoder().decode(keyString));
            byte[] encodedBytes = javax.xml.bind.DatatypeConverter.parseBase64Binary(privatekeyString);
            //KeySpec ks = new PKCS8EncodedKeySpec(byteArray);
            KeySpec ks = new PKCS8EncodedKeySpec(encodedBytes);
            String metascopes[] = new String[]{"ent_campaign_sdk"};
            RSAPrivateKey privateKey = (RSAPrivateKey) keyFactory.generatePrivate(ks);
            HashMap<String, Object> jwtClaims = new HashMap<>();
            jwtClaims.put("iss", orgid);
            jwtClaims.put("sub", techact);
            jwtClaims.put("exp", expirationTime);
            jwtClaims.put("aud", "https://" + AUTH_SERVER_FQDN + "/c/" + apikey);
            //jwtClaims.put("https://" + AUTH_SERVER_FQDN + "/s/" + realm, true);
            for (String metascope : metascopes) {
                jwtClaims.put("https://" + AUTH_SERVER_FQDN + "/s/" + metascope,java.lang.Boolean.TRUE);
            }
            
            // Create the final JWT token
            String jwtToken = Jwts.builder().setClaims(jwtClaims).signWith(RS256, privateKey).compact();
            log.debug("#####The jwtToken is  ####"+jwtToken+"#######");
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


        }
        catch (Exception e)
        {
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
        log.debug("The API Key is "+apikey);
        log.debug("###The Path is "+path);
        HttpHost server = new HttpHost(SERVER_FQDN, 443, "https");
        HttpPost postReq = new HttpPost(path);
        postReq.addHeader("Cache-Control", "no-cache");
        postReq.addHeader("Content-Type", "application/json");
        postReq.addHeader("X-Api-Key", apikey);
        postReq.addHeader("Authorization", "Bearer " + jwtToken);
        StringEntity se = null;
        log.debug("Creating profile for"+profile.toString());
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
    JsonObject responseJson = new JsonParser().parse(EntityUtils.toString(result.getEntity()))
    .getAsJsonObject();
    log.debug("The response on creating profile is " + responseJson.toString());
    return responseJson.get("PKey").getAsString();

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

## 生成访问令牌 {#generate-access-token}

然后，我们通过进行POST调用，将生成的JWT交换为访问令牌。 然后，此访问令牌将作为授权密钥在HTTP标头中发送，以供后续REST调用使用

## 后续步骤

[在ACS中针对表单提交创建配置文件](./parttwo.md)