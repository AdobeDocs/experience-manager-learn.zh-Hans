---
title: 建立自訂流程步驟
description: 使用Document Cloud將Word、Excel附件轉換為PDF的自訂程式步驟。
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
thumbnail: 7837.jpg
kt: 7837
exl-id: 24a788bb-f0dc-4774-91ab-26fde2de098f
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '77'
ht-degree: 0%

---

# 自訂流程步驟

以下是自訂流程步驟的完整程式碼，該程式碼會以轉換後的pdf轉換和取代原生檔案。此自訂步驟會搜尋資料夾名稱下的所有附件，該資料夾名稱會作為工作流程中的流程引數提供。
此自訂流程步驟使用自訂DocumentCloudSDKService的方法來建立PDF。


```java
package com.aemforms.doccloud.core;

import java.io.InputStream;
import java.util.HashMap;
import java.util.Map;

import javax.jcr.Binary;
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
	Constants.SERVICE_DESCRIPTION + "=Convert form attachments to PDF Using Document Cloud API",
	Constants.SERVICE_VENDOR + "=Adobe Systems",
	"process.label" + "=Convert form attachments to PDF Using Document Cloud API"
})

public class ConvertAttachmentsToPDF implements WorkflowProcess {
	private static final Logger log = LoggerFactory.getLogger(ConvertAttachmentsToPDF.class);
	@Reference
	QueryBuilder queryBuilder;
	@Reference
	DocumentCloudSDKService documentCloudService;
	@Override
	public void execute(WorkItem workItem, WorkflowSession workflowSession, MetaDataMap processArguments) throws WorkflowException {

		log.debug("The payload path is " + workItem.getWorkflowData().getPayload().toString());
		log.debug("The form attachments are stored under  ..." + processArguments.get("PROCESS_ARGS", "string").toString() + " folder in the payload");

		Session session = workflowSession.adaptTo(Session.class);
		Map<String, String> map = new HashMap<String, String> ();
		map.put("path", workItem.getWorkflowData().getPayload().toString() + "/" + processArguments.get("PROCESS_ARGS", "string").toString());
		map.put("type", "nt:file");
		Query query = queryBuilder.createQuery(PredicateGroup.create(map), workflowSession.adaptTo(Session.class));
		query.setStart(0);
		query.setHitsPerPage(20);

		SearchResult result = query.getResult();
		log.debug("Get result hits " + result.getHits().size());
		Node attachmentNode = null;
		for (Hit hit: result.getHits()) {
			try {
				String path = hit.getPath();
				log.debug("The title of the attachment is  " + hit.getTitle() + " and path " + path);
				String title = hit.getTitle();
				title = title.substring(0, title.indexOf("."));
				attachmentNode = workflowSession.adaptTo(Session.class).getNode(path + "/jcr:content");
				InputStream xmlDataStream = attachmentNode.getProperty("jcr:data").getBinary().getStream();
				Document convertedPDF = documentCloudService.createPDFFromInputStream(xmlDataStream, hit.getTitle());
				Node nonPDFNode = session.getNode(path);
				session.move(nonPDFNode.getPath(), nonPDFNode.getParent().getPath() + "/" + title + ".pdf");
				Binary binary = session.getValueFactory().createBinary(convertedPDF.getInputStream());
				attachmentNode.setProperty("jcr:data", binary);
				log.debug("Replacing the original file with converted pdf ");
				session.save();

			} catch (Exception e) {
				System.out.println(e.getMessage());
			}
		}

	}

}
```
