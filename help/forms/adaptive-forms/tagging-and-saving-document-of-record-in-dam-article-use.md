---
title: 在DAM中标记和存储AEM Forms DoR
description: 本文将介绍在AEM DAM中存储和标记AEM Forms生成的DoR的用例。 文档的标记是根据提交的表单数据完成的。
feature: Adaptive Forms
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
exl-id: 832f04b4-f22f-4cf9-8136-e3c1081de7a9
last-substantial-update: 2019-03-20T00:00:00Z
duration: 191
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '582'
ht-degree: 0%

---

# 在DAM中标记和存储AEM Forms DoR {#tagging-and-storing-aem-forms-dor-in-dam}

本文将介绍在AEM DAM中存储和标记AEM Forms生成的DoR的用例。 文档的标记是根据提交的表单数据完成的。

客户的一个常见问题是，在AEM DAM中存储和标记AEM Forms生成的记录文档(DoR)。 文档的标记需要基于自适应Forms提交的数据。 例如，如果提交数据中的雇用状态为“已停用”，则我们希望使用“已停用”标记标记文档并将文档存储在DAM中。

用例如下所示：

* 用户填写自适应表单。 在自适应表单中，记录用户的婚姻状况（如单身）和就业状况（如已退休）。
* 在提交表单时，会触发AEM工作流。 此工作流使用婚姻状态（单身）和就业状态（已停用）标记文档，并将文档存储在DAM中。
* 文档存储在DAM中后，管理员应该能够按这些标记搜索文档。 例如，搜索“单个”或“已停用”将获取相应的DoR。

为了满足此用例，编写了一个自定义流程步骤。 在此步骤中，我们从提交的数据中提取相应数据元素的值。 然后，我们使用此值构建标记拼贴。 例如，如果婚姻状况元素的值为“Single”，则标记标题将变为**Peak：EmploymentStatus/Single。 **使用TagManager API ，我们找到标记并将标记应用于DoR。

以下是在AEM DAM中标记和存储记录文档的完整代码。

