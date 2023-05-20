---
title: 使用ACS設定檔預先填寫最適化表單
description: 使用ACS設定檔預先填寫最適化Forms
feature: Adaptive Forms, Form Data Model
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 502f4bdf-d4af-409f-a611-62b7a1a6065a
source-git-commit: 38e0332ef2ef45a73a81f318975afc25600392a8
workflow-type: tm+mt
source-wordcount: '340'
ht-degree: 1%

---

# 使用ACS設定檔預先填寫最適化表單 {#prefilling-adaptive-form-using-acs-profile}

在本部分中，我們會使用ACS擷取的設定檔資訊預先填寫最適化表單。 AEM Forms具備預先填寫最適化表單的強大功能。

若要進一步瞭解預先填寫最適化表單，請閱讀此 [教學課程](https://helpx.adobe.com/experience-manager/kt/forms/using/prefill-service-adaptive-forms-article-use.html).

若要透過從ACS擷取資料預先填入最適化表單，我們假設ACS中有一個設定檔，其電子郵件與登入的AEM使用者相同。 例如，如果登入AEM的人員的電子郵件ID是csimms@adobe.com ，我們預計會在ACS中找到其電子郵件是csimms@adobe.com的設定檔。

使用REST API從ACS擷取設定檔資訊時，需要執行下列步驟

* 生成 JWT
* 交換JWT以取得存取權杖
* 向ACS發出REST呼叫，並透過電子郵件擷取設定檔
* 使用設定檔資訊建置XML檔案
* 傳回AEM Forms所使用的XML檔案的InputStream

![prefillservice](assets/prefillserviceaf.gif)

將預填服務與最適化表單建立關聯

以下是從ACS擷取和傳回設定檔資訊的程式碼。

在第68行，我們擷取AEM使用者的電子郵件ID。 藉由對Adobe Campaign Standard進行REST呼叫來擷取設定檔詳細資料。 從擷取的設定檔詳細資料中，XML檔案的建構方式可為AEM Forms所瞭解。 傳回此檔案的輸入資料流，供AEM Forms使用。

```java
package aemforms.campaign.core;

import java.io.ByteArrayInputStream;
import java.io.ByteArrayOutputStream;
import java.io.IOException;
import java.io.InputStream;

import javax.jcr.Session;
import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.parsers.ParserConfigurationException;
import javax.xml.transform.TransformerConfigurationException;
import javax.xml.transform.TransformerException;
import javax.xml.transform.TransformerFactory;
import javax.xml.transform.TransformerFactoryConfigurationError;
import javax.xml.transform.dom.DOMSource;
import javax.xml.transform.stream.StreamResult;

import org.apache.http.HttpHost;
import org.apache.http.HttpResponse;
import org.apache.http.client.ClientProtocolException;
import org.apache.http.client.HttpClient;
import org.apache.http.client.methods.HttpGet;
import org.apache.http.impl.client.HttpClientBuilder;
import org.apache.http.util.EntityUtils;
import org.apache.jackrabbit.api.JackrabbitSession;
import org.apache.jackrabbit.api.security.user.Authorizable;
import org.apache.jackrabbit.api.security.user.UserManager;
import org.apache.sling.api.resource.Resource;
import org.apache.sling.api.resource.ResourceResolver;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.w3c.dom.Document;
import org.w3c.dom.Element;

import com.adobe.forms.common.service.DataXMLOptions;
import com.adobe.forms.common.service.DataXMLProvider;
import com.adobe.forms.common.service.FormsException;
import com.google.gson.JsonObject;
import com.google.gson.JsonParser;

import formsandcampaign.demo.CampaignConfigurationService;

@Component
public class PrefillAdaptiveFormWithCampaignProfile implements DataXMLProvider {
private static final Logger log = LoggerFactory.getLogger(PrefillAdaptiveFormWithCampaignProfile.class);
private static final String SERVER_FQDN = "mc.adobe.io";
private static final String ENDPOINT = "/campaign/profileAndServices/profile/byEmail?email=";
@Reference
CampaignService jwtService;
@Reference
CampaignConfigurationService campaignConfig;

Session session = null;

public JsonObject getProfileDetails()
 {
    String jwtToken = null;
    String email = null;
    log.debug("$$$$ in getProfile Details");
    try
       {
          jwtToken = jwtService.getAccessToken();
          UserManager um = ((JackrabbitSession) session).getUserManager();
          Authorizable loggedinUser = um.getAuthorizable(session.getUserID());
          email = loggedinUser.getProperty("profile/email")[0].getString();
          log.debug("####Got email..." + email);
        }
        catch (Exception e)
        {
          log.error("Unable to generate JWT!\n", e);
        }
    String tenant = campaignConfig.getTenant();
    String apikey = campaignConfig.getApiKey();
    String path = "/" + tenant + ENDPOINT + email;
    HttpHost server = new HttpHost(SERVER_FQDN, 443, "https");
    HttpGet getReq = new HttpGet(path);
    getReq.addHeader("Cache-Control", "no-cache");
    getReq.addHeader("Content-Type", "application/json");
    getReq.addHeader("X-Api-Key", apikey);
    getReq.addHeader("Authorization", "Bearer " + jwtToken);
    HttpClient httpClient = HttpClientBuilder.create().build();
    try
       {
          HttpResponse result = httpClient.execute(server, getReq);
          String responseJson = EntityUtils.toString(result.getEntity());
          log.debug("The response Json" + responseJson);
          JsonObject responseJsonProfiles = new JsonParser().parse(responseJson).getAsJsonObject();
          log.debug("The json array is " + responseJsonProfiles.toString());
          return responseJsonProfiles.get("content").getAsJsonArray().get(0).getAsJsonObject();
        }
        catch (ClientProtocolException e)
       {
            e.printStackTrace();
        } catch (IOException e) {

      e.printStackTrace();
}
    return null;

}

public InputStream getDataXMLForDataRef(DataXMLOptions dataXmlOptions) throws FormsException {
// TODO Auto-generated method stub
    log.debug("Geting xml");
    InputStream xmlDataStream = null;
    Resource aemFormContainer = dataXmlOptions.getFormResource();
    ResourceResolver resolver = aemFormContainer.getResourceResolver();
    session = resolver.adaptTo(Session.class);
    JsonObject profile = getProfileDetails();
    log.debug("####profile last name ####" + profile.get("lastName").getAsString());
    try {
          DocumentBuilderFactory docFactory = DocumentBuilderFactory.newInstance();
          DocumentBuilder docBuilder = docFactory.newDocumentBuilder();
          Document doc = docBuilder.newDocument();
          Element rootElement = doc.createElement("data");
          doc.appendChild(rootElement);
          Element firstNameElement = doc.createElement("fname");
          firstNameElement.setTextContent(profile.get("firstName").getAsString());
          log.debug("created firstNameElement  " + profile.get("firstName").getAsString());
          Element lastNameElement = doc.createElement("lname");
          Element jobTitleElement = doc.createElement("jobTitle");
          jobTitleElement.setTextContent(profile.get("salutation").getAsString());
          Element cityElement = doc.createElement("city");
          cityElement.setTextContent(profile.get("location").getAsJsonObject().get("city").getAsString());
          log.debug("created cityElement  " + profile.get("location").getAsJsonObject().get("city").getAsString());
          Element countryElement = doc.createElement("country");
          countryElement.setTextContent(profile.get("location").getAsJsonObject().get("countryCode").getAsString());
          Element streetElement = doc.createElement("street");
          streetElement.setTextContent(profile.get("location").getAsJsonObject().get("address1").getAsString());
          Element postalCodeElement = doc.createElement("postalCode");
          postalCodeElement.setTextContent(profile.get("location").getAsJsonObject().get("zipCode").getAsString());
          Element genderElement = doc.createElement("gender");
          genderElement.setTextContent(profile.get("gender").getAsString());
          lastNameElement.setTextContent(profile.get("lastName").getAsString());
          Element emailElement = doc.createElement("email");
          emailElement.setTextContent(profile.get("email").getAsString());
          rootElement.appendChild(firstNameElement);
          rootElement.appendChild(lastNameElement);
          rootElement.appendChild(emailElement);
          rootElement.appendChild(streetElement);
          rootElement.appendChild(countryElement);
          rootElement.appendChild(cityElement);
          rootElement.appendChild(jobTitleElement);
          rootElement.appendChild(postalCodeElement);
          rootElement.appendChild(genderElement);
          ByteArrayOutputStream outputStream = new ByteArrayOutputStream();
          DOMSource source = new DOMSource(doc);
          StreamResult outputTarget = new StreamResult(outputStream);
          TransformerFactory.newInstance().newTransformer().transform(source, outputTarget);
          xmlDataStream = new ByteArrayInputStream(outputStream.toByteArray());
          return xmlDataStream;

}
    catch (ParserConfigurationException e)
    {
          e.printStackTrace();
    }catch (TransformerConfigurationException e)
    {
        e.printStackTrace();
    } 
    catch (TransformerException e)
   {
        e.printStackTrace();
  } catch (TransformerFactoryConfigurationError e) {
// TODO Auto-generated catch block
e.printStackTrace();
}

return null;
}

@Override
public String getServiceDescription() {

return "Custom Aem Form Pre Fill Service using campaign";
}

@Override
public String getServiceName() {
// TODO Auto-generated method stub
return "Pre Fill Forms Using Campaign Profile";
}

}
```

若要讓此功能在您的系統上運作，請遵循下列指示：

* [請確定您已依照此處所述的步驟進行](aem-forms-with-campaign-standard-getting-started-tutorial.md)
* [使用封裝管理程式將範例最適化表單匯入AEM](assets/pre-fill-af-from-campaign.zip)
* 請務必使用與其電子郵件ID由Adobe Campaign中的設定檔共用的使用者登入AEM。 例如，如果AEM使用者的電子郵件ID是johndoe@adobe.com ，則您需要在ACS中擁有其電子郵件是johndoe@adobe.com的設定檔。
* [預覽表單](http://localhost:4502/content/dam/formsanddocuments/prefillfromcampaign/jcr:content?wcmmode=disabled).

## 后续步骤

[使用表單資料模型建立Adobe Campaign設定檔](./partfour.md)

