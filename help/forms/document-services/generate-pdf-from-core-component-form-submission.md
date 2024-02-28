---
title: 使用基于核心组件的自适应表单中的数据生成PDF
description: 将基于核心组件的表单提交中的数据与工作流中的XDP模板合并
version: 6.5
feature: Forms Service
topic: Development
role: Developer
level: Experienced
jira: KT-15025
last-substantial-update: 2024-02-26T00:00:00Z
source-git-commit: acfd982ce471c8510cb59ed4d353f1d47dcfb5dc
workflow-type: tm+mt
source-wordcount: '324'
ht-degree: 0%

---

# 使用基于核心组件的表单提交中的数据生成PDF

以下是大写“核心组件”的修订文本：

典型场景涉及根据通过基于核心组件的自适应表单提交的数据生成PDF。 此数据始终采用JSON格式。 要使用渲染PDFAPI生成PDF，需要将JSON数据转换为XML格式。 此 `toString` 方法 `org.json.XML` 用于此转换。 欲知更多详情，请参见 [文档 `org.json.XML.toString` 方法](https://www.javadoc.io/doc/org.json/json/20171018/org/json/XML.html#toString-java.lang.Object-).

## 基于JSON架构的自适应表单

请按照以下步骤为您的自适应表单创建JSON架构：

### 为XDP生成示例数据

要简化该流程，请按照以下优化步骤操作：

1. 在AEM Forms设计器中打开XDP文件。
1. 导航到“文件”>“表单属性”>“预览”。
1. 选择“生成预览数据”。
1. 单击“生成”。
1. 分配有意义的文件名，例如 `form-data.xml`.

### 从XML数据生成JSON架构

您可以利用任何免费在线工具来 [将XML转换为JSON](https://jsonformatter.org/xml-to-jsonschema) 使用上一步中生成的XML数据。

### 用于将JSON转换为XML的自定义工作流进程

提供的代码将JSON转换为XML，并将生成的XML存储在名为的工作流进程变量中 `dataXml`.

```java
import org.slf4j.LoggerFactory;
import com.adobe.granite.workflow.WorkflowException;
import java.io.InputStream;
import java.io.BufferedReader;
import java.io.InputStreamReader;
import javax.jcr.Node;
import javax.jcr.Session;
import org.json.JSONObject;
import org.json.XML;
import org.slf4j.Logger;
import org.osgi.service.component.annotations.Component;
import com.adobe.granite.workflow.WorkflowSession;
import com.adobe.granite.workflow.exec.WorkItem;
import com.adobe.granite.workflow.exec.WorkflowProcess;
import com.adobe.granite.workflow.metadata.MetaDataMap;

@Component(property = {
    "service.description=Convert JSON to XML",
    "process.label=Convert JSON to XML"
})
public class ConvertJSONToXML implements WorkflowProcess {

    private static final Logger log = LoggerFactory.getLogger(ConvertJSONToXML.class);

    @Override
    public void execute(final WorkItem workItem, final WorkflowSession workflowSession, final MetaDataMap arg2) throws WorkflowException {
        String processArgs = arg2.get("PROCESS_ARGS", "string");
        log.debug("The process argument I got was " + processArgs);
        
        String submittedDataFile = processArgs;
        String payloadPath = workItem.getWorkflowData().getPayload().toString();
        log.debug("The payload in convert json to xml " + payloadPath);
        
        String dataFilePath = payloadPath + "/" + submittedDataFile + "/jcr:content";
        try {
            Session session = workflowSession.adaptTo(Session.class);
            Node submittedJsonDataNode = session.getNode(dataFilePath);
            InputStream jsonDataStream = submittedJsonDataNode.getProperty("jcr:data").getBinary().getStream();
            BufferedReader streamReader = new BufferedReader(new InputStreamReader(jsonDataStream, "UTF-8"));
            StringBuilder stringBuilder = new StringBuilder();
            String inputStr;
            while ((inputStr = streamReader.readLine()) != null) {
                stringBuilder.append(inputStr);
            }
            JSONObject submittedJson = new JSONObject(stringBuilder.toString());
            log.debug(submittedJson.toString());
            
            String xmlString = XML.toString(submittedJson);
            log.debug("The json converted to XML " + xmlString);
            
            MetaDataMap metaDataMap = workItem.getWorkflow().getWorkflowData().getMetaDataMap();
            metaDataMap.put("xmlData", xmlString);
        } catch (Exception e) {
            log.error("Error converting JSON to XML: " + e.getMessage(), e);
        }
    }
}
```

### 创建工作流

要处理表单提交，请创建包含两个步骤的工作流：

1. 初始步骤使用自定义过程将提交的JSON数据转换为XML。
1. 后续步骤通过将XML数据与XDPPDF结合来生成模板。

![json-to-xml](assets/json-to-xml-process-step.png)


## 部署示例代码

要在本地服务器上对此进行测试，请按照以下简化步骤操作：

1. [通过AEM OSGi Web控制台下载并安装自定义捆绑包](assets/convertJsonToXML.core-1.0.0-SNAPSHOT.jar).
1. [导入工作流包](assets/workflow_to_render_pdf.zip).
1. [导入自适应表单和XDP模板示例](assets/adaptive_form_and_xdp_template.zip).
1. [预览自适应表单](http://localhost:4502/content/dam/formsanddocuments/f23/jcr:content?wcmmode=disabled).
1. 填写一些表单字段。
1. 提交表单以启动AEM工作流。
1. 在工作流的有效负荷文件夹中找到渲染的PDF。

