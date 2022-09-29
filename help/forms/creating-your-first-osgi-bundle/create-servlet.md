---
title: 在AEM Forms中创建您的第一个Servlet
description: 构建您的第一个sling servlet以将数据与表单模板合并。
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: 72728ed7-80a2-48b5-ae7f-d744db8a524d
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '207'
ht-degree: 0%

---

# Sling Servlet

Servlet是一种类，用于扩展通过请求 — 响应编程模型访问的托管应用程序的服务器的功能。 对于此类应用程序，Servlet技术定义特定于HTTP的Servlet类。
所有Servlet都必须实施Servlet接口，该接口定义生命周期方法。


AEM中的Servlet可注册为OSGi服务：您可以扩展SlingSafeMethodsServlet以用于只读实现或SlingAllMethodsServlet，以便实施所有RESTful操作。

## Servlet代码

```java
package com.mysite.core.servlets;
import javax.servlet.Servlet;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;

import com.adobe.aemfd.docmanager.Document;
import com.adobe.fd.forms.api.FormsService;

@Component(service={Servlet.class}, property={"sling.servlet.methods=post", "sling.servlet.paths=/bin/mergedataWithAcroform"})
public class MyFirstAEMFormsServlet extends SlingAllMethodsServlet
{
    
    private static final long serialVersionUID = 1L;
    @Reference
    FormsService formsService;
     protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response)
      { 
         String file_path = request.getParameter("save_location");
         
         java.io.InputStream pdf_document_is = null;
         java.io.InputStream xml_is = null;
         javax.servlet.http.Part pdf_document_part = null;
         javax.servlet.http.Part xml_data_part = null;
              try
              {
                 pdf_document_part = request.getPart("pdf_file");
                 xml_data_part = request.getPart("xml_data_file");
                 pdf_document_is = pdf_document_part.getInputStream();
                 xml_is = xml_data_part.getInputStream();
                 Document data_merged_document = formsService.importData(new Document(pdf_document_is), new Document(xml_is));
                 data_merged_document.copyToFile(new File(file_path));
                 
              }
              catch(Exception e)
              {
                  response.sendError(400,e.getMessage());
              }
      }
}
```

## 构建和部署

要构建项目，请执行以下步骤：

* 打开 **命令提示符窗口**
* 导航至 `c:\aemformsbundles\mysite\core`
* 执行命令 `mvn clean install -PautoInstallBundle`
* 上述命令会自动生成包并将其部署到在localhost:4502上运行的AEM实例。

该包也可在以下位置使用 `C:\AEMFormsBundles\mysite\core\target`. 也可以使用 [Felix Web控制台。](http://localhost:4502/system/console/bundles)


## 测试Servlet解析程序

将您的浏览器指向 [servlet解析程序URL](http://localhost:4502/system/console/servletresolver?url=%2Fbin%2FmergedataWithAcroform&amp;method=POST). 这可告知您为给定路径调用的Servlet，如下面的屏幕快照所示
![servlet-resolver](assets/servlet-resolver.JPG)

## 使用Postman测试Servlet

![使用Postman测试Servlet](assets/test-servlet-postman.JPG)
