---
title: 在AEM Forms Workflow中将逗号分隔字符串转换为字符串数组
description: 当表单数据模型具有字符串数组作为输入参数之一时，您需要先消息传递从自适应表单的提交操作生成的数据，然后再调用表单数据模型的提交操作。
feature: Adaptive Forms
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Intermediate
jira: KT-8507
exl-id: 9ad69407-2413-416f-9cec-43f88989b31d
last-substantial-update: 2021-06-09T00:00:00Z
duration: 115
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '340'
ht-degree: 0%

---

# 将逗号分隔的字符串转换为字符串数组 {#setting-value-of-json-data-element-in-aem-forms-workflow}

当您的表单基于表单数据模型，该模型具有字符串数组作为输入参数时，您需要处理提交的自适应表单数据以插入字符串数组。 例如，如果您已将复选框字段绑定到字符串数组类型的表单数据模型元素，则复选框字段中的数据将为逗号分隔的字符串格式。 下面列出的示例代码说明了如何将逗号分隔的字符串替换为字符串数组。

## 创建流程步骤

我们希望工作流执行特定逻辑时，会在AEM工作流中使用流程步骤。 流程步骤可以与ECMA脚本或OSGi服务相关联。 我们的自定义流程步骤执行OSGi服务。

提交的数据采用以下格式。 businessUnits元素的值是以逗号分隔的字符串，需要转换为字符串数组。

![提交的数据](assets/submitted-data-string.png)

与表单数据模型关联的rest端点的输入数据需要字符串数组，如此屏幕快照中所示。 流程步骤中的自定义代码会将提交的数据转换为正确的格式。

![fdm-string-array](assets/string-array-fdm.png)

我们将JSON对象路径和元素名称传递给流程步骤。 流程步骤中的代码会将元素的逗号分隔值替换为一个字符串数组。
![process-step](assets/create-string-array.png)

>[!NOTE]
>
>确保自适应表单提交选项中的数据文件路径设置为“Data.xml”。 这是因为流程步骤中的代码将在有效负荷文件夹下查找名为Data.xml的文件。

## 流程步骤代码

```java
import java.io.BufferedReader;
import java.io.ByteArrayInputStream;
import java.io.InputStream;
import java.io.InputStreamReader;

import javax.jcr.Binary;
import javax.jcr.Node;
import javax.jcr.Session;

import org.osgi.framework.Constants;
import org.osgi.service.component.annotations.Component;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import com.adobe.granite.workflow.WorkflowException;
import com.adobe.granite.workflow.WorkflowSession;
import com.adobe.granite.workflow.exec.WorkItem;
import com.adobe.granite.workflow.exec.WorkflowProcess;
import com.adobe.granite.workflow.metadata.MetaDataMap;
import com.google.gson.JsonArray;
import com.google.gson.JsonObject;
import com.google.gson.JsonParser;

@Component(property = {
    Constants.SERVICE_DESCRIPTION + "=Create String Array",
    Constants.SERVICE_VENDOR + "=Adobe Systems",
    "process.label" + "=Replace comma seperated string with string array"
})

public class CreateStringArray implements WorkflowProcess {
    private static final Logger log = LoggerFactory.getLogger(CreateStringArray.class);
    @Override
    public void execute(WorkItem workItem, WorkflowSession workflowSession, MetaDataMap arg2) throws WorkflowException {
        log.debug("The string I got was ..." + arg2.get("PROCESS_ARGS", "string").toString());
        String[] arguments = arg2.get("PROCESS_ARGS", "string").toString().split(",");
        String objectName = arguments[0];
        String propertyName = arguments[1];

        String objects[] = objectName.split("\\.");
        System.out.println("The params is " + propertyName);
        log.debug("The params string is " + objectName);
        String payloadPath = workItem.getWorkflowData().getPayload().toString();
        log.debug("The payload  in set Elmement Value in Json is  " + workItem.getWorkflowData().getPayload().toString());
        String dataFilePath = payloadPath + "/Data.xml/jcr:content";
        Session session = workflowSession.adaptTo(Session.class);
        Node submittedDataNode = null;
        try {
            submittedDataNode = session.getNode(dataFilePath);

            InputStream submittedDataStream = submittedDataNode.getProperty("jcr:data").getBinary().getStream();
            BufferedReader streamReader = new BufferedReader(new InputStreamReader(submittedDataStream, "UTF-8"));
            StringBuilder stringBuilder = new StringBuilder();

            String inputStr;
            while ((inputStr = streamReader.readLine()) != null)
                stringBuilder.append(inputStr);
            JsonParser jsonParser = new JsonParser();
            JsonObject jsonObject = jsonParser.parse(stringBuilder.toString()).getAsJsonObject();
            System.out.println("The json object that I got was " + jsonObject);
            JsonObject targetObject = null;

            for (int i = 0; i < objects.length - 1; i++) {
                System.out.println("The object name is " + objects[i]);
                if (i == 0) {
                    targetObject = jsonObject.get(objects[i]).getAsJsonObject();
                } else {
                    targetObject = targetObject.get(objects[i]).getAsJsonObject();

                }

            }

            System.out.println("The final object is " + targetObject.toString());
            String businessUnits = targetObject.get(propertyName).getAsString();
            System.out.println("The values of " + propertyName + " are " + businessUnits);

            JsonArray jsonArray = new JsonArray();

            String[] businessUnitsArray = businessUnits.split(",");
            for (String name: businessUnitsArray) {
                jsonArray.add(name);
            }

            targetObject.add(propertyName, jsonArray);
            System.out.println(" After updating the property " + targetObject.toString());
            InputStream is = new ByteArrayInputStream(jsonObject.toString().getBytes());
            System.out.println("The changed json data  is " + jsonObject.toString());
            Binary binary = session.getValueFactory().createBinary(is);
            submittedDataNode.setProperty("jcr:data", binary);
            session.save();

        } catch (Exception e) {
            System.out.println(e.getMessage());
        }

    }
}
```

可从此处[&#128279;](assets/CreateStringArray.CreateStringArray.core-1.0-SNAPSHOT.jar)下载示例包
