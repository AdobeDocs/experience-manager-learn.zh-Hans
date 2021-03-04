---
title: 在AEM Forms中与服务用户一起开发
seo-title: 在AEM Forms中与服务用户一起开发
description: 本文将指导您完成在AEM Forms中创建服务用户的过程
seo-description: 本文将指导您完成在AEM Forms中创建服务用户的过程
uuid: 996f30df-3fc5-4232-a104-b92e1bee4713
feature: 自适应表单
topics: development,administration
audience: implementer,developer
doc-type: article
activity: setup
discoiquuid: 65bd4695-e110-48ba-80ec-2d36bc53ead2
topic: 开发
role: 开发人员
level: 富有经验
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '451'
ht-degree: 1%

---


# 在AEM Forms中与服务用户一起开发

本文将指导您完成在AEM Forms中创建服务用户的过程

在Adobe Experience Manager(AEM)的早期版本中，管理资源解析程序用于后端处理，后端处理需要访问存储库。 AEM 6.3中已弃用管理资源解析程序的使用。而是使用存储库中具有特定权限的系统用户。

本文将介绍如何创建系统用户并配置用户映射器属性。

1. 导航到[http://localhost:4502/crx/explorer/index.jsp](http://localhost:4502/crx/explorer/index.jsp)
1. 以“ admin ”登录
1. 单击“用户管理”
1. 单击“创建系统用户”
1. 将用户ID类型设置为“ data ”，然后单击绿色图标以完成创建系统用户的过程
1. [打开configMgr](http://localhost:4502/system/console/configMgr)
1. 搜索“ Apache Sling Service用户映射器服务”并单击以打开属性
1. 单击&#x200B;*+*&#x200B;图标（加号）以添加以下服务映射

   * DevelopingWithServiceUser.core:getresourceresolver=data
   * DevelopingWithServiceUser.core:getformsresourceresolver=fd-service

1. 单击“保存”

在上述配置设置中，DevelopingWithServiceUser.core是捆绑包的符号名。 getresourceresolver是子服务名。data是在上一步中创建的系统用户。

我们还可以代表fd-service用户获取资源解析器。 此服务用户用于文档服务。 例如，如果您要验证/应用使用权限等，我们将使用fd-service用户的资源解析程序来执行操作

1. [下载并解压缩与本文关联的zip文件。](assets/developingwithserviceuser.zip)
1. 导航到[http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles)
1. 上传和开始OSGi捆绑包
1. 确保捆绑包处于活动状态
1. 您现在已成功创建&#x200B;*系统用户*&#x200B;并部署了&#x200B;*服务用户包*。

   要提供对/content的访问，请为系统用户（“数据”）授予对内容节点的读取权限。

   1. 导航到[http://localhost:4502/useradmin](http://localhost:4502/useradmin)
   1. 搜索用户“数据”。 这是您在前一步中创建的同一系统用户。
   1. 多次单击用户，然后单击“权限”选项卡
   1. 授予对“content”文件夹的“ read”访问权限。
   1. 要使用服务用户访问/content文件夹，请使用以下代码

   ```java
   com.mergeandfuse.getserviceuserresolver.GetResolver aemDemoListings = sling.getService(com.mergeandfuse.getserviceuserresolver.GetResolver.class);
   
   resourceResolver = aemDemoListings.getServiceResolver();
   
   // get the resource. This will succeed because we have given ' read ' access to the content node
   
   Resource contentResource = resourceResolver.getResource("/content/forms/af/sandbox/abc.pdf");
   ```

   如果要访问捆绑包中的/content/dam/data.json文件，您将使用以下代码。 此代码假定您已为/content/dam/节点上的“data”用户授予读取权限

   ```java
   @Reference
   GetResolver getResolver;
   .
   .
   .
   ResourceResolver serviceResolver = getResolver.getServiceResolver();
   // to get resource resolver specific to fd-service user. This is for Document Services
   ResourceResolver fdserviceResolver = getResolver.getFormsServiceResolver();
   Node resNode = getResolver.getServiceResolver().getResource("/content/dam/data.json").adaptTo(Node.class);
   ```

   实施的完整代码如下

   ```java
   package com.mergeandfuse.getserviceuserresolver.impl;
   
   import java.util.HashMap;
   
   import org.apache.sling.api.resource.LoginException;
   import org.apache.sling.api.resource.ResourceResolver;
   import org.apache.sling.api.resource.ResourceResolverFactory;
   import org.osgi.service.component.annotations.Component;
   import org.osgi.service.component.annotations.Reference;
   import com.mergeandfuse.getserviceuserresolver.GetResolver;
   
   @Component(service = GetResolver.class)
   public class GetResolverImpl implements GetResolver {
    @Reference
    ResourceResolverFactory resolverFactory;
    @Override
    public ResourceResolver getServiceResolver() {
     HashMap<String, Object> param = new HashMap<String, Object>();
     param.put(ResourceResolverFactory.SUBSERVICE, "getresourceresolver");
     ResourceResolver resolver = null;
     try {
      resolver = resolverFactory.getServiceResourceResolver(param);
     } catch (LoginException e) {
      // TODO Auto-generated catch block
      e.printStackTrace();
     }
     return resolver;
    }
   ```

