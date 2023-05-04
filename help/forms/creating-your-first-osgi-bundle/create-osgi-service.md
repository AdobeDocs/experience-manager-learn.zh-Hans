---
title: 使用AEM Forms创建您的首个OSGi服务
description: 使用AEM Forms构建您的首个OSGi服务
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: 2f15782e-b60d-40c6-b95b-6c7aa8290691
last-substantial-update: 2021-04-23T00:00:00Z
source-git-commit: bd41cd9d64253413e793479b5ba900c8e01c0eab
workflow-type: tm+mt
source-wordcount: '354'
ht-degree: 2%

---

# OSGi服务

OSGi服务是Java类或服务接口，以及许多作为名称/值对的服务属性。 所述服务属性区分提供具有相同服务接口的服务的不同服务提供商。

OSGi服务由其服务接口语义定义并实现为服务对象。 服务的功能由其实现的接口定义。 因此，不同的应用程序可以实现相同的服务。 服务界面允许包通过绑定界面进行交互，而不是通过实施。 应指定尽可能少的实施详细信息的服务接口。

## 定义界面

使用一种方法将数据与 <span class="x x-first x-last">XDP</span> 模板。

```java
package com.mysite.samples;

import com.adobe.aemfd.docmanager.Document;

public interface MyfirstInterface
{
    public Document mergeDataWithXDPTemplate(Document xdpTemplate, Document xmlDocument);
}
 
```

## 实施界面

创建一个名为 `com.mysite.samples.impl` 来保存接口的实现。

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

注释 `@Component(...)` 第10行将此Java类标记为OSGi组件，并将其注册为OSGi服务。

的 `@Reference` 注释是OSGi声明性服务的一部分，用于注入 [Outputservice](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/index.html?com/adobe/fd/output/api/OutputService.html) 变量 `outputService`.


## 构建和部署包

* 打开 **命令提示符窗口**
* 导航至 `c:\aemformsbundles\mysite\core`
* 执行命令 `mvn clean install -PautoInstallBundle`
* 上述命令将自动生成包并将其部署到在localhost:4502上运行的AEM实例。

此包还将在以下位置提供 `C:\AEMFormsBundles\mysite\core\target`. 也可以使用 [Felix Web控制台。](http://localhost:4502/system/console/bundles)

## 使用服务

您现在可以在JSP页中使用服务。 以下代码片段显示如何获取对服务的访问权限以及如何使用服务实施的方法

```java
MyFirstAEMFormsService myFirstAEMFormsService = sling.getService(com.mysite.samples.MyFirstAEMFormsService.class);
com.adobe.aemfd.docmanager.Document generatedDocument = myFirstAEMFormsService.mergeDataWithXDPTemplate(xdp_or_pdf_template,xmlDocument);
```

包含JSP页的示例包可以是 [从此处下载](assets/learning_aem_forms.zip)

[可下载完整的包](assets/mysite.core-1.0.0-SNAPSHOT.jar)

## 测试包

使用将包导入并安装到AEM中 [包管理器](http://localhost:4502/crx/packmgr/index.jsp)

使用postman进行POST调用并提供输入参数，如下面的屏幕快照中所示
![邮递员](assets/test-service-postman.JPG)

## 后续步骤

[创建Sling Servlet](./create-servlet.md)

