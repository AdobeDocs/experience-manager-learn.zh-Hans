---
title: 通过对话框实施自定义流程步骤
description: 使用自定义流程步骤将自适应表单附件写入文件系统
feature: Workflow
version: Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
last-substantial-update: 2021-06-09T00:00:00Z
exl-id: 149d2c8c-bf44-4318-bba8-bec7e25da01b
duration: 135
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '446'
ht-degree: 0%

---

# 自定义流程步骤

本教程面向需要实施自定义工作流组件的AEM Forms客户。创建工作流组件的第一步是编写将与该工作流组件关联的Java代码。 在本教程中，我们将编写简单的java类，以将自适应表单附件存储到文件系统。此java代码将读取工作流组件中指定的参数。

编写java类并将该类部署为OSGi捆绑包时，需要执行以下步骤

## 创建Maven项目

第一步是使用适当的Adobe Maven原型创建一个Maven项目。 此[文章](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/creating-your-first-osgi-bundle/create-your-first-osgi-bundle.html)中列出了详细步骤。 将您的maven项目导入到eclipse中后，您就可以开始编写可在流程步骤中使用的第一个OSGi组件了。


### 创建实现WorkflowProcess的类

在eclipse IDE中打开maven项目。 展开&#x200B;**projectname** > **core**文件夹。 展开src/main/java文件夹。 您应该看到以“core”结尾的包。 创建在此包中实现WorkflowProcess的Java类。 您需要覆盖execute方法。 execute方法的签名如下
公共void execute(WorkItem workItem， WorkflowSession workflowSession， MetaDataMap processArguments)引发WorkflowException

在本教程中，我们将把添加到自适应表单的附件作为AEM Workflow的一部分写入文件系统。

为了完成此用例，编写了以下java类

让我们看一下此代码

```java
package com.mysite.core;
import java.io.File;
import java.io.InputStream;
import java.util.HashMap;
import java.util.Map;
import javax.jcr.Node;
import javax.jcr.Session;
import org.osgi.framework.Constants;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import com.adobe.aemfd.docmanager.Document;
import com.adobe.granite.workflow.WorkflowException;
import com.adobe.granite.workflow.WorkflowSession;
import com.adobe.granite.workflow.exec.WorkItem;
import com.adobe.granite.workflow.exec.WorkflowProcess;
import com.adobe.granite.workflow.metadata.MetaDataMap;
import com.day.cq.search.PredicateGroup;
import com.day.cq.search.Query;
import com.day.cq.search.QueryBuilder;
import com.day.cq.search.result.Hit;
import com.day.cq.search.result.SearchResult;
@Component(property = {
  Constants.SERVICE_DESCRIPTION + "=Custom component to wrtie form attachments to file system",
  Constants.SERVICE_VENDOR + "=Adobe Systems",
  "process.label" + "=Custom component to wrtie form attachments to file system"
})
public class WriteFormAttachmentsToFileSystem implements WorkflowProcess {

  private static final Logger log = LoggerFactory.getLogger(WriteFormAttachmentsToFileSystem.class);
  @Reference
  QueryBuilder queryBuilder;

  @Override
  public void execute(WorkItem workItem, WorkflowSession workflowSession, MetaDataMap metaDataMap)
  throws WorkflowException {

    String attachmentsPath = metaDataMap.get("attachmentsPath", String.class);

    log.debug("Got attachments path: " + attachmentsPath);
    String saveToLocation = metaDataMap.get("SaveToLocation", String.class);
    log.debug("Got save location: " + saveToLocation);

    log.debug("The seperator is" + File.separator);
    String payloadPath = workItem.getWorkflowData().getPayload().toString();
    Map < String, String > map = new HashMap < String, String > ();
    map.put("path", payloadPath + "/" + attachmentsPath);
    File saveLocationFolder = new File(saveToLocation);
    if (!saveLocationFolder.exists()) {
      saveLocationFolder.mkdirs();
    }

    map.put("type", "nt:file");
    Query query = queryBuilder.createQuery(PredicateGroup.create(map), workflowSession.adaptTo(Session.class));
    query.setStart(0);
    query.setHitsPerPage(20);

    SearchResult result = query.getResult();
    log.debug("Got  " + result.getHits().size() + " attachments ");
    Node attachmentNode = null;
    for (Hit hit: result.getHits()) {
      try {
        String path = hit.getPath();
        log.debug("The attachment title is  " + hit.getTitle() + " and the attachment path is  " + path);
        attachmentNode = workflowSession.adaptTo(Session.class).getNode(path + "/jcr:content");
        InputStream documentStream = attachmentNode.getProperty("jcr:data").getBinary().getStream();
        Document attachmentDoc = new Document(documentStream);
        attachmentDoc.copyToFile(new File(saveLocationFolder + File.separator + hit.getTitle()));
        attachmentDoc.close();
      } catch (Exception e) {
        log.error("Error saving file " + e.getMessage());
      }
    }
  }
}
```


* attachmentsPath — 这是您在自适应表单中指定用于调用AEM Workflow的自适应表单提交操作时的位置。 这是您希望附件保存在AEM中的文件夹的名称，相对于工作流的负荷。

* saveToLocation — 这是您希望将附件保存在AEM服务器文件系统中的位置。

这两个值将作为进程参数使用工作流组件的对话框传递

![ProcessStep](assets/custom-workflow-component.png)

QueryBuilder服务用于查询attachmentsPath文件夹下nt：file类型的节点。 其余代码遍历搜索结果以创建Document对象并将其保存到文件系统


>[!NOTE]
>
>由于我们使用的是AEM Forms专属的Document对象，因此需要在maven项目中包含该aemfd-client-sdk依赖项。

```xml
<dependency>
    <groupId>com.adobe.aemfd</groupId>
    <artifactId>aemfd-client-sdk</artifactId>
    <version>6.0.772</version>
</dependency>
```

#### 生成和部署

[按照此处所述生成包](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/creating-your-first-osgi-bundle/create-your-first-osgi-bundle.html)
[确保包已部署且处于活动状态](http://localhost:4502/system/console/bundles)

## 后续步骤

创建您的[自定义工作流组件](./custom-workflow-component.md)

