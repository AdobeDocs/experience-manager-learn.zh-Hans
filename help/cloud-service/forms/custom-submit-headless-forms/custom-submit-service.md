---
title: 创建自定义提交服务以处理Headless自适应表单提交
description: 根据提交的数据返回自定义响应
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-13520
exl-id: c23275d7-daf7-4a42-83b6-4d04b297c470
duration: 134
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '451'
ht-degree: 0%

---

# 创建自定义提交

AEM Forms提供了大量现成的提交选项，可满足大多数用例的要求。除了这些预定义的提交操作外，AEM Forms还允许您编写自己的自定义提交处理程序，以根据需要处理表单提交。

要编写自定义提交服务，请执行以下步骤

## 创建AEM项目

如果您已经有一个现有的AEM FormsCloud Service项目，您可以 [跳转至编写自定义提交服务](#Write-the-custom-submit-service)

* 在c驱动器上创建一个名为cloudmanager的文件夹。
* 导航到此新创建的文件夹
* 复制并粘贴以下项的内容： [此文本文件](./assets/creating-maven-project.txt) 命令提示符窗口中。您可能需要根据以下条件更改DarchetypeVersion=41： [最新版本](https://github.com/adobe/aem-project-archetype/releases). 在撰写本文时，最新版本是41。
* 按Enter键执行命令。如果一切运行正常，您应会看到生成成功消息。

## 编写自定义提交服务{#Write-the-custom-submit-service}

启动IntelliJ并打开AEM项目。 创建一个名为的新Java类 **HandleRegistrationFormSubmit** 如下面的屏幕快照所示
![custom-submit-service](./assets/custom-submit-service.png)

为实施服务编写了以下代码

```java
package com.aem.bankingapplication.core;
import java.util.HashMap;
import java.util.Map;
import com.google.gson.Gson;
import org.osgi.service.component.annotations.Component;
import com.adobe.aemds.guide.model.FormSubmitInfo;
import com.adobe.aemds.guide.service.FormSubmitActionService;
import com.adobe.aemds.guide.utils.GuideConstants;
import com.google.gson.JsonObject;
import org.slf4j.*;

@Component(
        service=FormSubmitActionService.class,
        immediate = true
)
public class HandleRegistrationFormSubmission implements FormSubmitActionService {
    private static final String serviceName = "Core Custom AF Submit";
    private static Logger logger = LoggerFactory.getLogger(HandleRegistrationFormSubmission.class);



    @Override
    public String getServiceName() {
        return serviceName;
    }

    @Override
    public Map<String, Object> submit(FormSubmitInfo formSubmitInfo) {
        logger.error("in my custom submit service");
        Map<String, Object> result = new HashMap<>();
        logger.error("in my custom submit service");
        String data = formSubmitInfo.getData();
        JsonObject formData = new Gson().fromJson(data,JsonObject.class);
        logger.error("The form data is "+formData);
        JsonObject jsonObject = new JsonObject();
        jsonObject.addProperty("firstName",formData.get("firstName").getAsString());
        jsonObject.addProperty("lastName",formData.get("lastName").getAsString());
        result.put(GuideConstants.FORM_SUBMISSION_COMPLETE, Boolean.TRUE);
        result.put("json",jsonObject.toString());
        return result;
    }

}
```

## 在应用程序下创建crx节点

展开ui.apps节点，创建一个名为的新包 **HandleRegistrationFormSubmit** 在应用程序节点下，如下面的屏幕快照中所示
![crx-node](./assets/crx-node.png)
在 **HandleRegistrationFormSubmit**. 将以下代码复制并粘贴到.content.xml中

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:sling="http://sling.apache.org/jcr/sling/1.0"
    jcr:description="Handle Registration Form Submission"
    jcr:primaryType="sling:Folder"
    guideComponentType="fd/af/components/guidesubmittype"
    guideDataModel="xfa,xsd,basic"
    submitService="Core Custom AF Submit"/>
```

的值 **submittservice** 元素必须匹配  **serviceName = &quot;核心自定义AF提交&quot;** 在FormSubmitActionService实施中。

## 将代码部署到本地AEM Forms实例

将更改推送到Cloud Manager存储库之前，建议将代码部署到本地云就绪的创作实例来测试代码。 确保创作实例正在运行。
要将代码部署到云就绪创作实例，请导航到AEM项目的根文件夹，然后运行以下命令

```
mvn clean install -PautoInstallSinglePackage
```

这会将代码作为单个包部署到创作实例

## 将代码推送到Cloud Manager并部署代码

在本地实例上验证代码后，将代码推送到您的云实例。
将更改推送到本地Git存储库，然后推送到Cloud Manager存储库。 您可参阅  [Git设置](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/forms/developing-for-cloud-service/setup-git.html)， [将AEM项目推送到cloud manager存储库](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/forms/developing-for-cloud-service/push-project-to-cloud-manager-git.html) 和 [部署到开发环境](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/forms/developing-for-cloud-service/deploy-to-dev-environment.html) 文章。

成功执行管道后，您应该能够将表单的提交操作关联到自定义提交处理程序，如下面的屏幕快照所示
![提交操作](./assets/configure-submit-action.png)

## 后续步骤

[在您的react应用程序中显示自定义响应](./handle-response-react-app.md)
