---
title: 在AEM Forms中与服务用户一起开发
description: 本文将指导您完成在AEM Forms中创建服务用户的过程
feature: Adaptive Forms
topic: Development
role: Developer
level: Experienced
exl-id: 5fa3d52a-6a71-45c4-9b1a-0e6686dd29bc
source-git-commit: c462d48d26c9a7aa0e4cfc4f24005b41e8e82cb8
workflow-type: tm+mt
source-wordcount: '445'
ht-degree: 1%

---

# 在AEM Forms中与服务用户一起开发

本文将指导您完成在AEM Forms中创建服务用户的过程

在Adobe Experience Manager(AEM)的早期版本中，管理资源解析程序用于后端处理，这需要访问存储库。 AEM 6.3中已弃用管理资源解析程序，而是使用在存储库中具有特定权限的系统用户。

详细了解 [在AEM中创建和使用服务用户](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/advanced/service-users.html).

本文将介绍如何创建系统用户并配置用户映射器属性。

1. 导航到 [http://localhost:4502/crx/explorer/index.jsp](http://localhost:4502/crx/explorer/index.jsp)
1. 以“管理员”身份登录
1. 单击“ User Administration ”
1. 单击“创建系统用户”
1. 将userid类型设置为“ data ”，然后单击绿色图标以完成创建系统用户的过程
1. [打开configMgr](http://localhost:4502/system/console/configMgr)
1. 搜索 _Apache Sling服务用户映射器服务_ ，然后单击以打开属性
1. 单击 *+* 图标（加号）以添加以下服务映射

   * DevelopingWithServiceUser.core:getresourceresolver=data
   * DevelopingWithServiceUser.core:getformsresourceresolver=fd-service

1. 单击“保存”

在上述配置设置中，DevelopingWithServiceUser.core是包的符号名称。 getresourceresolver是子服务名称。data是在前面步骤中创建的系统用户。

我们还可以代表fd-service用户获取资源解析程序。 此服务用户用于文档服务。 例如，如果您想要验证/应用使用权限等，我们将使用fd-service用户的资源解析程序来执行操作

1. [下载并解压缩与本文关联的zip文件。](assets/developingwithserviceuser.zip)
1. 导航到 [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles)
1. 上传并启动OSGi包
1. 确保包处于活动状态
1. 您现在已成功创建 *系统用户* 并部署了 *服务用户包*.

   要提供对/content的访问权，请为系统用户（“数据”）授予对内容节点的读取权限。

   1. 导航到 [http://localhost:4502/useradmin](http://localhost:4502/useradmin)
   1. 搜索用户“数据”。 这是您在前面步骤中创建的系统用户。
   1. 双击用户，然后单击“ Permissions ”选项卡
   1. 授予对“content”文件夹的“read”访问权限。
   1. 要使用服务用户获取对/content文件夹的访问权限，请使用以下代码



```java
com.mergeandfuse.getserviceuserresolver.GetResolver aemDemoListings = sling.getService(com.mergeandfuse.getserviceuserresolver.GetResolver.class);
   
resourceResolver = aemDemoListings.getServiceResolver();
   
// get the resource. This will succeed because we have given ' read ' access to the content node
   
Resource contentResource = resourceResolver.getResource("/content/forms/af/sandbox/abc.pdf");
```

如果要访问包中的/content/dam/data.json文件，将使用以下代码。 此代码假定您已为/content/dam/节点上的“data”用户授予读取权限

```java
@Reference
GetResolver getResolver;
.
.
.
try {
   ResourceResolver serviceResolver = getResolver.getServiceResolver();

   // To get resource resolver specific to fd-service user. This is for Document Services
   ResourceResolver fdserviceResolver = getResolver.getFormsServiceResolver();

   Node resNode = getResolver.getServiceResolver().getResource("/content/dam/data.json").adaptTo(Node.class);
} catch(LoginException ex) {
   // Unable to get the service user, handle this exception as needed
} finally {
  // Always close all ResourceResolvers you open!
  
  if (serviceResolver != null( { serviceResolver.close(); }
  if (fdserviceResolver != null) { fdserviceResolver.close(); }
}
```

实施的完整代码如下所示

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
                System.out.println("#### Trying to get service resource resolver ....  in my bundle");
                HashMap < String, Object > param = new HashMap < String, Object > ();
                param.put(ResourceResolverFactory.SUBSERVICE, "getresourceresolver");
                ResourceResolver resolver = null;
                try {
                        resolver = resolverFactory.getServiceResourceResolver(param);
                } catch (LoginException e) {

                        System.out.println("Login Exception " + e.getMessage());
                }
                return resolver;

        }

        @Override
        public ResourceResolver getFormsServiceResolver() {

                System.out.println("#### Trying to get Resource Resolver for forms ....  in my bundle");
                HashMap < String, Object > param = new HashMap < String, Object > ();
                param.put(ResourceResolverFactory.SUBSERVICE, "getformsresourceresolver");
                ResourceResolver resolver = null;
                try {
                        resolver = resolverFactory.getServiceResourceResolver(param);
                } catch (LoginException e) {
                        System.out.println("Login Exception ");
                        System.out.println("The error message is " + e.getMessage());
                }
                return resolver;
        }

}
```
