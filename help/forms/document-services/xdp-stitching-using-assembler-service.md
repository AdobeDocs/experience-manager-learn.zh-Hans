---
title: 使用汇编程序服务进行XDP拼合
description: 使用AEM Forms中的汇编程序服务拼合xdp
feature: Assembler
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
last-substantial-update: 2022-12-19T00:00:00Z
source-git-commit: 8f17e98c56c78824e8850402e3b79b3d47901c0b
workflow-type: tm+mt
source-wordcount: '357'
ht-degree: 2%

---

# 使用汇编程序服务进行XDP拼合

本文为您提供了一些资产，用于演示使用汇编程序服务拼合xdp文档的功能。
以下jsp代码用于插入名为 **地址** 从名为address.xdp的xdp文档到名为 **地址** 在主控.xdp文档中。 生成的xdp保存在AEM安装的根文件夹中。

汇编程序服务依赖有效的DDX文档来描述PDF文档的操作。 您可以将 [此处提供DDX参考文档](assets/ddxRef.pdf).第40页包含有关xdp拼合的信息。

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

下面列出了用于将片段插入其他xdp的DDX文件。 DDX插入子表单  **地址** 从address.xdp到名为 **地址** 在主控.xdp中。 生成的文档名为 **suntid.xdp** 保存到文件系统。

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

使此功能在您的AEM Server上正常工作

* 下载 [XDP拼合包](assets/xdp-stitching.zip) 到本地系统。
* 使用上传和安装包 [包管理器](http://localhost:4502/crx/packmgr/index.jsp)
* [提取此zip文件的内容](assets/xdp-and-ddx.zip) 获取xdp和DDX文件的示例

**安装包后，必须在Granite CSRF过允许列表滤器中Adobe以下URL。**

1. 请按照下面提到的步骤来允许列表上述路径。
1. [登录到configMgr](http://localhost:4502/system/console/configMgr)
1. 搜索AdobeGranite CSRF过滤器
1. 在排除的部分中添加以下路径并保存 `/content/AemFormsSamples/assemblerservice`
1. 搜索“Sling反向链接过滤器”
1. 选中“允许空”复选框。 （此设置应仅用于测试目的）有多种方法可测试示例代码。 最快、最简单的方法是使用Postman应用程序。 Postman允许您向服务器发出POST请求。 在系统上安装Postman应用程序。
启动应用程序并输入以下URL以测试导出数据API http://localhost:4502/content/AemFormsSamples/assemblerservice.html

按照屏幕快照中指定的方式提供以下输入参数。 您可以使用之前下载的示例文档，
![xdp-stitch-postman](assets/xdp-stitching-postman.png)

>[!NOTE]
>
>确保AEM Forms安装完成。 所有包都需要处于活动状态。
