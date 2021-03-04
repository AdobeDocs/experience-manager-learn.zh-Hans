---
title: 组合表单附件
description: 按指定顺序组合表单附件
feature: 汇编程序
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
kt: 6406
thumbnail: kt-6406.jpg
topic: 开发
role: 开发人员
level: 富有经验
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '637'
ht-degree: 0%

---


# 组合表单附件

本文提供了按指定顺序组合自适应表单附件的资源。 表单附件需要采用pdf格式才能使用此示例代码。 以下是用例。
填写自适应表单的用户会将一个或多个pdf文档附加到表单。
提交表单时，合并表单附件以生成一个pdf。 您可以指定附件的组装顺序以生成最终的pdf。

## 创建用于实现WorkflowProcess接口的OSGi组件

创建一个OSGi组件，用于实现[com.adobe.granite.workflow.exec.WorkflowProcess接口](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/workflow/exec/WorkflowProcess.html)。 此组件中的代码可以与AEM工作流中的流程步骤组件关联。 此组件中实现了com.adobe.granite.workflow.exec.WorkflowProcess接口的执行方法。

提交自适应表单以触发AEM工作流时，提交的数据将存储在有效负荷文件夹下的指定文件中。 例如，这是已提交的数据文件。 我们需要组合在idcard和bankstatement标签下指定的附件。
![submitted-data](assets/submitted-data.JPG)。

### 获取标记名称

附件的顺序在工作流中指定为进程步骤参数，如下面的屏幕截图所示。 在此，我们将添加到现场身份证的附件汇编成银行报表

![process-step](assets/process-step.JPG)

以下代码片断从进程参数中提取附件名称

```java
String  []attachmentNames  = arg2.get("PROCESS_ARGS","string").toString().split(",");
```

### 从附件名称创建DDX

然后，我们需要创建[文档描述XML(DDX)](https://helpx.adobe.com/pdf/aem-forms/6-2/ddxRef.pdf)文档,Assembler服务使用它来组合文档。 以下是从进程参数创建的DDX。 NoForms元素允许您拼合基于XFA的文档，然后再进行组合。 请注意，PDF源元素按流程参数中指定的正确顺序排列。

![ddx-xml](assets/ddx.PNG)

### 创建文档映射

然后，我们创建一个文档映射，其中附件名称为键，附件为值。 查询 Builder服务用于查询有效负荷路径下的附件并构建文档映射。 汇编程序服务需要此文档图和DDX组合最终的pdf。

```java
public Map<String, Object> createMapOfDocuments(String payloadPath,WorkflowSession workflowSession )
{
  Map<String, String> queryMap = new HashMap<String, String>();
  Map<String,Object>mapOfDocuments = new HashMap<String,Object>();
  queryMap.put("type", "nt:file");
  queryMap.put("path",payloadPath);
  Query query = queryBuilder.createQuery(PredicateGroup.create(queryMap),workflowSession.adaptTo(Session.class));
  query.setStart(0);
  query.setHitsPerPage(30);
  SearchResult result = query.getResult();
  log.debug("Get result hits "+result.getHits().size());
  for (Hit hit : result.getHits()) {
    try {
          String path = hit.getPath();
          log.debug("The title "+hit.getTitle()+" path "+path);
          if(hit.getTitle().endsWith("pdf"))
           {
             com.adobe.aemfd.docmanager.Document attachmentDocument = new com.adobe.aemfd.docmanager.Document(path);
             mapOfDocuments.put(hit.getTitle(),attachmentDocument);
             log.debug("@@@@Added to map@@@@@ "+hit.getTitle());
           }
        }
    catch (Exception e)
       {
          log.debug(e.getMessage());
       }

}
return mapOfDocuments;
}
```

### 使用AssemblerService组合文档

创建DDX和文档映射后，下一步是使用AssemblerService组合文档。
以下代码将汇编并返回汇编的pdf。

```java
private com.adobe.aemfd.docmanager.Document assembleDocuments(Map<String, Object> mapOfDocuments, com.adobe.aemfd.docmanager.Document ddxDocument)
{
    AssemblerOptionSpec aoSpec = new AssemblerOptionSpec();
    aoSpec.setFailOnError(true);
    AssemblerResult ar = null;
    try
    {
        ar = assemblerService.invoke(ddxDocument, mapOfDocuments, aoSpec);
        return (com.adobe.aemfd.docmanager.Document) ar.getDocuments().get("GeneratedDocument.pdf");
    }
    catch (OperationException e)
    {
        log.debug(e.getMessage());
    }
    return null;
    
}
```

### 在有效负荷文件夹下保存已装配的pdf

最后一步是在有效负荷文件夹下保存已装配的pdf。 然后，可以在工作流的后续步骤中访问此pdf以进一步处理。
以下代码段用于将文件保存在有效负荷文件夹下

```java
Session session = workflowSession.adaptTo(Session.class);
javax.jcr.Node payloadNode =  workflowSession.adaptTo(Session.class).getNode(workItem.getWorkflowData().getPayload().toString());
log.debug("The payload Path is "+payloadNode.getPath());
javax.jcr.Node assembledPDFNode = payloadNode.addNode("assembled-pdf.pdf", "nt:file"); 
javax.jcr.Node jcrContentNode =  assembledPDFNode.addNode("jcr:content", "nt:resource");
Binary binary =  session.getValueFactory().createBinary(assembledDocument.getInputStream());
jcrContentNode.setProperty("jcr:data", binary);
log.debug("Saved !!!!!!"); 
session.save();
```

以下是组装和存储表单附件后的有效负荷文件夹结构。

![有效载荷结构](assets/payload-structure.JPG)

### 要使此功能在您的AEM服务器上工作

* 将[组合表单附件表单](assets/assemble-form-attachments-af.zip)下载到您的本地系统。
* 从[Forms和文档](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)页面导入表单。
* 下载[workflow](assets/assemble-form-attachments.zip)并使用包管理器导入AEM。
* 下载[自定义捆绑包](assets/assembletaskattachments.assembletaskattachments.core-1.0-SNAPSHOT.jar)
* 使用[Web控制台](http://localhost:4502/system/console/bundles)部署和开始捆绑包
* 将您的浏览器指向[组合附件表单](http://localhost:4502/content/dam/formsanddocuments/assembleattachments/jcr:content?wcmmode=disabled)
* 在ID文档中添加附件，并在银行对帐单部分添加几个pdf文档
* 提交表单以触发工作流
* 检查crx](http://localhost:4502/crx/de/index.jsp#/var/fd/dashboard/payload)中工作流的[有效负荷文件夹，以查找已组合的pdf

>[!NOTE]
> 如果您为自定义捆绑启用了记录器，则DDX和已组合的文件将写入AEM安装的文件夹。

