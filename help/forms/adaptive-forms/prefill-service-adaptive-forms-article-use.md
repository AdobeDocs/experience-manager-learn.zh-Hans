---
title: 自适应Forms中的预填充服务
description: 通过从后端数据源获取数据，预填充自适应表单。
feature: Adaptive Forms
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Intermediate
exl-id: f2c324a3-cbfa-4942-b3bd-dc47d8a3f7b5
last-substantial-update: 2021-11-27T00:00:00Z
duration: 129
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '424'
ht-degree: 0%

---

# 在自适应Forms中使用预填充服务

您可以使用现有数据预填自适应表单的字段。 当用户打开表单时，这些字段的值会预先填充。 预填充自适应表单字段的方法有多种。 在本文中，我们将介绍如何使用AEM Forms预填充服务预填充自适应表单。

要了解有关预填充自适应表单的各种方法的更多信息，[请遵循此文档](https://helpx.adobe.com/cn/experience-manager/6-4/forms/using/prepopulate-adaptive-form-fields.html#AEMFormsprefillservice)

要使用预填充服务预填充自适应表单，您必须创建一个实现`com.adobe.forms.common.service.DataXMLProvider`接口的类。 方法`getDataXMLForDataRef`将具有生成和返回自适应表单用于预填充字段的数据的逻辑。 在此方法中，您可以从任何源获取数据并返回数据文档的输入流。 以下示例代码获取登录用户的用户配置文件信息，并构造一个XML文档，该文档的输入流返回给自适应表单使用。

在下面的代码片段中，我们有一个实现DataXMLProvider接口的类。 我们获得对登录用户的访问权限，然后获取登录用户的配置文件信息。 然后，我们创建具有名为“data”的根节点元素的XML文档，并将相应的元素附加到此数据节点。 一旦构造了XML文档，就返回XML文档的输入流。

然后，将此类制作为OSGi捆绑包并部署到AEM中。 部署捆绑包后，此预填充服务即可用作自适应表单的预填充服务。

>[!NOTE]
>
>您可以使用xml或json数据预填充表单，方法是本文中所列的方法。

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

要在您的服务器上测试此功能，请执行以下操作

* 确保已填写登录[用户的配置文件](http://localhost:4502/security/users.html)信息。 该示例查找登录用户的FirstName、LastName和Email属性。
* [将zip文件的内容下载并解压缩到您的计算机上](assets/prefillservice.zip)
* 使用[AEM Web控制台](http://localhost:4502/system/console/bundles)部署prefill.core-1.0.0-SNAPSHOT捆绑包
* 使用“创建”导入自适应表单 | 从[FormsAndDocuments节](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)上载文件
* 确保[表单](http://localhost:4502/editor.html/content/forms/af/prefill.html)使用&#x200B;**“自定义AEM Forms预填充服务”**&#x200B;作为预填充服务。 这可以从&#x200B;**表单容器**&#x200B;部分的配置属性中验证。
* [预览表单](http://localhost:4502/content/dam/formsanddocuments/prefill/jcr:content?wcmmode=disabled)。 您应该会看到表单已填入正确的值。

>[!NOTE]
>
>如果已为com.aem.prefill.core.PrefillAdaptiveForm启用了调试，则生成的xml数据文件将写入AEM服务器安装文件夹。

