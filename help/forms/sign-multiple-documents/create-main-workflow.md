---
title: 创建主工作流以触发签名流程
description: 创建工作流以在数据库中存储要签名的表单
feature: 自适应表单
version: 6.4,6.5
thumbnail: 6887.jpg
kt: 6887
topic: 开发
role: Developer
level: Intermediate
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '165'
ht-degree: 2%

---


# 创建主工作流

当用户提交初始表单(**ReconfactForm**)时，将触发主工作流。 以下是工作流的流程

![主工作流](assets/main-workflow.PNG)

**将Forms存储** 到签名是自定义流程步骤。

实施自定义流程步骤的动机是扩展AEM工作流。 以下代码可实施自定义流程步骤。 代码会提取要签名的表单名称，并将提交的表单数据传递到SignMultipleForms服务中的`insertData`方法。 然后，`insertData`方法将行插入由数据源&#x200B;**aemformstutorial**&#x200B;标识的数据库中。

此自定义流程步骤中的代码引用了`SignMultipleForms`服务。



```java
package com.aem.forms.signmultipleforms;

import java.io.ByteArrayInputStream;
import java.io.ByteArrayOutputStream;
import java.io.InputStream;
import java.io.StringWriter;
import java.nio.charset.Charset;
import java.nio.charset.StandardCharsets;

import javax.jcr.Node;
import javax.jcr.Session;
import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.transform.TransformerFactory;
import javax.xml.transform.dom.DOMSource;
import javax.xml.transform.stream.StreamResult;
import javax.xml.xpath.XPath;

import org.apache.commons.io.IOUtils;
import org.osgi.framework.Constants;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.w3c.dom.Document;

import com.adobe.granite.workflow.WorkflowException;
import com.adobe.granite.workflow.WorkflowSession;
import com.adobe.granite.workflow.exec.WorkItem;
import com.adobe.granite.workflow.exec.WorkflowProcess;
import com.adobe.granite.workflow.metadata.MetaDataMap;

@Component(property = {
  Constants.SERVICE_DESCRIPTION + "=StoreFormsToSign",
  Constants.SERVICE_VENDOR + "=Adobe Systems",
  "process.label" + "=StoreFormsToSign"
})
public class StoreFormsToSignWorkflowStep implements WorkflowProcess {
  private static final Logger log = LoggerFactory.getLogger(StoreFormsToSignWorkflowStep.class);@Reference
  SignMultipleForms signMultipleForms;

  @Override
  public void execute(WorkItem workItem, WorkflowSession workflowSession, MetaDataMap arg2) throws WorkflowException {
    String payloadPath = workItem.getWorkflowData().getPayload().toString();
    log.debug("The payload  in StoreFormsToSign " + workItem.getWorkflowData().getPayload().toString());
    String dataFilePath = payloadPath + "/Data.xml/jcr:content";
    String serverURL = arg2.get("PROCESS_ARGS", "string").toString();
    Session session = workflowSession.adaptTo(Session.class);
    DocumentBuilderFactory factory = null;
    DocumentBuilder builder = null;
    Document xmlDocument = null;
    Node xmlDataNode = null;
    try {
      xmlDataNode = session.getNode(dataFilePath);
      InputStream xmlDataStream = xmlDataNode.getProperty("jcr:data").getBinary().getStream();
      XPath xPath = javax.xml.xpath.XPathFactory.newInstance().newXPath();
      factory = DocumentBuilderFactory.newInstance();
      builder = factory.newDocumentBuilder();
      xmlDocument = builder.parse(xmlDataStream);
      org.w3c.dom.Node node = (org.w3c.dom.Node) xPath.compile("/afData/afUnboundData/data/formsToSign").evaluate(xmlDocument, javax.xml.xpath.XPathConstants.NODE);
      log.debug("The form names to sign are  t" + node.getTextContent());
      String formNamesToSign[] = node.getTextContent().split(",");
      ByteArrayOutputStream outputStream = new ByteArrayOutputStream();
      DOMSource source = new DOMSource(xmlDocument);
      StreamResult outputTarget = new StreamResult(outputStream);
      TransformerFactory.newInstance().newTransformer().transform(source, outputTarget);
      InputStream inputStream = new ByteArrayInputStream(outputStream.toByteArray());
      StringWriter writer = new StringWriter();
      IOUtils.copy(inputStream, writer, Charset.defaultCharset());
      String formData = writer.toString();
      signMultipleForms.insertData(formNamesToSign, formData, serverURL, workItem, workflowSession);

  }
    catch(Exception e) {
      log.debug(e.getMessage());
    }

  }
}
```




## 资产

本文中使用的Sign Multiple Forms工作流可从此处](assets/sign-multiple-forms-workflows.zip)下载[

>[!NOTE]
> 请确保配置Day CQ Mail Service ，以发送电子邮件通知。 电子邮件模板也提供在上述包中。
