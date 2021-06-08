---
title: 自定义流程步骤，以压缩文件附件
description: 自定义流程步骤，将自适应表单附件添加到zip文件，并将zip文件存储到工作流变量中
sub-product: 表单
feature: 工作流
topics: integrations
audience: developer
doc-type: technical video
activity: setup
version: 6.4,6.5
topic: 开发
role: Developer
level: Beginner
source-git-commit: 22437e93cbf8f36d723dc573fa327562cb51b562
workflow-type: tm+mt
source-wordcount: '158'
ht-degree: 1%

---


# 自定义流程步骤


实施了自定义流程步骤，以创建包含表单附件的zip文件。 如果您不熟悉如何创建OSGi包，请[按照以下说明](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/creating-your-first-osgi-bundle/create-your-first-osgi-bundle.html?lang=en)操作

自定义流程步骤中的代码会执行以下操作

* 在有效负荷文件夹下查询所有自适应表单附件。 文件夹名称将作为进程参数传递到进程步骤。
* 创建zip文件，并将表单附件添加到zip文件。
* 设置2个工作流变量的值（attachments_zip和no_of_attachments）

```java
import java.io.ByteArrayOutputStream;
import java.io.IOException;
import java.io.InputStream;
import java.util.HashMap;
import java.util.Map;
import java.util.zip.ZipEntry;
import java.util.zip.ZipOutputStream;

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
import com.day.cq.search.result.SearchResult;;

@Component(property = {
        Constants.SERVICE_DESCRIPTION + "=Zip form attachments",
        Constants.SERVICE_VENDOR + "=Adobe Systems",
        "process.label" + "=Zip form attachments"
})

public class CreateFormAttachmentCopy implements WorkflowProcess {
        private static final Logger log = LoggerFactory.getLogger(CreateFormAttachmentCopy.class);
        @Reference
        QueryBuilder queryBuilder;

        @Override
        public void execute(WorkItem workItem, WorkflowSession workflowSession, MetaDataMap processArguments) throws WorkflowException
        {
                String payloadPath = workItem.getWorkflowData().getPayload().toString();
                log.debug("The payload path  is" + payloadPath);
                MetaDataMap metaDataMap = workItem.getWorkflow().getWorkflowData().getMetaDataMap();
                Session session = workflowSession.adaptTo(Session.class);
                Map < String, String > map = new HashMap < String, String > ();
                map.put("path", workItem.getWorkflowData().getPayload().toString() + "/" + processArguments.get("PROCESS_ARGS", "string").toString());
                map.put("type", "nt:file");
                Query query = queryBuilder.createQuery(PredicateGroup.create(map), workflowSession.adaptTo(Session.class));
                query.setStart(0);
                query.setHitsPerPage(20);
                SearchResult result = query.getResult();
                log.debug("Get result hits " + result.getHits().size());
                ByteArrayOutputStream baos = new ByteArrayOutputStream();
                ZipOutputStream zipOut = new ZipOutputStream(baos);
                int no_of_attachments = result.getHits().size();
                for (Hit hit: result.getHits())
                {
                        try
                        {
                                String attachmentPath = hit.getPath();
                                log.debug("The hit path is" + hit.getPath());
                                Node attachmentNode = session.getNode(attachmentPath + "/jcr:content");
                                InputStream attachmentStream = attachmentNode.getProperty("jcr:data").getBinary().getStream();
                                ByteArrayOutputStream buffer = new ByteArrayOutputStream();
                                int nRead;
                                byte[] data = new byte[1024];
                                while ((nRead = attachmentStream.read(data, 0, data.length)) != -1)
                                {
                                        buffer.write(data, 0, nRead);
                                }

                                buffer.flush();
                                byte[] byteArray = buffer.toByteArray();
                                ZipEntry zipEntry = new ZipEntry(hit.getTitle());
                                zipOut.putNextEntry(zipEntry);
                                zipOut.write(byteArray);
                                zipOut.closeEntry();

                        } 
                        catch (Exception e)
                        {
                                log.debug("The error message is " + e.getMessage());
                        }
                }
                try
                {
                        zipOut.close();

                }
                catch (IOException e)
                {
                        
                        log.debug(("Error in closing zipout" + e.getMessage()));
                }

                // set the value of the workflow variables.
                metaDataMap.put("attachments_zip", new Document(baos.toByteArray()));
                metaDataMap.put("no_of_attachments", no_of_attachments);

                workflowSession.updateWorkflowData(workItem.getWorkflow(), workItem.getWorkflow().getWorkflowData());
                log.debug("Updated workflow");

        }

}
```

>[!NOTE]
>
> 请确保您的工作流中有一个名为&#x200B;*attachments_zip*&#x200B;的类型为document的变量，以及类型为Double的&#x200B;*no_of_attachments*&#x200B;的变量，以便此代码正常工作


