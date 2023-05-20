---
title: 建立自訂提交動作處理常式
description: 將最適化表單提交至自訂提交處理常式
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

# 建立servlet以處理提交的資料

在IntelliJ中啟動您的aem-banking專案。
建立簡單的servlet，將提交的資料輸出至記錄檔。確認程式碼位於核心專案中，如下列熒幕擷圖所示
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

## 建立自訂提交

在app/bankingapplication資料夾中建立自訂提交，就像在 [舊版AEM Forms](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/adaptive-forms/custom-submit-aem-forms-article.html?lang=en)
post.request.jsp中的下列程式碼只會將POST轉送給掛載在/bin/formstutorial上的servlet。 這是先前步驟中建立的相同servlet

```java
com.adobe.aemds.guide.utils.GuideSubmitUtils.setForwardPath(slingRequest,"/bin/formstutorial",null,null);
```

## 設定最適化表單

您現在可以設定最適化表單，以提交至這個自訂提交處理常式，稱為 **提交至AEM Servlet**



