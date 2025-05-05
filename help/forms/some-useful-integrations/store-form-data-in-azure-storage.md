---
title: 在Azure存储中存储表单提交
description: 使用REST API在Azure存储中存储表单数据
feature: Adaptive Forms
version: Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2023-08-14T00:00:00Z
jira: KT-13781
exl-id: 2bec5953-2e0c-4ae6-ae98-34492d4cfbe4
duration: 143
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '601'
ht-degree: 0%

---

# 在Azure存储中存储表单提交

本文说明如何进行REST调用以将提交的AEM Forms数据存储在Azure Storage中。
为了能够在Azure Storage中存储提交的表单数据，必须执行以下步骤。

>[!NOTE]
>本文中的代码不适用于基于核心组件的自适应表单。 [此处提供了基于核心组件的自适应表单的等效文章](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/prefill-form-with-data-attachments/introduction.html?lang=en)


## 创建Azure存储帐户

[登录到您的Azure门户帐户并创建存储帐户](https://learn.microsoft.com/en-us/azure/storage/common/storage-account-create?tabs=azure-portal#create-a-storage-account-1)。 为存储帐户提供一个有意义的名称，单击“查看” ，然后单击“创建”。 这将创建具有所有默认值的存储帐户。 为撰写本文的目的，我们命名了存储帐户`aemformstutorial`。


## 创建容器

接下来，我们需要创建一个容器，用于存储来自表单提交的数据。
在存储帐户页面中，单击左侧的“容器”菜单项，然后创建一个名为`formssubmissions`的容器。 确保将公共访问级别设置为私有
![容器](./assets/new-container.png)

## 在容器上创建SAS

我们将使用共享访问签名或SAS授权方法来与Azure存储容器交互。
导航到存储帐户中的容器，单击省略号并选择生成SAS选项，如屏幕快照中所示
![容器上的sas](./assets/sas-on-container.png)
请确保指定如下面的屏幕快照所示的适当权限和适当结束日期，然后单击生成SAS令牌和URL。 复制Blob SAS令牌和Blob SAS URL。 我们将使用这两个值来进行HTTP调用
![共享访问密钥](./assets/shared-access-signature.png)


## 提供Blob SAS令牌和存储URI

为了使代码更通用，可以使用OSGi配置来配置这两个属性，如下所示。 _&#x200B;**aemformstutorial**&#x200B;_&#x200B;是存储帐户的名称，_&#x200B;**formsubmissions**&#x200B;_是将存储数据的容器。
请确保存储URI末尾具有/，并且SAS令牌的开头为？
![osgi-configuration](./assets/azure-portal-osgi-configuration.png)


## 创建PUT请求

下一步是创建PUT请求，以将提交的表单数据存储在Azure Storage中。 每个表单提交都需要使用唯一的BLOB ID进行标识。 唯一BLOB ID通常会在您的代码中创建并插入PUT请求的URL中。
以下是PUT请求的部分URL。 `aemformstutorial`是存储帐户的名称，formsubmissions是将使用唯一BLOB ID存储数据的容器。 URL的其余部分将保持不变。
https://aemformstutorial.blob.core.windows.net/formsubmissions/blobid/sastoken
以下是使用PUT请求将提交的表单数据存储在Azure Storage中的函数。 请注意是否在URL中使用了容器名称和uuid。 您可以使用下面列出的示例代码创建OSGi服务或Sling Servlet，并将表单提交存储在Azure存储中。

```java
 public String saveFormDatainAzure(String formData) {
    log.debug("in SaveFormData!!!!!" + formData);
    String sasToken = azurePortalConfigurationService.getSASToken();
    String storageURI = azurePortalConfigurationService.getStorageURI();
    log.debug("The SAS Token is " + sasToken);
    log.debug("The Storage URL is " + storageURI);
    org.apache.http.impl.client.CloseableHttpClient httpClient = HttpClientBuilder.create().build();
    UUID uuid = UUID.randomUUID();
    String putRequestURL = storageURI + uuid.toString();
    putRequestURL = putRequestURL + sasToken;
    HttpPut httpPut = new HttpPut(putRequestURL);
    httpPut.addHeader("x-ms-blob-type", "BlockBlob");
    httpPut.addHeader("Content-Type", "text/plain");

    try {
        httpPut.setEntity(new StringEntity(formData));

        CloseableHttpResponse response = httpClient.execute(httpPut);
        log.debug("Response code " + response.getStatusLine().getStatusCode());
        if (response.getStatusLine().getStatusCode() == 201) {
            return uuid.toString();
        }
    } catch (IOException e) {
        log.error("Error: " + e.getMessage());
        throw new RuntimeException(e);
    }
    return null;

}
```

## 验证容器中存储的数据

![form-data-in-container](./assets/form-data-in-container.png)

## 测试解决方案

* [部署自定义OSGi捆绑包](./assets/SaveAndFetchFromAzure.core-1.0.0-SNAPSHOT.jar)

* [导入自定义自适应表单模板以及与模板关联的页面组件](./assets/store-and-fetch-from-azure.zip)

* [导入自适应表单示例](./assets/bank-account-sample-form.zip)

* [使用OSGi配置控制台在Azure Portal配置中指定适当的值](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/some-useful-integrations/store-form-data-in-azure-storage.html?lang=en#provide-the-blob-sas-token-and-storage-uri)

* [预览并提交银行帐户表单](http://localhost:4502/content/dam/formsanddocuments/azureportalstorage/bankaccount/jcr:content?wcmmode=disabled)

* 验证数据是否存储在您选择的Azure存储容器中。 复制Blob ID。
* [预览BankAccount表单](http://localhost:4502/content/dam/formsanddocuments/azureportalstorage/bankaccount/jcr:content?wcmmode=disabled&amp;guid=dba8ac0b-8be6-41f2-9929-54f627a649f6)，并将Blob ID指定为URL中的guid参数，该表单将使用Azure存储中的数据预填充

