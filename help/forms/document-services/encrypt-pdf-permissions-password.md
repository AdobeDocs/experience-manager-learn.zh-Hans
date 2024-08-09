---
title: 使用权限密码加密PDF
description: 使用DocAssuranceService加密PDF
feature: Document Services
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
jira: KT-15849
last-substantial-update: 2024-07-19T00:00:00Z
exl-id: 5df8581c-a44c-449c-bf3b-8cdf57635c4d
source-git-commit: d01a56cd1fd3085b0230918b15b4635ba375e346
workflow-type: tm+mt
source-wordcount: '186'
ht-degree: 0%

---

# 使用权限密码加密PDF

复制、编辑或打印PDF时需要权限密码（也称为所有者或主密码）。 了解如何使用[DocAssuranceService](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javadocs/index.html?com/adobe/fd/docassurance/client/api/DocAssuranceService.html) API以编程方式将权限密码应用于PDF。

以下JSP代码使用权限密码对PDF进行加密：

```java
<%--
     Encrypt PDF with permissions password
--%>
    <%@include file="/libs/foundation/global.jsp"%>
<%@ page import="com.adobe.fd.docassurance.client.api.EncryptionOptions,java.util.*,java.io.*,com.adobe.fd.encryption.client.*" %>
    <%@page session="false" %>
<%
    String filePath = request.getParameter("saveLocation");
    InputStream pdfIS = null;
    com.adobe.aemfd.docmanager.Document generatedDocument = null;
    // get the pdf file
    javax.servlet.http.Part pdfPart = request.getPart("pdfFile");
    pdfIS = pdfPart.getInputStream();
    com.adobe.aemfd.docmanager.Document pdfDocument = new com.adobe.aemfd.docmanager.Document(pdfIS);


// encrypt the document with permssions password. You can only print this document
    PasswordEncryptionOptionSpec poSpec = new PasswordEncryptionOptionSpec();    
    poSpec.setCompatability(PasswordEncryptionCompatability.ACRO_X);
    poSpec.setEncryptOption(PasswordEncryptionOption.ALL);
    List<PasswordEncryptionPermission> permissionList = new ArrayList<PasswordEncryptionPermission>();
    permissionList.add(PasswordEncryptionPermission.PASSWORD_PRINT_LOW);
    //hardcoding passwords into code is for demonstration purposes only.In real life scenarios the password is sourced from a secure location
    poSpec.setPermissionPassword("adobe");
    poSpec.setPermissionsRequested(permissionList);
    EncryptionOptions encryptionOptions = EncryptionOptions.getInstance();
    encryptionOptions.setEncryptionType(com.adobe.fd.docassurance.client.api.DocAssuranceServiceOperationTypes.ENCRYPT_WITH_PASSWORD);
    encryptionOptions.setPasswordEncryptionOptionSpec(poSpec);
    com.adobe.fd.docassurance.client.api.DocAssuranceService docAssuranceService = sling.getService(com.adobe.fd.docassurance.client.api.DocAssuranceService.class);
    com.adobe.aemfd.docmanager.Document securedDocument = docAssuranceService.secureDocument(pdfDocument,encryptionOptions,null,null,null);
    securedDocument.copyToFile(new java.io.File(filePath));
    out.println("Document encrypted and saved to " +filePath);
%>
```


**在系统上测试示例包**

[使用AEM包管理器下载并安装包](assets/encryptpdf.zip)

**安装包后，将以下URL添加到AdobeGranite CSRF筛选器的OSGi配置允许列表：**

1. [登录到configMgr](http://localhost:4502/system/console/configMgr)
1. 搜索AdobeGranite CSRF筛选器
1. 在排除的部分中添加以下路径并保存
1. /content/AemFormsSamples/encrypt

## 测试样本

可通过多种方法来测试示例代码。 最快、最轻松的是使用Postman应用程序。 Postman允许您向服务器发出POST请求。以下屏幕截图显示了post请求工作所需的请求参数。 在提交请求之前，请确保指定适当的授权类型。

![encrypt-pdf-postman](assets/encrypt-pdf-postman.png)