```java
package com.aemforms.setvalue.core;
import java.io.InputStream;
import javax.jcr.Node;
import javax.jcr.Session;
import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.xpath.XPath;
import org.apache.sling.api.resource.Resource;
import org.apache.sling.api.resource.ResourceResolver;
import org.osgi.framework.Constants;
import org.osgi.service.component.annotations.Activate;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.metatype.annotations.Designate;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.w3c.dom.Document;
import com.adobe.granite.workflow.WorkflowException;
import com.adobe.granite.workflow.WorkflowSession;
import com.adobe.granite.workflow.exec.WorkItem;
import com.adobe.granite.workflow.exec.WorkflowData;
import com.adobe.granite.workflow.exec.WorkflowProcess;
import com.adobe.granite.workflow.metadata.MetaDataMap;
import com.adobe.granite.workflow.model.WorkflowModel;
import com.day.cq.tagging.Tag;
import com.day.cq.tagging.TagManager;

@Component(property = {
   Constants.SERVICE_DESCRIPTION + "=Tag and Store Dor in DAM",
   Constants.SERVICE_VENDOR + "=Adobe Systems",
   "process.label" + "=Tag and Store Dor in DAM"
})
@Designate(ocd = TagDorServiceConfiguration.class)
public class TagAndStoreDoRinDAM implements WorkflowProcess
{
   private static final Logger log = LoggerFactory.getLogger(TagAndStoreDoRinDAM.class);

   private TagDorServiceConfiguration serviceConfig;
   @Activate
   public void activate(TagDorServiceConfiguration config)
   {
      this.serviceConfig = config;
   }
   @Override
   public void execute(WorkItem workItem, WorkflowSession workflowSession, MetaDataMap arg2) throws WorkflowException
   {
       log.debug("The process arguments passed ..." + arg2.get("PROCESS_ARGS", "string").toString());
      String params = arg2.get("PROCESS_ARGS", "string").toString();
      WorkflowModel wfModel = workflowSession.getModel("/var/workflow/models/dam/update_asset");
      // Read the Tag DoR service configuration
      String damFolder = serviceConfig.damFolder();
      String dorPDFName = serviceConfig.dorPath();
      String dataXmlFile = serviceConfig.dataFilePath();
      log.debug("The Data Xml File is ..." + dataXmlFile + "DorPDFName" + dorPDFName);
      // Read the arguments passed to this workflow step
      String parameters[] = params.split(",");
      log.debug("The %%%% length of parameters is " + parameters.length);
      Tag[] tagArray = new Tag[parameters.length];
      WorkflowData wfData = workItem.getWorkflowData();
      String dorFileName = (String) wfData.getMetaDataMap().get("filename");
      log.debug("The dorFileName is ..." + dorFileName);
      String payloadPath = workItem.getWorkflowData().getPayload().toString();
      String dataFilePath = payloadPath + "/" + dataXmlFile + "/jcr:content";
      String dorDocumentPath = payloadPath + "/" + dorPDFName + "/jcr:content";
      log.debug("Data File Path" + dataFilePath);
      log.debug("Dor File Path" + dorDocumentPath);
      Session session = workflowSession.adaptTo(Session.class);
      ResourceResolver resourceResolver = workflowSession.adaptTo(ResourceResolver.class);
      com.day.cq.dam.api.AssetManager assetMgr = resourceResolver.adaptTo(com.day.cq.dam.api.AssetManager.class);
      DocumentBuilderFactory factory = null;
      DocumentBuilder builder = null;
      Document xmlDocument = null;
      Node xmlDataNode = null;
      Node dorDocumentNode = null;

      try
      {
         // create org.w3c.dom.Document object from submitted form data
         xmlDataNode = session.getNode(dataFilePath);
         log.debug("xml Data Node" + xmlDataNode.getName());
         dorDocumentNode = session.getNode(dorDocumentPath);
         log.debug("DOR Document Node is " + dorDocumentNode.getName());
         InputStream xmlDataStream = xmlDataNode.getProperty("jcr:data").getBinary().getStream();
         InputStream dorInputStream = dorDocumentNode.getProperty("jcr:data").getBinary().getStream();
         XPath xPath = javax.xml.xpath.XPathFactory.newInstance().newXPath();
         factory = DocumentBuilderFactory.newInstance();
         builder = factory.newDocumentBuilder();
         xmlDocument = builder.parse(xmlDataStream);
         String newFile = "/content/dam/" + damFolder + "/" + dorFileName;
         log.debug("the new file is ..." + newFile);
         // Store the DoR in DAM
         assetMgr.createAsset(newFile, dorInputStream, "application/pdf", true);
         WorkflowData wfDataLoad = workflowSession.newWorkflowData("JCR_PATH", newFile);
         log.debug("Wrote the document to DAM" + newFile);
         TagManager tagManager = resourceResolver.adaptTo(TagManager.class);
         Resource pdfDocumentNode = resourceResolver.getResource(newFile);
         Resource metadata = pdfDocumentNode.getChild("jcr:content/metadata");
         // Fetch the xml elements from the xml document
         for (int i = 0; i < parameters.length; i++)
            {
                String tagTitle = parameters[i].split("=")[0];
                log.debug("The tag title is" + tagTitle);
                String nameOfNode = parameters[i].split("=")[1];
                org.w3c.dom.Node xmlElement = (org.w3c.dom.Node) xPath.compile(nameOfNode).evaluate(xmlDocument, javax.xml.xpath.XPathConstants.NODE);
                log.debug("###The value data node is " + xmlElement.getTextContent());
                Tag tagFound = tagManager.resolveByTitle(tagTitle + xmlElement.getTextContent());
                log.debug("The tag found was ..." + tagFound.getPath());
                tagArray[i] = tagFound;
            }
         tagManager.setTags(metadata, tagArray, true);
         workflowSession.startWorkflow(wfModel, wfDataLoad);
         log.debug("Workflow started");
         log.debug("Done setting tags");
         xmlDataStream.close();
         dorInputStream.close();
      } catch (Exception e)
            {
                 log.debug("The error message is " + e.getMessage());
            }

   }

}
```

要使此示例在您的系统上正常工作，请按照以下步骤操作：
* [部署Developingwithserviceuser捆绑包](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [下载并部署setvalue包](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)。 这是自定义OSGI捆绑包，用于从提交的表单数据设置标记。

* [下载自适应表单示例](assets/tag-and-store-in-dam-adaptive-form.zip)

* [转到Forms和文档](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)

* 单击“创建” | 文件上传并上传tag-and-store-in-dam-adaptive-form.zip

* [使用AEM包管理器导入文章资源](assets/tag-and-store-in-dam-assets.zip)
* 在预览模式](http://localhost:4502/content/dam/formsanddocuments/tagandstoreindam/jcr:content?wcmmode=disabled)中打开[示例表单。 **填写所有字段**&#x200B;并提交表单。
* [导航到DAM中的峰值文件夹](http://localhost:4502/assets.html/content/dam/Peak)。 您应该会在Peak文件夹中看到DoR。 检查文档的属性。 应该对其进行适当的标记。
恭喜!! 您已在系统上成功安装示例

* 让我们浏览在提交表单时触发的[工作流](http://localhost:4502/editor.html/conf/global/settings/workflow/models/TagAndStoreDoRinDAM.html)。
* 工作流中的第一个步骤通过连接申请人姓名和居住地县创建唯一的文件名。
* 工作流的第二步是传递标记层次结构和需要标记的表单字段元素。 处理步骤从提交的数据中提取值，并构建需要标记文档的标记标题。
* 如果要将DoR存储在DAM的其他文件夹中，请使用以下屏幕快照中指定的配置属性指定文件夹位置。

其他两个参数专用于在“自适应表单”提交选项中指定的DoR和数据文件路径。 请确保您在此处指定的值与您在自适应表单提交选项中指定的值匹配。

![标记Dor](assets/tag_dor_service_configuration.gif)
