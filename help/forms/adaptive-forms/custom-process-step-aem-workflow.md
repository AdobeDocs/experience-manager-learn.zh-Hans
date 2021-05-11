---
title: 实施自定义进程步骤
seo-title: 实施自定义进程步骤
description: 使用自定义流程步骤将自适应表单附件写入文件系统
seo-description: 使用自定义流程步骤将自适应表单附件写入文件系统
feature: 工作流
topics: development
audience: developer
doc-type: tutorial
activity: understand
version: 6.5
topic: 开发
role: Developer
level: Experienced
translation-type: tm+mt
source-git-commit: dbc0a35ae96594fec1e10f411d57d2a3812c1cf2
workflow-type: tm+mt
source-wordcount: '833'
ht-degree: 0%

---


# 自定义流程步骤

本教程面向需要实施自定义流程步骤的AEM Forms客户。 进程步骤可以执行ECMA脚本或调用自定义Java代码以执行操作。 本教程将说明实现由进程步骤执行的WorkflowProcess所需的步骤。

实施自定义流程步骤的主要原因是扩展AEM工作流。 例如，如果您在工作流模型中使用AEM Forms组件，则可能需要执行以下操作

* 将自适应表单附件保存到文件系统
* 处理提交的数据

要完成上述用例，您通常会编写一个由进程步骤执行的OSGi服务。

## 创建Maven项目

第一步是使用适当的AdobeMaven Archetype创建一个主项目。 本[文章](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/create-your-first-osgi-bundle.html?lang=en)中列出了详细步骤。 将主项目导入Eclipse后，您就可以开始编写可在流程步骤中使用的第一个OSGi组件。


### 创建实现WorkflowProcess的类

在Eclipse IDE中打开主项目。 展开&#x200B;**projectname** > **core**文件夹。 展开src/main/java文件夹。 您应当看到以“core”结尾的包。 创建在此包中实现WorkflowProcess的Java类。 您需要覆盖execute方法。 execute方法的签名如下
public void execute(WorkItem workItem、WorkflowSession workflowSession、MetaDataMap processArguments)引发WorkflowException
execute方法允许访问以下3个变量

**工作项**:workItem变量将允许访问与工作流相关的数据。此处提供公共API文档[。](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/reference-materials/diff-previous/changes/com.adobe.granite.workflow.WorkflowSession.html)

**WorkflowSession**:此workflowSession变量将使您能够控制工作流。可在[此处](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/reference-materials/diff-previous/changes/com.adobe.granite.workflow.WorkflowSession.html)找到公共API文档

**MetaDataMap**:与工作流关联的所有元数据。传递到进程步骤的任何进程参数都可以使用MetaDataMap对象。[API文档](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/workflow/metadata/MetaDataMap.html)

在本教程中，我们将将添加到自适应表单的附件作为AEM工作流的一部分写入文件系统。

要完成此用例，编写了以下java类

让我们看看这个代码

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

第1行 — 定义组件的属性。 如以下某个屏幕截图所示，将OSGi组件与进程步骤关联时，将看到process.label属性。

第13-15行 — 传递给此OSGi组件的进程参数使用“，”分隔符进行拆分。 随后，将从字符串数组中提取attachmentPath和saveToLocation的值。

* attachmentPath — 这是您在自适应表单中指定的位置，当您配置自适应表单的提交操作以调用AEM工作流时。 这是您希望将附件保存在AEM中的文件夹的名称，该文件夹与工作流的有效负荷相关。

* saveToLocation — 这是您希望将附件保存在AEM服务器文件系统中的位置。

这两个值将作为进程参数传递，如以下屏幕截图所示。

![ProcessStep](assets/implement-process-step.gif)

QueryBuilder服务用于查询attachmentsPath文件夹下nt:file类型的节点。 其余的代码循环搜索结果，以创建文档对象并将其保存到文件系统


>[!NOTE]
>
>由于我们使用的是特定于AEM Forms的文档对象，因此您需要在您的主项目中包含aemfd-client-sdk依赖关系。 组ID是com.adobe.aemfd，对象ID是aemfd-client-sdk。

#### 构建和部署

[按照此处所述构建](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/create-your-first-osgi-bundle.html?lang=en#build-your-project)
[捆绑包确保捆绑包已部署并处于活动状态](http://localhost:4502/system/console/bundles)

创建工作流模型。 在工作流模型中拖放流程步骤。 将流程步骤与“将自适应表单附件保存到文件系统”关联。

提供以逗号分隔的必要进程参数。 例如，附件c:\\scrappp\\。 第一个参数是将相对于工作流有效负荷存储自适应表单附件时的文件夹。 此值必须与配置自适应表单的提交操作时指定的值相同。 第二个参数是您希望存储附件的位置。

创建自适应表单。 将文件附件组件拖放到表单中。 配置表单的提交操作，以调用在前面的步骤中创建的工作流。 提供适当的附件路径。

保存设置。

预览表单。 添加几个附件并提交表单。 附件应在您在工作流中指定的位置保存到文件系统。

