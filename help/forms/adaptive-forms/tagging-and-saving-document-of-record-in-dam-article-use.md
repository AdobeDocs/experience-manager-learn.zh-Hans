---
title: 在DAM中标记和存储AEM Forms DoR
description: 本文将介绍在AEM DAM中存储和标记AEM Forms生成的DoR的用例。 文档的标记是根据提交的表单数据完成的。
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 832f04b4-f22f-4cf9-8136-e3c1081de7a9
source-git-commit: 55583effd0400bac2e38756483d69f5bd114cb21
workflow-type: tm+mt
source-wordcount: '612'
ht-degree: 0%

---

# 在DAM中标记和存储AEM Forms DoR {#tagging-and-storing-aem-forms-dor-in-dam}

本文将介绍在AEM DAM中存储和标记AEM Forms生成的DoR的用例。 文档的标记是根据提交的表单数据完成的。

客户的常见要求是在AEM DAM中存储并标记AEM Forms生成的记录文档(DoR)。 文档的标记必须基于自适应Forms提交的数据。 例如，如果提交数据中的雇佣状态为“已停用”，则我们希望使用“已停用”标记来标记文档，并将该文档存储在DAM中。

用例如下：

* 用户填写自适应表单。 在自适应表单中，捕获用户的婚姻状态（前单身）和就业状态（前已退休）。
* 提交表单时，会触发AEM工作流。 此工作流会使用婚姻状态（单个）和雇佣状态（已停用）标记文档，并将文档存储在DAM中。
* 文档存储到DAM中后，管理员应该能够通过这些标记来搜索文档。 例如，在“单个”或“已停用”上搜索将获取相应的DoR。

为了满足此用例的要求，编写了自定义流程步骤。 在此步骤中，我们将从提交的数据中获取相应数据元素的值。 然后，使用此值构建标记拼贴。 例如，如果婚姻状态元素的值为“Single”，则标记标题将变为**Peak:EmploymentStatus/Single。 **使用TagManager API ，我们会找到标记并将标记应用到DoR。

以下是用于在AEM DAM中标记和存储记录文档的完整代码。

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

要使此示例在您的系统上工作，请按照下面列出的步骤操作：
* [部署Developmingwithserviceuser包](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [下载和部署setvalue包](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). 这是自定义OSGI包，用于从提交的表单数据中设置标记。

* [下载示例自适应表单](assets/tag-and-store-in-dam-assets.zip)

* [转到Forms和文档](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)

* 单击Create |文件上传和上传示例adaptiveform.zip

* [导入文章资产](assets/tag-and-store-in-dam-assets.zip) 使用AEM包管理器
* 打开 [预览模式中的示例表单](http://localhost:4502/content/dam/formsanddocuments/summit/peakform/jcr:content?wcmmode=disabled). 填写人员部分并提交表单。
* [导航到DAM中的“峰值”文件夹](http://localhost:4502/assets.html/content/dam/Peak). 您应会在Peak文件夹中看到DoR。 检查文档的属性。 应正确标记。
恭喜你!! 您已成功在系统上安装示例

* 让我们来探索 [工作流](http://localhost:4502/editor.html/conf/global/settings/workflow/models/TagAndStoreDoRinDAM.html) 表单提交时触发的URL。
* 工作流中的第一步是通过关联申请人姓名和居住县来创建唯一的文件名。
* 工作流的第二步传递标记层次结构和需要标记的表单字段元素。 流程步骤从提交的数据中提取值，并构建需要标记文档的标记标题。
* 如果要将DoR存储在DAM的其他文件夹中，请使用下面屏幕截图中指定的配置属性指定文件夹位置。

其他两个参数特定于“自适应表单提交”选项中指定的“DoR”和“数据文件路径”。 请确保此处指定的值与自适应表单提交选项中指定的值匹配。

![标记多尔](assets/tag_dor_service_configuration.gif)
