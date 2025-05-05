---
title: 使用AEM Forms创建您的第一个OSGi服务
description: 使用AEM Forms构建您的第一个OSGi服务
feature: Adaptive Forms
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
exl-id: 2f15782e-b60d-40c6-b95b-6c7aa8290691
last-substantial-update: 2021-04-23T00:00:00Z
duration: 87
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '337'
ht-degree: 0%

---

# OSGi服务

OSGi服务是一个Java类或服务接口，以及许多作为名称/值对的服务属性。 服务属性区分使用相同服务接口提供服务的不同服务提供商。

OSGi服务由其服务接口语义定义，并作为服务对象实现。 服务的功能由其实现的接口定义。 因此，不同的应用程序可以实现相同的服务。 服务接口允许捆绑包通过绑定接口而不是实现进行交互。 指定服务接口时应尽可能少地提供实现详细信息。

## 定义接口

一个简单接口，具有一个将数据与<span class="x x-first x-last">XDP</span>模板合并的方法。

```java
package com.mysite.samples;

import com.adobe.aemfd.docmanager.Document;

public interface MyfirstInterface
{
    public Document mergeDataWithXDPTemplate(Document xdpTemplate, Document xmlDocument);
}
 
```

## 实施接口

创建一个名为`com.mysite.samples.impl`的新包来保存接口的实现。

```java
package com.mysite.samples.impl;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import com.adobe.aemfd.docmanager.Document;
import com.adobe.fd.output.api.OutputService;
import com.adobe.fd.output.api.OutputServiceException;
import com.mysite.samples.MyfirstInterface;
@Component(service = MyfirstInterface.class)
public class MyfirstInterfaceImpl implements MyfirstInterface {
  @Reference
  OutputService outputService;

  private static final Logger log = LoggerFactory.getLogger(MyfirstInterfaceImpl.class);

  @Override
  public Document mergeDataWithXDPTemplate(Document xdpTemplate, Document xmlDocument) {
    com.adobe.fd.output.api.PDFOutputOptions pdfOptions = new com.adobe.fd.output.api.PDFOutputOptions();
    pdfOptions.setAcrobatVersion(com.adobe.fd.output.api.AcrobatVersion.Acrobat_11);
    try {
      return outputService.generatePDFOutput(xdpTemplate, xmlDocument, pdfOptions);

    } catch (OutputServiceException e) {

      log.error("Failed to merge data with XDP Template", e);

    }

    return null;
  }

}
```

第10行的注释`@Component(...)`将此Java类标记为OSGi组件，并将其注册为OSGi服务。

`@Reference`注释是OSGi声明性服务的一部分，用于将[Outputservice](https://helpx.adobe.com/cn/experience-manager/6-5/forms/javadocs/index.html?com/adobe/fd/output/api/OutputService.html)的引用插入到变量`outputService`中。


## 生成并部署捆绑包

* 打开&#x200B;**命令提示符窗口**
* 导航到`c:\aemformsbundles\mysite\core`
* 执行命令`mvn clean install -PautoInstallBundle`
* 上述命令将自动构建捆绑包，并将其部署到在localhost：4502上运行的AEM实例

该包还将在以下位置`C:\AEMFormsBundles\mysite\core\target`提供。 也可以使用[Felix Web控制台](http://localhost:4502/system/console/bundles)将该捆绑包部署到AEM中。

## 使用服务

现在，您可以在JSP页中使用服务。 以下代码片段显示了如何获取对服务的访问权限，以及如何使用由服务实施的方法

```java
MyFirstAEMFormsService myFirstAEMFormsService = sling.getService(com.mysite.samples.MyFirstAEMFormsService.class);
com.adobe.aemfd.docmanager.Document generatedDocument = myFirstAEMFormsService.mergeDataWithXDPTemplate(xdp_or_pdf_template,xmlDocument);
```

可以从此处[&#128279;](assets/learning_aem_forms.zip)下载包含JSP页的示例包

[完整的捆绑包可供下载](assets/mysite.core-1.0.0-SNAPSHOT.jar)

## 测试包

使用[包管理器](http://localhost:4502/crx/packmgr/index.jsp)将包导入并安装到AEM中

使用Postman进行POST调用并提供输入参数，如下面的屏幕快照中所示
![邮递员](assets/test-service-postman.JPG)

## 后续步骤

[创建Sling Servlet](./create-servlet.md)

