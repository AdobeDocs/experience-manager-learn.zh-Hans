---
title: 使用汇编程序服务的XDP拼接
description: 在AEM Forms中使用Assembler服务拼接xdp
feature: Assembler
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
last-substantial-update: 2022-12-19T00:00:00Z
exl-id: e116038f-7d86-41ee-b1b0-7b8569121d6d
duration: 91
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '346'
ht-degree: 0%

---

# 使用汇编程序服务的XDP拼接

本文提供了用于展示使用汇编程序服务拼接xdp文档这一能力的宝贵资源。
编写了以下jsp代码，以将xdp文档（名为address.xdp）中名为**address**&#x200B;的子表单插入到master.xdp文档中名为&#x200B;**address**&#x200B;的插入点中。 生成的xdp已保存在AEM安装的根文件夹中。

汇编程序服务依赖于有效的DDX文档来描述PDF文档的操作。 您可以在此参阅[DDX参考文档](assets/ddxRef.pdf)。第40页包含有关xdp拼接的信息。

```java
    javax.servlet.http.Part ddxFile = request.getPart("xdpstitching.ddx");
    System.out.println("Got DDX");
    java.io.InputStream ddxIS = ddxFile.getInputStream();
    com.adobe.aemfd.docmanager.Document ddxDocument = new com.adobe.aemfd.docmanager.Document(ddxIS);
    javax.servlet.http.Part masterXdpPart = request.getPart("masterxdp.xdp");
    System.out.println("Got master xdp");
    java.io.InputStream masterXdpPartIS = masterXdpPart.getInputStream();
    com.adobe.aemfd.docmanager.Document masterXdpDocument = new com.adobe.aemfd.docmanager.Document(masterXdpPartIS);

    javax.servlet.http.Part fragmentXDPPart = request.getPart("fragment.xdp");
    System.out.println("Got fragment.xdp");
    java.io.InputStream fragmentXDPPartIS = fragmentXDPPart.getInputStream();
    com.adobe.aemfd.docmanager.Document fragmentXdpDocument = new com.adobe.aemfd.docmanager.Document(fragmentXDPPartIS);

    java.util.Map < String, Object > mapOfDocuments = new java.util.HashMap < String, Object > ();
    mapOfDocuments.put("master.xdp", masterXdpDocument);
    mapOfDocuments.put("address.xdp", fragmentXdpDocument);
    com.adobe.fd.assembler.service.AssemblerService assemblerService = sling.getService(com.adobe.fd.assembler.service.AssemblerService.class);
    if (assemblerService != null)
      System.out.println("Got assembler service");

    com.adobe.fd.assembler.client.AssemblerOptionSpec aoSpec = new com.adobe.fd.assembler.client.AssemblerOptionSpec();
    aoSpec.setFailOnError(true);

    com.adobe.fd.assembler.client.AssemblerResult assemblerResult = assemblerService.invoke(ddxDocument, mapOfDocuments, aoSpec);
    com.adobe.aemfd.docmanager.Document finalXDP = assemblerResult.getDocuments().get("stitched.xdp");
    finalXDP.copyToFile(new java.io.File("stitched.xdp"));
```

下面列出了用于将片段插入另一个xdp的DDX文件。 DDX将address.xdp中的子表单&#x200B;**地址**&#x200B;插入到master.xdp中名为&#x200B;**地址**&#x200B;的插入点中。 名为&#x200B;**stitched.xdp**&#x200B;的结果文档已保存到文件系统。

```xml
<?xml version="1.0" encoding="UTF-8"?> 
<DDX xmlns="http://ns.adobe.com/DDX/1.0/"> 
        <XDP result="stitched.xdp"> 
           <XDP source="master.xdp"> 
            <XDPContent insertionPoint="address" source="address.xdp" fragment="address"/> 
         </XDP> 
        </XDP>         
</DDX>
```

使此功能在您的AEM服务器上正常工作

* 将[XDP拼接包](assets/xdp-stitching.zip)下载到您的本地系统。
* 使用[包管理器](http://localhost:4502/crx/packmgr/index.jsp)上载并安装包
* [提取此zip文件的内容](assets/xdp-and-ddx.zip)以获取示例xdp和DDX文件

**安装包后，必须在Adobe Granite CSRF筛选器中允许列表以下URL。**

1. 请按照以下所述步骤列入允许列表上述路径。
1. [登录到configMgr](http://localhost:4502/system/console/configMgr)
1. 搜索Adobe Granite CSRF筛选器
1. 在排除的部分中添加以下路径并保存`/content/AemFormsSamples/assemblerservice`
1. 搜索“Sling引用过滤器”
1. 选中“允许为空”复选框。 （此设置仅用于测试目的）
可通过多种方法来测试示例代码。 最快、最轻松的是使用Postman应用程序。 Postman允许您向服务器发出POST请求。 在您的系统上安装Postman应用程序。
启动应用程序并输入以下URL以测试导出数据API
http://localhost:4502/content/AemFormsSamples/assemblerservice.html

提供屏幕快照中指定的以下输入参数。 您可以使用之前下载的示例文档，
![xdp-stitch-postman](assets/xdp-stitching-postman.png)

>[!NOTE]
>
>确保您的AEM Forms安装已完成。 您的所有包都必须处于活动状态。
>
