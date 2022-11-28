---
title: 自适应Forms中的预填充服务
description: 通过从后端数据源获取数据来预填充自适应表单。
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: f2c324a3-cbfa-4942-b3bd-dc47d8a3f7b5
last-substantial-update: 2021-11-27T00:00:00Z
source-git-commit: 381812397fa7d15f6ee34ef85ddf0aa0acc0af42
workflow-type: tm+mt
source-wordcount: '443'
ht-degree: 0%

---

# 在自适应Forms中使用预填充服务

您可以使用现有数据预填自适应表单的字段。 用户打开表单时，将预填这些字段的值。 有多种方法可预填自适应表单字段。 在本文中，我们将介绍如何使用AEM Forms预填充服务预填自适应表单。

要进一步了解预填充自适应表单的各种方法， [请遵循本文档](https://helpx.adobe.com/experience-manager/6-4/forms/using/prepopulate-adaptive-form-fields.html#AEMFormsprefillservice)

要使用预填充服务预填充自适应表单，必须创建一个实现 `com.adobe.forms.common.service.DataXMLProvider` 界面。 方法 `getDataXMLForDataRef` 将具有生成并返回自适应表单用于预填充字段的数据的逻辑。 在此方法中，您可以从任何源获取数据并返回数据文档的输入流。 以下示例代码提取登录用户的用户配置文件信息，并构建一个XML文档，其输入流被返回以供自适应表单使用。

在下面的代码片段中，我们有一个实现DataXMLProvider接口的类。 我们可以访问已登录的用户，然后获取已登录用户的配置文件信息。 然后，我们使用名为“data”的根节点元素创建XML文档，并将相应的元素附加到此数据节点。 一旦构建XML文档，将返回XML文档的输入流。

然后，将此类制成OSGi包并部署到AEM中。 部署包后，此预填充服务便可用作自适应表单的预填充服务。

```java
package com.aem.prefill.core;
import com.adobe.forms.common.service.DataXMLOptions;
import com.adobe.forms.common.service.DataXMLProvider;
import com.adobe.forms.common.service.FormsException;
import java.io.ByteArrayInputStream;
import java.io.ByteArrayOutputStream;
import java.io.FileOutputStream;
import java.io.InputStream;
import javax.jcr.Session;
import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.transform.Transformer;
import javax.xml.transform.TransformerFactory;
import javax.xml.transform.dom.DOMSource;
import javax.xml.transform.stream.StreamResult;
import org.apache.jackrabbit.api.JackrabbitSession;
import org.apache.jackrabbit.api.security.user.Authorizable;
import org.apache.jackrabbit.api.security.user.UserManager;
import org.apache.sling.api.resource.Resource;
import org.apache.sling.api.resource.ResourceResolver;
import org.osgi.service.component.annotations.Component;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.w3c.dom.Document;
import org.w3c.dom.Element;

@Component
public class PrefillAdaptiveForm implements DataXMLProvider {
  private static final Logger log = LoggerFactory.getLogger(PrefillAdaptiveForm.class);

  @Override
  public String getServiceDescription() {

    return "Custom AEM Forms PreFill Service";
  }

  @Override
  public String getServiceName() {

    return "CustomAemFormsPrefillService";
  }

  @Override
  public InputStream getDataXMLForDataRef(DataXMLOptions dataXmlOptions) throws FormsException {
    InputStream xmlDataStream = null;
    Resource aemFormContainer = dataXmlOptions.getFormResource();
    ResourceResolver resolver = aemFormContainer.getResourceResolver();
    Session session = (Session) resolver.adaptTo(Session.class);
    try {
      UserManager um = ((JackrabbitSession) session).getUserManager();
      Authorizable loggedinUser = um.getAuthorizable(session.getUserID());
      log.debug("The path of the user is" + loggedinUser.getPath());
      DocumentBuilderFactory docFactory = DocumentBuilderFactory.newInstance();
      DocumentBuilder docBuilder = docFactory.newDocumentBuilder();
      Document doc = docBuilder.newDocument();
      Element rootElement = doc.createElement("data");
      doc.appendChild(rootElement);

      if (loggedinUser.hasProperty("profile/givenName")) {
        Element firstNameElement = doc.createElement("fname");
        firstNameElement.setTextContent(loggedinUser.getProperty("profile/givenName")[0].getString());
        rootElement.appendChild(firstNameElement);
        log.debug("Created firstName Element");
      }

      if (loggedinUser.hasProperty("profile/familyName")) {
        Element lastNameElement = doc.createElement("lname");
        lastNameElement.setTextContent(loggedinUser.getProperty("profile/familyName")[0].getString());
        rootElement.appendChild(lastNameElement);
        log.debug("Created lastName Element");

      }
      if (loggedinUser.hasProperty("profile/email")) {
        Element emailElement = doc.createElement("email");
        emailElement.setTextContent(loggedinUser.getProperty("profile/email")[0].getString());
        rootElement.appendChild(emailElement);
        log.debug("Created email Element");

      }

      TransformerFactory transformerFactory = TransformerFactory.newInstance();
      ByteArrayOutputStream outputStream = new ByteArrayOutputStream();
      Transformer transformer = transformerFactory.newTransformer();
      DOMSource source = new DOMSource(doc);
      StreamResult outputTarget = new StreamResult(outputStream);
      TransformerFactory.newInstance().newTransformer().transform(source, outputTarget);
      if (log.isDebugEnabled()) {
        FileOutputStream output = new FileOutputStream("afdata.xml");
        StreamResult result = new StreamResult(output);
        transformer.transform(source, result);
      }

      xmlDataStream = new ByteArrayInputStream(outputStream.toByteArray());
      return xmlDataStream;
    } catch (Exception e) {
      log.error("The error message is " + e.getMessage());
    }
    return null;

  }

}
```

要在服务器上测试此功能，请执行以下操作

* 确保已登录 [用户配置文件](http://localhost:4502/security/users.html) 信息已填写。 示例会查找已登录用户的FirstName、LastName和Email属性。
* [将zip文件的内容下载并解压缩到您的计算机中](assets/prefillservice.zip)
* 使用 [AEM web控制台](http://localhost:4502/system/console/bundles)
* 使用创建 |从 [表单和文档部分](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* 确保 [表单](http://localhost:4502/editor.html/content/forms/af/prefill.html) 正在使用 **&quot;自定义AEM Forms PreFill服务&quot;** 作为预填充服务。 这可以通过 **表单容器** 中。
* [预览表单](http://localhost:4502/content/dam/formsanddocuments/prefill/jcr:content?wcmmode=disabled). 您应会看到表单中填充了正确的值。

>[!NOTE]
>
>如果已为com.aem.prefill.core.PrefillAdaptiveForm启用调试，则生成的xml数据文件将写入您的AEM服务器安装文件夹中。

