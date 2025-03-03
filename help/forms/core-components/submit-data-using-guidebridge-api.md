---
title: 使用GuideBridge API发布表单数据
description: 了解如何使用GuideBridge API访问和发送自适应表单的表单数据。 轻松保存和检索表单数据。
duration: 68
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Experienced
jira: KT-15286
last-substantial-update: 2024-04-05T00:00:00Z
exl-id: 099aaeaf-2514-4459-81a7-2843baa1c981
source-git-commit: 52b7e6afbfe448fd350e84c3e8987973c87c4718
workflow-type: tm+mt
source-wordcount: '132'
ht-degree: 0%

---


# 使用GuideBridge API访问和发送表单数据

了解如何使用GuideBridge API访问表单数据，并将其发送到REST端点以进行存储和检索。 此功能使用户能够无缝地保存和恢复表单完成。

单击规则编辑器中的按钮后，触发JavaScript函数，以保存表单数据。

![规则编辑器](assets/rule-editor.png)

下面的JavaScript函数演示了如何将表单数据发送到指定的端点：

```javascript
/**
* Submits data and attachments 
* @name submitFormDataAndAttachments Submit form data and attachments to REST endpoint
* @param {string} endpoint in String format
* @return {string} 
 */
 
function submitFormDataAndAttachments(endpoint) {
    guideBridge.getFormDataObject({
        success: function(resultObj) {
            const afFormData = resultObj.data.data;
            const formData = new FormData();
            formData.append("dataXml", afFormData);
            resultObj.data.attachments.forEach(attachment => {
                console.log(attachment.name);
                formData.append(attachment.name, attachment.data);
            });
            fetch(endpoint, {
                method: 'POST',
                body: formData
            })
            .then(response => {
                if (response.ok) {
                    console.log("Successfully saved");
                    const fld = guideBridge.resolveNode("$form.confirmation");
                    return "Form data was saved successfully";
                } else {
                    throw new Error('Failed to save form data');
                }
            })
            .catch(error => {
                console.error('Error:', error);
            });
        }
    });
}
```

## 服务器端代码

以下服务器端Java代码处理表单数据处理。 AEM中的此Java servlet可通过上述JavaScript函数中的XHR调用进行调用。

```java
package com.azuredemo.core.servlets;

import com.adobe.aemfd.docmanager.Document;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.request.RequestParameter;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.apache.sling.servlets.annotations.SlingServletResourceTypes;
import org.osgi.service.component.annotations.Component;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import javax.servlet.Servlet;
import javax.servlet.http.HttpServletResponse;
import java.io.File;
import java.io.IOException;
import java.io.Serializable;
import java.util.List;

@Component(
   service = {
      Servlet.class
   }
)
@SlingServletResourceTypes(
   resourceTypes = "azure/handleFormSubmission",
   methods = "POST",
   extensions = "json"
)
public class StoreFormSubmission extends SlingAllMethodsServlet implements Serializable {
   private static final long serialVersionUID = 1L;
   private final transient Logger log = LoggerFactory.getLogger(this.getClass());

   protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) throws IOException {
      List<RequestParameter> listOfRequestParameters = request.getRequestParameterList();
      log.debug("The size of the list is " + listOfRequestParameters.size());
      
      for (int i = 0; i < listOfRequestParameters.size(); i++) {
         RequestParameter requestParameter = listOfRequestParameters.get(i);
         log.debug("Is this request parameter a form field?" + requestParameter.isFormField());
         
         if (!requestParameter.isFormField()) {
            Document attachmentDOC = new Document(requestParameter.getInputStream());
            attachmentDOC.copyToFile(new File(requestParameter.getName()));
         } else {
            log.debug("Not a form field " + requestParameter.getName());
            log.debug(requestParameter.getString());
         }
      }
      
      response.setStatus(HttpServletResponse.SC_OK);
   }
}
```
