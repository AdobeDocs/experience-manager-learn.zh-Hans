---
title: 使用表单数据模型创建Campaign配置文件
description: 使用Adobe Campaign Standard表单数据模型创建AEM Forms配置文件所涉及的步骤
feature: Adaptive Forms
version: 6.4,6.5
topic: Integrations, Development
role: Developer
level: Experienced
badgeIntegration: label="集成" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
exl-id: 59d5ba6d-91c1-48c7-8c87-8e0caf4f2d7e
duration: 129
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '431'
ht-degree: 3%

---

# 使用表单数据模型创建Campaign配置文件 {#create-campaign-profile-using-form-data-model}

使用Adobe Campaign Standard表单数据模型创建AEM Forms配置文件所涉及的步骤

## 创建自定义身份验证 {#create-custom-authentication}

使用swagger文件创建数据源时，AEM Forms支持以下类型的身份验证

* 无
* OAuth 2.0
* 基本身份验证
* API 密钥
* 自定义身份验证

![campaignfdm](assets/campaignfdm.gif)

我们必须使用自定义身份验证才能对Adobe Campaign Standard进行REST调用。

要使用自定义身份验证，我们必须开发一个实施IAuthentication接口的OSGi组件

需要实施getAuthDetails方法。 此方法将返回AuthenticationDetails对象。 此AuthenticationDetails对象将具有对Adobe Campaign进行REST API调用所需的HTTP标头集。

以下是创建自定义身份验证时使用的代码。 getAuthDetails方法可完成所有工作。 我们将创建AuthenticationDetails对象。 然后，我们将相应的HttpHeaders添加到此对象并返回此对象。

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

第一步是创建swagger文件。 swagger文件定义将用于在Adobe Campaign Standard中创建配置文件的REST API。 swagger文件定义REST API的输入参数和输出参数。

使用swagger文件创建数据源。 在创建数据源时，可以指定身份验证类型。 在这种情况下，我们将使用自定义身份验证来对Adobe Campaign进行身份验证。上面列出的代码用于对Adobe Campaign进行身份验证。

示例swagger文件将作为与本文相关的资产的一部分提供给您。**确保更改swagger文件中的host和basePath以匹配您的ACS实例**

## 测试解决方案 {#test-the-solution}

要测试解决方案，请执行以下步骤：
* [确保已按照此处所述的步骤进行操作](aem-forms-with-campaign-standard-getting-started-tutorial.md)
* [下载并解压缩此文件以获取swagger文件](assets/create-acs-profile-swagger-file.zip)
* 使用swagger文件创建表单数据模型创建数据源，并将其基于上一步中创建的数据源
* 根据上一步中创建的表单数据模型创建自适应表单。
* 将下列元素从数据源选项卡拖放到自适应表单上

   * 电子邮件
   * 名字
   * 姓氏
   * 手机

* 将提交操作配置为“使用表单数据模型提交”。
* 配置数据模型以正确提交。
* 预览表单。 填写字段并提交。
* 验证是否在Adobe Campaign Standard中创建了配置文件。
