---
title: 建立自訂OSGi設定
description: 擷取Document Cloud特定詳細資料的自訂OSGi設定
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
thumbnail: 7818.jpg
kt: 7818
exl-id: 1f34c356-6c0c-46ff-9cea-7baacfc4bb7f
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '63'
ht-degree: 1%

---

# 简介

建立自訂OSGi設定以擷取您Document Cloud帳戶的認證


若要進行自訂OSGi設定，我們需要先建立介面，其公用方法將代表設定中的欄位。

![doc-cloud-config](assets/doc-cloud-configuration.JPG)


建立名為DocumentCloudConfiguration的介面，並在其中貼上下列程式碼。

```java
package com.aemforms.doccloud.core;

import org.osgi.service.metatype.annotations.AttributeDefinition;
import org.osgi.service.metatype.annotations.ObjectClassDefinition;

@ObjectClassDefinition(name="Document Cloud Service Configuration", description = "Connect AEM Forms With Document Cloud")
public @interface DocumentCloudConfiguration {
	  @AttributeDefinition(name="Client ID", description="Client ID")
	  String getClientID() default "";
	  
	  @AttributeDefinition(name="Client Secret", description="Client Secret")
	  public String getClientSecret() default "26dc4de1-f7f0-46b6-9e8e-86270ad34f58";
	  
	  @AttributeDefinition(name="Technical Account ID", description="Technical Account ID")
	  public String getTechnicalAccount() default "42FF05A7606F71BB0A495FBE@techacct.adobe.com";

	  @AttributeDefinition(name="Organization ID", description="Organization ID")
	  String getOrganizationID() default "299805FF5FC9199D0A495EBC@AdobeOrg";
	  
	  @AttributeDefinition(name="Metascope", description="Metascope")
	  String getMetascope() default "ent_documentcloud_sdk";


}
```
