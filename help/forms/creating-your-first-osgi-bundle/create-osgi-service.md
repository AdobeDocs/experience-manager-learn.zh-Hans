---
title: 使用AEM Forms创建您的首个OSGi服务
description: '使用AEM Forms构建您的首个OSGi服务 '
feature: 自适应表单
version: 6.4,6.5
topic: 开发
role: Developer
level: Beginner
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '345'
ht-degree: 2%

---


# OSGi服务

OSGi服务是Java类或服务接口，以及许多服务属性（如名称/值对）。 所述服务属性区分提供具有相同服务接口的服务的不同服务提供商。

OSGi服务由其服务接口语义定义并实现为服务对象。 服务的功能由其实现的接口定义。 因此，不同的应用程序可以实现相同的服务。 服务界面允许包通过绑定界面进行交互，而不是通过实施。 应指定尽可能少的实施详细信息的服务接口。

## 定义界面

使用一种方法将数据与<span class="x x-first x-last">XDP</span>模板合并的简单接口。

```java
package com.learningaemforms.adobe.core;

import com.adobe.aemfd.docmanager.Document;

public interface MyfirstInterface {
  public Document mergeDataWithXDPTemplate(Document xdpTemplate, Document xmlDocument);
} 
```

## 实施界面

创建名为`com.learningaemforms.adobe.core.impl`的新包，以保存接口的实现。

```java
package com.learningaemforms.adobe.core.impl;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import com.adobe.aemfd.docmanager.Document;
import com.adobe.fd.output.api.OutputService;
import com.adobe.fd.output.api.OutputServiceException;
import com.learningaemforms.adobe.core.MyfirstInterface;
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

第10行上的注释`@Component(...)`将此Java类标记为OSGi组件，并将其注册为OSGi服务。

`@Reference`注释是OSGi声明性服务的一部分，用于将[Outputservice](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/index.html?com/adobe/fd/output/api/OutputService.html)的引用注入变量`outputService`中。


## 构建和部署包

* 打开&#x200B;**命令提示符窗口**
* 导航至 `c:\aemformsbundles\learningaemforms\core`
* 执行命令`mvn clean install -PautoInstallBundle`
* 上述命令将自动生成包并将其部署到在localhost:4502上运行的AEM实例。

此包还将在以下位置`C:\AEMFormsBundles\learningaemforms\core\target`提供。 也可以使用[Felix Web控制台将包部署到AEM中。](http://localhost:4502/system/console/bundles)

## 使用服务

您现在可以在JSP页中使用服务。 以下代码片段显示如何获取对服务的访问权限以及如何使用服务实施的方法

```java
MyFirstAEMFormsService myFirstAEMFormsService = sling.getService(com.learningaemforms.adobe.core.MyFirstAEMFormsService.class);
com.adobe.aemfd.docmanager.Document generatedDocument = myFirstAEMFormsService.mergeDataWithXDPTemplate(xdp_or_pdf_template,xmlDocument);
```

包含JSP页的示例包可以从此处](assets/learning-aem-forms.zip)下载![

## 测试包

使用[包管理器](http://localhost:4502/crx/packmgr/index.jsp)将包导入并安装到AEM中

使用postman进行POST调用并提供输入参数，如下面的屏幕快照中所示
![postman](assets/test-service-postman.JPG)
