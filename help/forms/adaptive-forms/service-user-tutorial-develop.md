---
title: 在AEM Forms中使用服务用户进行开发
description: 本文向您介绍在AEM Forms中创建服务用户的过程
feature: Adaptive Forms
topic: Development
role: Developer
level: Experienced
exl-id: 5fa3d52a-6a71-45c4-9b1a-0e6686dd29bc
last-substantial-update: 2020-09-09T00:00:00Z
duration: 140
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '415'
ht-degree: 0%

---

# 在AEM Forms中使用服务用户进行开发

本文向您介绍在AEM Forms中创建服务用户的过程

在Adobe Experience Manager (AEM)的早期版本中，管理资源解析程序用于需要访问存储库的后端处理。 AEM 6.3中不建议使用管理资源解析程序。而是使用存储库中具有特定权限的系统用户。

详细了解 [在AEM中创建和使用服务用户](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/advanced/service-users.html).

本文介绍了如何创建系统用户和配置用户映射器属性。

1. 导航到 [http://localhost:4502/crx/explorer/index.jsp](http://localhost:4502/crx/explorer/index.jsp)
1. 以“管理员”身份登录
1. 单击“用户管理”
1. 单击“创建系统用户”
1. 将用户ID类型设置为“ data ”，然后单击绿色图标以完成创建系统用户的过程
1. [打开configMgr](http://localhost:4502/system/console/configMgr)
1. 搜索 _Apache Sling服务用户映射器服务_ ，然后单击以打开属性
1. 单击 *+* 图标（加号）以添加以下服务映射

   * DevelopingWithServiceUser.core:getresourceresolver=data
   * DevelopingWithServiceUser.core:getformsresourceresolver=fd-service

1. 单击“保存”

在上述配置设置中， DevelopingWithServiceUser.core是捆绑包的符号名称。 getresourceresolver是子服务名称。data是在前一步骤中创建的系统用户。

还可以代表fd-service用户获取资源解析程序。 此服务用户用于文档服务。 例如，如果要验证/应用使用权限等，我们将使用fd-service用户的资源解析器来执行操作

1. [下载并解压缩与本文关联的zip文件。](assets/developingwithserviceuser.zip)
1. 导航到 [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles)
1. 上传并启动OSGi捆绑包
1. 确保捆绑包处于活动状态
1. 您现在已成功创建了 *系统用户* 并部署了 *服务用户捆绑包*.

   要提供对/content的访问权限，请授予系统用户（“数据”）对内容节点的读取权限。

   1. 导航到 [http://localhost:4502/useradmin](http://localhost:4502/useradmin)
   1. 搜索用户“ data ”。 这是您在上一步中创建的相同系统用户。
   1. 双击用户，然后单击“权限”选项卡
   1. 授予“内容”文件夹的“读取”访问权限。
   1. 要使用服务用户访问/content文件夹，请使用以下代码



```java
com.mergeandfuse.getserviceuserresolver.GetResolver aemDemoListings = sling.getService(com.mergeandfuse.getserviceuserresolver.GetResolver.class);
   
resourceResolver = aemDemoListings.getServiceResolver();
   
// get the resource. This will succeed because we have given ' read ' access to the content node
   
Resource contentResource = resourceResolver.getResource("/content/forms/af/sandbox/abc.pdf");
```

如果要访问捆绑包中的/content/dam/data.json文件，将使用以下代码。 此代码假定您已经为/content/dam/节点上的“data”用户授予了读取权限

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

下面给出了实施的完整代码

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
