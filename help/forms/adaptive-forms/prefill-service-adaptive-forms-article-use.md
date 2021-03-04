---
title: 自适应Forms中的预填服务
seo-title: 自适应Forms中的预填服务
description: 通过从后端数据源获取数据来预填充自适应表单。
seo-description: 通过从后端数据源获取数据来预填充自适应表单。
sub-product: 表单
feature: 自适应表单
topics: integrations
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
uuid: 26a8cba3-7921-4cbb-a182-216064e98054
discoiquuid: 936ea5e9-f5f0-496a-9188-1a8ffd235ee5
topic: 开发
role: 开发人员
level: 中间
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '483'
ht-degree: 0%

---


# 使用自适应Forms中的预填充服务

您可以使用现有数据预填自适应表单的字段。 用户打开表单时，将预填这些字段的值。 可通过多种方式预填自适应表单域。 在本文中，我们将看到使用AEM Forms预填服务预填自适应表单。

要了解有关预填充自适应表单的各种方法的更多信息，请遵循本文档](https://helpx.adobe.com/experience-manager/6-4/forms/using/prepopulate-adaptive-form-fields.html#AEMFormsprefillservice)[

要使用预填服务预填自适应表单，您必须创建一个实现DataProvider接口的类。 方法getPrefillData将具有构建和返回自适应表单将使用的数据以预填充字段的逻辑。 在此方法中，您可以从任何源获取数据并返回数据文档的输入流。 以下示例代码获取已登录用户的用户用户档案信息，并构建其输入流被返回以被自适应表单使用的XML文档。

在下面的代码片断中，我们有一个实现DataProvider接口的类。 我们可以访问已登录的用户，然后获取已登录用户的用户档案信息。 然后，我们使用名为“data”的根节点元素创建XML文档，并将适当的元素附加到此数据节点。 构建XML文档后，将返回XML文档的输入流。

然后，将此类生成到OSGi包中并部署到AEM中。 部署捆绑包后，此预填服务便可用作自适应表单的预填服务。

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

* [将zip文件的内容下载并解压到您的计算机上](assets/prefillservice.zip)
* 确保已登录[用户的用户档案](http://localhost:4502/libs/granite/security/content/useradmin)信息已完全填写完毕。 这是样本工作所必需的。 该示例没有错误检查缺少的用户用户档案属性。
* 使用[AEM Web控制台](http://localhost:4502/system/console/bundles)部署捆绑包
* 使用XSD创建自适应表单
* 将“自定义Aem表单预填服务”关联为自适应表单的预填服务
* 将模式元素拖放到表单
* 预览表单

>[!NOTE]
>
>如果自适应表单基于XSD，请确保预填服务返回的XML文档与自适应表单所基于的XSD匹配。
>
>如果自适应表单不基于XSD，则您必须手动绑定字段。 例如，要将自适应表单域绑定到XML数据中的fname元素，您将在自适应表单域的绑定引用中使用`/data/fname`。

