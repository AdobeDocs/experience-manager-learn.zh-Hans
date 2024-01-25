---
title: 实施自定义流程步骤
description: 使用自定义流程步骤将自适应表单附件写入文件系统
feature: Workflow
version: 6.5
topic: Development
role: Developer
level: Experienced
exl-id: 879518db-3f05-4447-86e8-5802537584e5
last-substantial-update: 2021-06-09T00:00:00Z
duration: 234
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '769'
ht-degree: 0%

---

# 自定义流程步骤

本教程面向需要实施自定义流程步骤的AEM Forms客户。 流程步骤可以执行ECMA脚本或调用自定义java代码来执行操作。 本教程将说明实施由流程步骤执行的WorkflowProcess所需的步骤。

实施自定义流程步骤的主要原因是为了扩展AEM Workflow。 例如，如果在工作流模型中使用AEM Forms组件，则可能需要执行以下操作

* 将自适应表单附件保存到文件系统
* 处理提交的数据

要完成上述用例，您通常会编写一个在流程步骤中执行的OSGi服务。

## 创建Maven项目

第一步是使用相应的AdobeMaven原型创建一个Maven项目。 此页面中列出了详细步骤 [文章](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/creating-your-first-osgi-bundle/create-your-first-osgi-bundle.html). 将您的maven项目导入到eclipse中后，您就可以开始编写可在流程步骤中使用的第一个OSGi组件了。


### 创建实现WorkflowProcess的类

在eclipse IDE中打开maven项目。 展开 **projectname** > **核心** 文件夹。 展开src/main/java文件夹。 您应该看到以“core”结尾的包。 创建在此包中实现WorkflowProcess的Java类。 您需要覆盖execute方法。 execute方法的签名如下public void execute(WorkItem workItem， WorkflowSession workflowSession， MetaDataMap processArguments)throws WorkflowException执行方法提供对以下3个变量的访问

**工作项**：workItem变量将授予对与工作流相关的数据的访问权限。 提供了公共API文档 [此处。](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/reference-materials/diff-previous/changes/com.adobe.granite.workflow.WorkflowSession.html)

**Workflowsession**：此workflowSession变量使您能够控制工作流。 提供了公共API文档 [此处](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/reference-materials/diff-previous/changes/com.adobe.granite.workflow.WorkflowSession.html)

**元数据映射**：与工作流关联的所有元数据。 传递给流程步骤的任何流程参数都可以使用MetaDataMap对象使用。[API文档](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/workflow/metadata/MetaDataMap.html)

在本教程中，我们将把添加到自适应表单的附件作为AEM Workflow的一部分写入文件系统。

为了完成此用例，编写了以下java类

让我们看一下此代码

```java
package com.learningaemforms.adobe.core;

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
    Constants.SERVICE_DESCRIPTION + "=Write Adaptive Form Attachments to File System",
    Constants.SERVICE_VENDOR + "=Adobe Systems",
    "process.label" + "=Save Adaptive Form Attachments to File System"
})
public class WriteFormAttachmentsToFileSystem implements WorkflowProcess {

    private static final Logger log = LoggerFactory.getLogger(WriteFormAttachmentsToFileSystem.class);
    @Reference
    QueryBuilder queryBuilder;

    @Override
    public void execute(WorkItem workItem, WorkflowSession workflowSession, MetaDataMap processArguments)
    throws WorkflowException {
        // TODO Auto-generated method stub
        log.debug("The string I got was ..." + processArguments.get("PROCESS_ARGS", "string").toString());
        String[] params = processArguments.get("PROCESS_ARGS", "string").toString().split(",");
        String attachmentsPath = params[0];
        String saveToLocation = params[1];
        log.debug("The seperator is" + File.separator);
        String payloadPath = workItem.getWorkflowData().getPayload().toString();
        Map<String, String> map = new HashMap<String, String> ();
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
                log.debug("Error saving file " + e.getMessage());
            }
```

第1行 — 定义组件的属性。 将OSGi组件与流程步骤关联时，您会看到process.label属性，如下面的某个屏幕截图所示。

行13-15 — 使用“，”分隔符拆分传递到此OSGi组件的进程参数。 随后，将从字符串数组中提取attachmentPath和saveToLocation的值。

* attachmentPath — 这是您在自适应表单中指定用于调用AEM Workflow的自适应表单提交操作时的位置。 这是您希望附件保存在AEM中的文件夹的名称，相对于工作流的负荷。

* saveToLocation — 这是您希望将附件保存在AEM服务器文件系统中的位置。

这两个值将作为进程参数传递，如下面的屏幕快照所示。

![流程步骤](assets/implement-process-step.gif)

QueryBuilder服务用于查询attachmentsPath文件夹下nt：file类型的节点。 其余代码遍历搜索结果以创建Document对象并将其保存到文件系统


>[!NOTE]
>
>由于我们使用的是AEM Forms专属的Document对象，因此需要在maven项目中包含该aemfd-client-sdk依赖项。 组ID为com.adobe.aemfd ，工件ID为aemfd-client-sdk。

#### 生成和部署

[按照此处所述构建捆绑包](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/creating-your-first-osgi-bundle/create-your-first-osgi-bundle.html)
[确保包已部署且处于活动状态](http://localhost:4502/system/console/bundles)

创建工作流模型。 在工作流模型中拖放流程步骤。 将流程步骤与“将自适应表单附件保存到文件系统”关联。

提供以逗号分隔的必要进程参数。 例如，“附件”，c：\\scrappp\\。 第一个参数是自适应表单附件将相对于工作流有效负载进行存储时的文件夹。 该值必须与配置自适应表单的提交操作时指定的值相同。 第二个参数是希望存储附件的位置。

创建自适应表单。 将文件附件组件拖放到表单上。 配置表单的提交操作，以调用在之前步骤中创建的工作流。 提供相应的附件路径。

保存设置。

预览表单。 添加几个附件并提交表单。 附件应保存在您在工作流中指定的位置的文件系统中。
