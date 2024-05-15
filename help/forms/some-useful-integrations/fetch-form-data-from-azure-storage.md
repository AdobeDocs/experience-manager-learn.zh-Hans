---
title: 在Azure存储中存储表单提交
description: 使用REST API在Azure存储中存储表单数据
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2023-10-23T00:00:00Z
jira: KT-14238
duration: 78
exl-id: 77f93aad-0cab-4e52-b0fd-ae5af23a13d0
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '286'
ht-degree: 0%

---

# 从Azure存储获取数据

本文说明如何使用存储在Azure存储中的数据填充自适应表单。
假设您已将自适应表单数据存储在Azure存储中，现在希望使用该数据预填充自适应表单。
>[!NOTE]
>本文中的代码不适用于基于核心组件的自适应表单。[此处提供了基于核心组件的自适应表单的等效文章](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/prefill-form-with-data-attachments/introduction.html?lang=en)


## 创建GET请求

下一步是使用blobID编写代码以从Azure存储中提取数据。 编写了以下代码以提取数据。 URL是使用OSGi配置中的sasToken和storageURI值以及传递给getBlobData函数的blobID构建的

```java
 @Override
public String getBlobData(String blobID) {
    String sasToken = azurePortalConfigurationService.getSASToken();
    String storageURI = azurePortalConfigurationService.getStorageURI();
    log.debug("The SAS Token is " + sasToken);
    log.debug("The Storage URL is " + storageURI);
    String httpGetURL = storageURI + blobID;
    httpGetURL = httpGetURL + sasToken;
    HttpGet httpGet = new HttpGet(httpGetURL);

    org.apache.http.impl.client.CloseableHttpClient httpClient = HttpClientBuilder.create().build();
    CloseableHttpResponse httpResponse = null;
    try {
        httpResponse = httpClient.execute(httpGet);
        HttpEntity httpEntity = httpResponse.getEntity();
        String blobData = EntityUtils.toString(httpEntity);
        log.debug("The blob data I got was " + blobData);
        return blobData;

    } catch (ClientProtocolException e) {

        log.debug("Got Client Protocol Exception " + e.getMessage());
    } catch (IOException e) {

        log.debug("Got IOEXception " + e.getMessage());
    }

    return null;
}
```

在URL中使用guid参数呈现自适应表单时，与模板关联的自定义页面组件会获取并使用Azure存储中的数据填充自适应表单。
以下是与该模板关联的页面组件jsp中的代码

```java
com.aemforms.saveandfetchfromazure.StoreAndFetchDataFromAzureStorage azureStorage = sling.getService(com.aemforms.saveandfetchfromazure.StoreAndFetchDataFromAzureStorage.class);


String guid = request.getParameter("guid");

if(guid!=null&&!guid.isEmpty())
{
    String dataXml = azureStorage.getBlobData(guid);
    slingRequest.setAttribute("data",dataXml);

}
```

## 测试解决方案

* [部署自定义OSGi捆绑包](./assets/SaveAndFetchFromAzure.core-1.0.0-SNAPSHOT.jar)

* [导入自定义自适应表单模板以及与模板关联的页面组件](./assets/store-and-fetch-from-azure.zip)

* [导入自适应表单示例](./assets/bank-account-sample-form.zip)

* [使用OSGi配置控制台在Azure门户配置中指定适当的值。](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/some-useful-integrations/store-form-data-in-azure-storage.html?lang=en#provide-the-blob-sas-token-and-storage-uri)

* [预览和提交BankAccount表单](http://localhost:4502/content/dam/formsanddocuments/azureportalstorage/bankaccount/jcr:content?wcmmode=disabled)

* 验证数据是否存储在您选择的Azure存储容器中。 复制Blob ID。

* [预览BankAccount表单](http://localhost:4502/content/dam/formsanddocuments/azureportalstorage/bankaccount/jcr:content?wcmmode=disabled&amp;guid=dba8ac0b-8be6-41f2-9929-54f627a649f6) 并将Blob ID指定为URL中的guid参数，以便使用Azure存储中的数据预填充表单
