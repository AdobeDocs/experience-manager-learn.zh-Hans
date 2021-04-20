---
title: 自定义分配任务通知
description: 在分配任务通知电子邮件中包含表单数据
sub-product: 表单
feature: Workflow
topics: integrations
audience: developer
doc-type: article
activity: setup
version: 6.4,6.5
kt: 6279
thumbnail: KT-6279.jpg
topic: Development
role: Developer
level: Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '450'
ht-degree: 2%

---


# 自定义分配任务通知

“分配任务”组件用于向工作流参加者分配任务。 将任务分配给用户或用户组时，将向定义的用户或用户组成员发送电子邮件通知。
此电子邮件通知通常包含与任务相关的动态数据。 使用系统生成的[元数据属性](https://docs.adobe.com/content/help/en/experience-manager-65/forms/publish-process-aem-forms/use-metadata-in-email-notifications.html#using-system-generated-metadata-in-an-email-notification)获取此动态数据。
要在电子邮件通知中包含已提交表单数据的值，我们需要创建自定义元数据属性，然后在电子邮件模板中使用这些自定义元数据属性



## 创建自定义元数据属性

建议的方法是创建一个OSGI组件，用于实现[WorkitemUserMetadataService](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/com/adobe/fd/workspace/service/external/WorkitemUserMetadataService.html#getUserMetadataMap--)的getUserMetadata方法

以下代码创建4个元数据属性（_firstName_、_lastName_、_reason_&#x200B;和&#x200B;_amountRequested_），并从提交的数据中设置其值。 例如，从提交的数据中将元数据属性&#x200B;_firstName_&#x200B;的值设置为名为firstName的元素的值。 以下代码假定自适应表单的提交数据采用xml格式。 基于JSON模式或表单数据模型的自适应Forms以JSON格式生成数据。


```java
package com.aemforms.workitemuserservice.core;

import java.io.InputStream;
import java.util.HashMap;
import java.util.Map;

import javax.jcr.Session;
import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.xpath.XPath;

import org.osgi.framework.Constants;
import org.osgi.service.component.annotations.Component;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.w3c.dom.*;


import com.adobe.fd.workspace.service.external.WorkitemUserMetadataService;
import com.adobe.granite.workflow.WorkflowSession;
import com.adobe.granite.workflow.exec.WorkItem;
import com.adobe.granite.workflow.metadata.MetaDataMap;
@Component(property={Constants.SERVICE_DESCRIPTION+"=A sample implementation of a user metadata service.",
Constants.SERVICE_VENDOR+"=Adobe Systems",
"process.label"+"=Sample Custom Metadata Service"})


public class WorkItemUserServiceImpl implements WorkitemUserMetadataService {
private static final Logger log = LoggerFactory.getLogger(WorkItemUserServiceImpl.class);

@Override
public Map<String, String> getUserMetadata(WorkItem workItem, WorkflowSession workflowSession,MetaDataMap metadataMap)
{
HashMap<String, String> customMetadataMap = new HashMap<String, String>();
String payloadPath = workItem.getWorkflowData().getPayload().toString();
String dataFilePath = payloadPath + "/Data.xml/jcr:content";
Session session = workflowSession.adaptTo(Session.class);
DocumentBuilderFactory factory = null;
DocumentBuilder builder = null;
Document xmlDocument = null;
javax.jcr.Node xmlDataNode = null;
try
{
    xmlDataNode = session.getNode(dataFilePath);
    InputStream xmlDataStream = xmlDataNode.getProperty("jcr:data").getBinary().getStream();
    XPath xPath = javax.xml.xpath.XPathFactory.newInstance().newXPath();
    factory = DocumentBuilderFactory.newInstance();
    builder = factory.newDocumentBuilder();
    xmlDocument = builder.parse(xmlDataStream);
    Node firstNameNode = (org.w3c.dom.Node) xPath.compile("afData/afUnboundData/data/firstName")
            .evaluate(xmlDocument, javax.xml.xpath.XPathConstants.NODE);
    log.debug("The value of first name element  is " + firstNameNode.getTextContent());
    Node lastNameNode = (org.w3c.dom.Node) xPath.compile("afData/afUnboundData/data/lastName")
            .evaluate(xmlDocument, javax.xml.xpath.XPathConstants.NODE);
    Node amountRequested = (org.w3c.dom.Node) xPath
            .compile("afData/afUnboundData/data/amountRequested")
            .evaluate(xmlDocument, javax.xml.xpath.XPathConstants.NODE);
    Node reason = (org.w3c.dom.Node) xPath.compile("afData/afUnboundData/data/reason")
            .evaluate(xmlDocument, javax.xml.xpath.XPathConstants.NODE);
    customMetadataMap.put("firstName", firstNameNode.getTextContent());
    customMetadataMap.put("lastName", lastNameNode.getTextContent());
    customMetadataMap.put("amountRequested", amountRequested.getTextContent());
    customMetadataMap.put("reason", reason.getTextContent());
    log.debug("Created  " + customMetadataMap.size() + " metadata  properties");

}
catch (Exception e)
{
    log.debug(e.getMessage());
}
return customMetadataMap;
}

}
```

## 在任务通知电子邮件模板中使用自定义元数据属性

在电子邮件模板中，您可以使用以下语法包含metadata属性，其中amountRequested是元数据属性`${amountRequested}`

## 配置分配任务以使用自定义元数据属性

在将OSGi组件构建并部署到AEM服务器后，配置如下所示的分配任务组件以使用自定义元数据属性。


![任务通知](assets/task-notification.PNG)

## 启用自定义元数据属性的使用

![自定义元数据属性](assets/custom-meta-data-properties.PNG)

## 在服务器上尝试

* [配置Day CQ邮件服务](https://docs.adobe.com/content/help/en/experience-manager-65/administering/operations/notification.html#configuring-the-mail-service)
* 将有效的电子邮件ID与[admin user](http://localhost:4502/security/users.html)关联
* 使用[包管理器](http://localhost:4502/crx/packmgr/index.jsp)下载并安装[Workflow-and-notification-template](assets/workflow-and-task-notification-template.zip)
* 下载[自适应表单](assets/request-travel-authorization.zip)并从[表单和文档ui](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)导入AEM。
* 使用[Web控制台](http://localhost:4502/system/console/bundles)部署和开始[自定义捆绑包](assets/work-items-user-service-bundle.jar)
* [预览并提交表单](http://localhost:4502/content/dam/formsanddocuments/requestfortravelauhtorization/jcr:content?wcmmode=disabled)

在表单提交时，任务分配通知将发送到与管理员用户关联的电子邮件ID。 以下屏幕截图显示了示例任务分配通知

![通知](assets/task-nitification-email.png)

>[!NOTE]
>分配任务通知的电子邮件模板需要采用以下格式。
>
> subject=任务- `${workitem_title}`
>
> message=表示您的电子邮件模板的字符串，不带任何新行字符。
