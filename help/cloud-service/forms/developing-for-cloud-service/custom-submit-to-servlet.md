---
title: 创建自定义提交操作处理程序
description: 将自适应表单提交到自定义提交处理程序
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
kt: 8852
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '139'
ht-degree: 0%

---

# 创建servlet以处理提交的数据

在IntelliJ中启动您的aem-banking项目。
创建一个简单的servlet以将提交的数据输出到日志文件。确保代码位于核心项目中，如下面的屏幕快照所示
![create-servlet](assets/create-servlet.png)

```java
package com.aem.bankingapplication.core.servlets;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import javax.servlet.Servlet;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.osgi.service.component.annotations.Component;
@Component(service = { Servlet.class}, property = {"sling.servlet.methods=post","sling.servlet.paths=/bin/formstutorial"})
public class HandleFormSubmissison extends SlingAllMethodsServlet {
    private static final Logger log = LoggerFactory.getLogger(HandleFormSubmissison.class);
    protected void doPost(SlingHttpServletRequest request,SlingHttpServletResponse response) {
        log.debug("Inside my formstutorial servlet");
        log.debug("The form data I got was "+request.getParameter("jcr:data"));
    }
}
```

## 创建自定义提交

在app/bankingapplication文件夹中创建自定义提交，方法与在 [AEM Forms的早期版本](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/adaptive-forms/custom-submit-aem-forms-article.html?lang=en)
post.request.jsp中的以下代码只是将POST转发到挂载在/bin/formstutorial上的servlet。 这与上一步中创建的servlet相同

```java
com.adobe.aemds.guide.utils.GuideSubmitUtils.setForwardPath(slingRequest,"/bin/formstutorial",null,null);
```

## 配置自适应表单

您现在可以配置自适应表单以提交到此自定义提交处理程序，其名称为 **提交到AEM Servlet**



