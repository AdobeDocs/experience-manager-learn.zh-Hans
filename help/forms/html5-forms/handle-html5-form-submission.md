---
title: 处理HTML5表单提交
description: 创建HTML5表单提交处理程序。
feature: Mobile Forms
doc-type: article
version: Experience Manager 6.4, Experience Manager 6.5
jira: KT-5269
thumbnail: kt-5269.jpg
topic: Development
role: Developer
level: Experienced
exl-id: 93e1262b-0e93-4ba8-aafc-f9c517688ce9
last-substantial-update: 2020-07-07T00:00:00Z
duration: 66
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '227'
ht-degree: 0%

---


# 处理HTML5表单提交

HTML5 forms可以提交到AEM中托管的servlet。 提交的数据可在servlet中作为输入流访问。 要提交HTML5表单，请使用AEM Forms Designer在表单模板上添加“HTTP提交按钮”。

## 创建提交处理程序

一个简单的servlet可以处理HTML5表单提交。 使用以下代码片段提取提交的数据。 下载本教程中提供的[servlet](assets/html5-submit-handler.zip)。 使用[包管理器](http://localhost:4502/crx/packmgr/index.jsp)安装[servlet](assets/html5-submit-handler.zip)。

```java
StringBuffer stringBuffer = new StringBuffer();
String line = null;
java.io.InputStreamReader isReader = new java.io.InputStreamReader(request.getInputStream(), "UTF-8");
java.io.BufferedReader reader = new java.io.BufferedReader(isReader);
while ((line = reader.readLine()) != null) {
    stringBuffer.append(line);
}
System.out.println("The submitted form data is " + stringBuffer.toString());
```

如果您计划使用代码调用J2EE进程，请确保已配置[Adobe LiveCycle Client SDK配置](https://helpx.adobe.com/aem-forms/6/submit-form-data-livecycle-process.html)。

## 配置HTML5表单的提交URL

![提交URL](assets/submit-url.PNG)

- 打开xdp并导航到&#x200B;_属性_->_高级_。
- 复制http://localhost:4502/content/AemFormsSamples/handlehml5formsubmission.html并将其粘贴到提交URL文本字段中。
- 单击&#x200B;_SaveAndClose_&#x200B;按钮。

### 在排除路径中添加条目

- 转到[configMgr](http://localhost:4502/system/console/configMgr)。
- 搜索&#x200B;_Adobe Granite CSRF筛选器_。
- 在排除的路径部分中添加以下条目： _/content/AemFormsSamples/handlehml5formsubmission_。
- 保存更改。

### 测试表单

- 打开xdp模板。
- 单击&#x200B;_预览_->预览为HTML。
- 在表单中输入数据，然后单击提交。
- 检查服务器的stdout.log文件以了解提交的数据。

### 附加阅读

有关通过HTML5表单提交生成PDF的更多信息，请参阅此[文章](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/document-services/generate-pdf-from-mobile-form-submission-article.html)。

