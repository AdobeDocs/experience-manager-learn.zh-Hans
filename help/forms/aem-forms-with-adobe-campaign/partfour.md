---
title: 使用表单数据模型创建Campaign用户档案
description: 使用Adobe Campaign Standard表单数据模型创建AEM Forms用户档案时涉及的步骤
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

# 使用表单数据模型创建Campaign用户档案 {#create-campaign-profile-using-form-data-model}

使用Adobe Campaign Standard表单数据模型创建AEM Forms用户档案时涉及的步骤

## 创建自定义身份验证 {#create-custom-authentication}

使用swagger文件创建数据源时，AEM Forms支持以下类型的身份验证类型

* 无
* OAuth 2.0
* 基本身份验证
* API 密钥
* 自定义身份验证

![campafdm](assets/campaignfdm.gif)

我们必须使用自定义身份验证才能对Adobe Campaign Standard进行REST调用。

要使用自定义身份验证，我们必须开发一个OSGi组件来实现IAuthentication接口

需要实施方法getAuthDetails。 此方法将返回AuthenticationDetails对象。 此AuthenticationDetails对象将设置对Adobe Campaign进行REST API调用所需的HTTP标头。

以下是创建自定义身份验证时使用的代码。 getAuthDetails方法可完成所有工作。 我们创建AuthenticationDetails对象。 然后，我们将相应的HttpHeaders添加到此对象并返回此对象。

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

## 创建数据源 {#create-data-source}

第一步是创建swagger文件。 swagger文件定义将在Adobe Campaign Standard中创建用户档案的REST API。 swagger文件定义REST API的输入参数和输出参数。

使用swagger文件创建数据源。 创建数据源时，您可以指定身份验证类型。 在这种情况下，我们将使用自定义身份验证来针对Adobe Campaign进行身份验证。上面列出的代码用于针对Adobe Campaign进行身份验证。

作为与本文相关的资产的一部分，会向您提供示例swagger文件。**确保更改swagger文件中的主机和basePath以匹配您的ACS实例**

## 测试解决方案 {#test-the-solution}

要测试解决方案，请执行以下步骤：
* [确保已按照此处所述的步骤执行操作](aem-forms-with-campaign-standard-getting-started-tutorial.md)
* [下载并解压缩此文件以获取swagger文件](assets/create-acs-profile-swagger-file.zip)
* 使用Swagger文件创建数据源创建表单数据模型，并将其基于在上一步中创建的数据源
* 根据前面步骤中创建的表单数据模型创建自适应表单。
* 将数据源选项卡中的以下元素拖放到自适应表单上

   * 电子邮件
   * 名字
   * 姓氏
   * 手机

* 将提交操作配置为“使用表单数据模型提交”。
* 配置数据模型以正确提交。
* 预览表单。 填写字段并提交。
* 验证配置文件是否在Adobe Campaign Standard中创建。
