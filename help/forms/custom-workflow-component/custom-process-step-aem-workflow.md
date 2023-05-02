---
title: 使用对话框实施自定义流程步骤
description: 使用自定义流程步骤将自适应表单附件写入文件系统
feature: Workflow
version: 6.5
topic: Development
role: Developer
level: Experienced
last-substantial-update: 2021-06-09T00:00:00Z
exl-id: 149d2c8c-bf44-4318-bba8-bec7e25da01b
source-git-commit: 00257efe045eb85fb192bbb47f8e178cf909eb86
workflow-type: tm+mt
source-wordcount: '455'
ht-degree: 0%

---

# 自定义流程步骤

本教程面向需要实施自定义工作流组件的AEM Forms客户。创建工作流组件的第一步是编写将与工作流组件关联的java代码。 在本教程中，我们将编写简单的java类，以将自适应表单附件存储到文件系统中。此java代码将读取工作流组件中指定的参数。

需要以下步骤来编写java类并将该类部署为OSGi包

## 创建Maven项目

第一步是使用相应的AdobeMaven Archetype创建Maven项目。 下面列出了详细步骤 [文章](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/creating-your-first-osgi-bundle/create-your-first-osgi-bundle.html). 将maven项目导入到eclipse后，您便可以开始编写可在流程步骤中使用的第一个OSGi组件。


### 创建实现WorkflowProcess的类

在Eclipse IDE中打开maven项目。 展开 **projectname** > **核心** 文件夹。 展开src/main/java文件夹。 您应会看到以“core”结尾的包。 在此包中创建用于实现WorkflowProcess的Java类。 您需要覆盖执行方法。 执行方法的签名为：公共void execute(WorkItem workItem，WorkflowSession workflowSession，MetaDataMap processArguments)引发WorkflowException

在本教程中，我们将将添加到自适应表单的附件作为AEM工作流的一部分写入文件系统。

为了完成此用例，编写了以下java类

让我们看看此代码

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


* attachmentsPath — 这与您在自适应表单中指定的位置相同，当您配置自适应表单的提交操作以调用AEM工作流时。 这是您希望将附件保存在AEM中的文件夹的名称，相对于工作流的有效负载。

* saveToLocation — 这是您希望将附件保存到AEM服务器文件系统中的位置。

这两个值将使用工作流组件的对话框作为进程参数传递

![ProcessStep](assets/custom-workflow-component.png)

QueryBuilder服务用于查询attachmentsPath文件夹下nt:file类型的节点。 其余的代码通过搜索结果进行迭代以创建文档对象并将其保存到文件系统


>[!NOTE]
>
>由于我们使用的是特定于AEM Forms的Document对象，因此您需要在Maven项目中包含aemfd-client-sdk依赖项。

```xml
<dependency>
    <groupId>com.adobe.aemfd</groupId>
    <artifactId>aemfd-client-sdk</artifactId>
    <version>6.0.772</version>
</dependency>
```

#### 构建和部署

[按如下所述构建包](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/creating-your-first-osgi-bundle/create-your-first-osgi-bundle.html)
[确保包已部署并处于活动状态](http://localhost:4502/system/console/bundles)
