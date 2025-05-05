---
title: 创建自定义提交操作处理程序
description: 将自适应表单提交到自定义提交处理程序
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
topic: Development
feature: Developer Tools
jira: KT-8852
exl-id: 983e0394-7142-481f-bd5e-6c9acefbfdd0
duration: 52
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '203'
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

## 创建自定义提交处理程序

在`apps/bankingapplication`文件夹中创建自定义提交操作的方式与在[早期版本的AEM Forms](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/adaptive-forms/custom-submit-aem-forms-article.html?lang=zh-Hans)中创建的方式相同。 在本教程中，我在CRX存储库的`apps/bankingapplication`节点下创建了一个名为SubmitToAEMServlet的文件夹。

post.POST.jsp中的以下代码只是将请求转发到挂载在/bin/formstutorial上的servlet。 这与上一步中创建的servlet相同

```java
com.adobe.aemds.guide.utils.GuideSubmitUtils.setForwardPath(slingRequest,"/bin/formstutorial",null,null);
```

在IntelliJ的AEM项目中，右键单击`apps/bankingapplication`文件夹并选择“新建” | 在“新建包”对话框中，将apps.banking应用程序打包并键入SubmitToAEMServlet。 右键单击SubmitToAEMServlet节点并选择repo | 获取用于将AEM项目与AEM服务器存储库同步的命令。


## 配置自适应表单

您现在可以将任何自适应表单配置为提交到此自定义提交处理程序，此处理程序称为&#x200B;**提交到AEM Servlet**

## 后续步骤

[使用资源类型注册servlet](./registering-servlet-using-resourcetype.md)
