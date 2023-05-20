---
title: 更新資料庫中表單的簽章狀態
description: 使用AEM工作流程更新資料庫中已簽署表單的簽名狀態
feature: Adaptive Forms
version: 6.4,6.5
kt: 6888
thumbnail: 6888.jpg
topic: Development
role: Developer
level: Experienced
exl-id: 75852a4b-7008-4c65-bab1-cc5dbf525e20
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '116'
ht-degree: 2%

---

# 更新簽名狀態

當使用者完成簽署儀式時，就會觸發UpdateSignatureStatus工作流程。 以下是工作流程的流程

![主要工作流程](assets/update-signature.PNG)

更新簽章狀態為自訂程式步驟。
實作自訂流程步驟的主要原因是為了擴充AEM Workflow。 以下是用來更新簽名狀態的自訂程式碼。
此自訂流程步驟中的程式碼會參照SignMultipleForms服務。


```java
@Component(property = {
  Constants.SERVICE_DESCRIPTION + "=Update Signature Status in DB",
  Constants.SERVICE_VENDOR + "=Adobe Systems",
  "process.label" + "=Update Signature Status in DB"
})

public class UpdateSignatureStatusWorkflowStep implements WorkflowProcess {
  private static final Logger log = LoggerFactory.getLogger(UpdateSignatureStatusWorkflowStep.class);@Reference
  SignMultipleForms signMultipleForms;@Override
  public void execute(WorkItem workItem, WorkflowSession workflowSession, MetaDataMap args) throws WorkflowException {
    String payloadPath = workItem.getWorkflowData().getPayload().toString();
    String dataFilePath = payloadPath + "/Data.xml/jcr:content";
    Session session = workflowSession.adaptTo(Session.class);
    DocumentBuilderFactory factory = null;
    DocumentBuilder builder = null;
    Document xmlDocument = null;
    Node xmlDataNode = null;
    try {
      xmlDataNode = session.getNode(dataFilePath);
      InputStream xmlDataStream = xmlDataNode.getProperty("jcr:data").getBinary().getStream();
      factory = DocumentBuilderFactory.newInstance();
      builder = factory.newDocumentBuilder();
      xmlDocument = builder.parse(xmlDataStream);
      XPath xPath = javax.xml.xpath.XPathFactory.newInstance().newXPath();
      org.w3c.dom.Node node = (org.w3c.dom.Node) xPath.compile("/afData/afUnboundData/data/guid").evaluate(xmlDocument, javax.xml.xpath.XPathConstants.NODE);
      String guid = node.getTextContent();
      StringWriter writer = new StringWriter();
      IOUtils.copy(xmlDataStream, writer, StandardCharsets.UTF_8);
      System.out.println("After ioutils copy" + writer.toString());
      signMultipleForms.updateSignatureStatus(writer.toString(), guid);
    }
    catch(Exception e) {
      log.debug(e.getMessage());
    }

  }

}
```

## Assets

更新簽名狀態工作流程可以是 [已從此處下載](assets/update-signature-status-workflow.zip)

## 后续步骤

[自訂摘要步驟以顯示下一個要簽署的表單](./customize-summary-component.md)
