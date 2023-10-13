---
title: 在Azure存储中存储表单提交
description: 使用REST API在Azure存储中存储表单数据
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2023-08-14T00:00:00Z
kt: 13781
exl-id: 2bec5953-2e0c-4ae6-ae98-34492d4cfbe4
source-git-commit: 097ff8fd0f3a28f3e21c10e03f6dc28695cf9caf
workflow-type: tm+mt
source-wordcount: '453'
ht-degree: 0%

---

# 在Azure存储中存储表单提交

本文说明如何进行REST调用以将提交的AEM Forms数据存储在Azure Storage中。
为了能够在Azure Storage中存储提交的表单数据，必须执行以下步骤。

## 创建Azure存储帐户

[登录您的Azure门户帐户并创建存储帐户](https://learn.microsoft.com/en-us/azure/storage/common/storage-account-create?tabs=azure-portal#create-a-storage-account-1). 为存储帐户提供一个有意义的名称，单击“查看” ，然后单击“创建”。 这将创建具有所有默认值的存储帐户。 出于本文的目的，我们命名了我们的存储帐户 `aemformstutorial`.

## 创建共享访问

我们将使用共享访问签名或SAS授权方法来与Azure存储容器交互。
在门户的存储帐户页面中，单击左侧的共享访问签名菜单项以打开新的共享访问签名密钥设置页面。 确保指定如下面的屏幕快照所示的设置和相应的结束日期，然后单击“生成SAS和连接字符串”按钮。 复制Blob服务SAS url。 我们将使用此URL进行HTTP调用
![共享访问密钥](./assets/shared-access-signature.png)

## 创建容器

接下来，我们需要创建一个容器，用于存储来自表单提交的数据。
在存储帐户页面中，单击左侧的容器菜单项，然后创建一个名为的容器 `formssubmissions`. 确保将公共访问级别设置为私有
![容器](./assets/new-container.png)

## 创建PUT请求

下一步是创建PUT请求，以将提交的表单数据存储在Azure Storage中。 我们必须修改Blob服务SAS URL，以便在URL中包含容器名称和BLOB ID。 每个表单提交都需要使用唯一的BLOB ID进行标识。 唯一BLOB ID通常在代码中创建并插入PUT请求的URL中。
以下是PUT请求的部分URL。 此 `aemformstutorial` 是存储帐户的名称，formsubmissions是将使用唯一BLOB ID存储数据的容器。 URL的其余部分将保持不变。
https://aemformstutorial.blob.core.windows.net/formsubmissions/00cb1a0c-a891-4dd7-9bd2-67a22bef3b8b?...............

以下是使用PUT请求将提交的表单数据存储在Azure Storage中的函数。 请注意是否在URL中使用了容器名称和uuid。 您可以使用下面列出的示例代码创建OSGi服务或Sling Servlet，并将表单提交存储在Azure存储中。

```java
 public String saveFormDatainAzure(String formData) {
        System.out.println("in SaveFormData!!!!!"+formData);
        org.apache.http.impl.client.CloseableHttpClient httpClient = HttpClientBuilder.create().build();
        UUID uuid = UUID.randomUUID();
        
        String url = "https://aemformstutorial.blob.core.windows.net/formsubmissions/"+uuid.toString();
        url = url+"?sv=2022-11-02&ss=bf&srt=o&sp=rwdlaciytfx&se=2024-06-28T00:42:59Z&st=2023-06-27T16:42:59Z&spr=https&sig=v1MR%2FJuhEledioturDFRTd9e2fIDVSGJuAiUt6wNlkLA%3D";
        HttpPut httpPut = new HttpPut(url);
        httpPut.addHeader("x-ms-blob-type","BlockBlob");
        httpPut.addHeader("Content-Type","text/plain");
        try {
            httpPut.setEntity(new StringEntity(formData));
            CloseableHttpResponse response = httpClient.execute(httpPut);
            log.debug("Response code "+response.getStatusLine().getStatusCode());
        } catch (IOException e) {
            log.error("Error: "+e.getMessage());
            throw new RuntimeException(e);
        }
        return uuid.toString();


    }
```

## 验证容器中存储的数据

![form-data-in-container](./assets/form-data-in-container.png)
