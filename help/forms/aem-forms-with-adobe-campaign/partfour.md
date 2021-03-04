---
title: 使用表单用户档案模型创建活动
seo-title: 使用表单用户档案模型创建活动
description: 使用Adobe Campaign Standard表单用户档案模型创建AEM Forms Form数据涉及的步骤
seo-description: 使用Adobe Campaign Standard表单用户档案模型创建AEM Forms Form数据涉及的步骤
uuid: 3216827e-e1a2-4203-8fe3-4e2a82ad180a
feature: 输出服务
topics: integrations
audience: developer
doc-type: tutorial
activity: setup
version: 6.3,6.4,6.5
discoiquuid: 461c532e-7a07-49f5-90b7-ad0dcde40984
topic: 开发
role: 开发人员
level: 富有经验
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '452'
ht-degree: 3%

---


# 使用表单用户档案模型{#create-campaign-profile-using-form-data-model}创建活动

使用Adobe Campaign Standard表单用户档案模型创建AEM Forms Form数据涉及的步骤

## 创建自定义身份验证{#create-custom-authentication}

使用Swagger文件创建数据源时，AEM Forms支持以下类型的身份验证

* 无
* OAuth 2.0
* 基本身份验证
* API 密钥
* 自定义身份验证

![campafdm](assets/campaignfdm.gif)

我们必须使用自定义身份验证才能向Adobe Campaign Standard发出REST调用。

要使用自定义身份验证，我们必须开发一个OSGi组件来实现IAuthentication接口

需要实现方法getAuthDetails。 此方法将返回AuthenticationDetails对象。 此AuthenticationDetails对象将设置进行REST API调用Adobe Campaign所需的HTTP头。

以下是用于创建自定义身份验证的代码。 方法getAuthDetails可完成所有工作。 我们创建AuthenticationDetails对象。 然后，我们将相应的HttpHeaders添加到此对象并返回此对象。

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

## 创建数据源{#create-data-source}

第一步是创建Swagger文件。 Swagger文件定义将在Adobe Campaign Standard中用于创建用户档案的REST API。 Swagger文件定义REST API的输入参数和输出参数。

将使用Swagger文件创建数据源。 创建数据源时，可以指定身份验证类型。 在这种情况下，我们将使用自定义身份验证来针对Adobe Campaign进行身份验证。上面列出的代码用于针对Adobe Campaign进行身份验证。

示例Swagger文件作为与本文相关的资产的一部分提供给您。**确保更改swagger文件中的host和basePath以匹配您的ACS实例**

## 测试解决方案{#test-the-solution}

要测试解决方案，请执行以下步骤：
* [确保您遵循了此处所述的步骤](aem-forms-with-campaign-standard-getting-started-tutorial.md)
* [下载并解压缩此文件以获取Swagger文件](assets/create-acs-profile-swagger-file.zip)
* 使用Swagger文件创建数据源
创建表单数据模型，并基于在上一步中创建的数据源
* 根据在前一步中创建的表单数据模型创建自适应表单。
* 将以下元素从“数据源”选项卡拖放到自适应表单

   * 电子邮件
   * 名字
   * 姓氏
   * 手机

* 将提交操作配置为“使用表单数据模型提交”。
* 配置数据模型以正确提交。
* 预览表单。 填写字段并提交。
* 验证用户档案是否在Adobe Campaign Standard中创建。
