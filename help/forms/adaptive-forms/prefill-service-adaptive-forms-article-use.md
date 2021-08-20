---
title: 自适应Forms中的预填充服务
description: 通过从后端数据源获取数据来预填充自适应表单。
feature: 自适应表单
version: 6.4,6.5
topic: 开发
role: Developer
level: Intermediate
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '465'
ht-degree: 0%

---


# 在自适应Forms中使用预填充服务

您可以使用现有数据预填自适应表单的字段。 用户打开表单时，将预填这些字段的值。 有多种方法可预填自适应表单字段。 在本文中，我们将介绍如何使用AEM Forms预填充服务预填自适应表单。

要进一步了解预填充自适应表单的各种方法，请[阅读本文档](https://helpx.adobe.com/experience-manager/6-4/forms/using/prepopulate-adaptive-form-fields.html#AEMFormsprefillservice)

要使用预填充服务预填充自适应表单，您必须创建一个实现DataProvider接口的类。 getPrefillData方法将具有构建和返回自适应表单用来预填充字段的数据的逻辑。 在此方法中，您可以从任何源获取数据并返回数据文档的输入流。 以下示例代码获取登录用户的用户配置文件信息，并构建一个XML文档，其输入流被返回以供自适应表单使用。

在下面的代码片段中，我们有一个实现DataProvider接口的类。 我们可以访问已登录的用户，然后获取已登录用户的配置文件信息。 然后，我们使用名为“data”的根节点元素创建XML文档，并将相应的元素附加到此数据节点。 一旦构建XML文档，将返回XML文档的输入流。

然后，将此类制成OSGi包并部署到AEM中。 部署包后，此预填充服务便可用作自适应表单的预填充服务。

```java
public class PrefillAdaptiveForm implements DataProvider {
 private Logger logger = LoggerFactory.getLogger(PrefillAdaptiveForm.class);

 public String getServiceName() {
  return "Default Prefill Service";
 }
 
 public String getServiceDescription() {
  return "This is default prefill service to prefill adaptive form with user data";
 }
 
 public PrefillData getPrefillData(final DataOptions dataOptions) throws FormsException {
  PrefillData prefillData = new PrefillData() {
   public InputStream getInputStream() {
    return getData(dataOptions);
   }
   
   public ContentType getContentType() {
    return ContentType.XML;
   }
  };
  return prefillData;
 }

 private InputStream getData(DataOptions dataOptions) throws FormsException {  
  try {
   Resource aemFormContainer = dataOptions.getFormResource();
   ResourceResolver resolver = aemFormContainer.getResourceResolver();
   Session session = resolver.adaptTo(Session.class);
   UserManager um = ((JackrabbitSession) session).getUserManager();
   Authorizable loggedinUser = um.getAuthorizable(session.getUserID());
   DocumentBuilderFactory docFactory = DocumentBuilderFactory.newInstance();
   DocumentBuilder docBuilder = docFactory.newDocumentBuilder();
   Document doc = docBuilder.newDocument();
   Element rootElement = doc.createElement("data");
   doc.appendChild(rootElement);
   Element firstNameElement = doc.createElement("fname");
   firstNameElement.setTextContent(loggedinUser.getProperty("profile/givenName")[0].getString());
     .
     .
     .
   InputStream inputStream = new ByteArrayInputStream(rootElement.getTextContent().getBytes());
   return inputStream;
  } catch (Exception e) {
   logger.error("Error while creating prefill data", e);
   throw new FormsException(e);
  }
 }
}
```

要在服务器上测试此功能，请执行以下操作

* [将zip文件的内容下载并解压缩到您的计算机中](assets/prefillservice.zip)
* 确保已登录[用户的用户档案](http://localhost:4502/libs/granite/security/content/useradmin)信息已完全填写完毕。 这是样例工作所必需的。 在检查缺少的用户配置文件属性时，该示例没有任何错误。
* 使用[AEM Web控制台](http://localhost:4502/system/console/bundles)部署包
* 使用XSD创建自适应表单
* 将“自定义Aem表单预填充服务”关联为自适应表单的预填充服务
* 将架构元素拖放到表单中
* 预览表单

>[!NOTE]
>
>如果自适应表单基于XSD，请确保预填充服务返回的XML文档与自适应表单所基于的XSD相匹配。
>
>如果自适应表单不基于XSD，则必须手动绑定字段。 例如，要将自适应表单字段绑定到XML数据中的fname元素，您将在自适应表单字段的绑定引用中使用`/data/fname`。

