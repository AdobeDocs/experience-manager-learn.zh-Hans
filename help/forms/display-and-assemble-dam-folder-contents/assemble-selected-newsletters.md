---
title: 将所选新闻稿合并为一个文件
description: 使用汇编程序服务合并所选新闻稿
feature: Adaptive Forms
version: 6.5
topic: Development
role: User
level: Beginner
last-substantial-update: 2023-01-01T00:00:00Z
exl-id: 3a64315f-f699-4538-b999-626e7a998c05
duration: 79
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '161'
ht-degree: 1%

---

# 将所选新闻稿合并为一个PDF

用户的选择存储在隐藏字段中。 此隐藏字段的值将传递到servlet，它将使用将所选内容组合为一个pdf [Forms Assembler服务](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javadocs/com/adobe/fd/assembler/service/AssemblerService.html).


## 用于组合pdf文件的Servlet

以下代码将组合选定的新闻稿。 该代码根据用户的选择创建文档映射。 通过此映射创建DDX，并将此DDX与文档映射一起传递给Assembler服务的调用方法，以得到组合文档。 组合后的pdf存储在存储库中，其路径返回到调用应用程序。

```java
protected void doPost(SlingHttpServletRequest request,SlingHttpServletResponse response)
   {
   
       String []newsletters = request.getParameter("selectedNewsLetters").split(",");
       Map<String, Object> mapOfDocuments = new HashMap<String, Object>();
       for(int i= 0;i<newsletters.length;i++)
       {
           Resource resource = request.getResourceResolver().getResource(newsletters[i]);
           
           log.debug("The resource name is "+resource.getName());
           Document newsletter = new Document(resource.getPath());
           mapOfDocuments.put(resource.getName(), newsletter);
       }
       log.debug("The newsletters selected: "+newsletters);
       Document ddxDocument = createDDXFromMapOfDocuments(mapOfDocuments);
       AssemblerOptionSpec aoSpec = new AssemblerOptionSpec();
       aoSpec.setFailOnError(true);
       AssemblerResult ar = null;
       try {
               ar = assemblerService.invoke(ddxDocument, mapOfDocuments, aoSpec);
               Document assembledPDF = ar.getDocuments().get("GeneratedPDF.pdf");
               // This is my custom code to get fd-service user
               ResourceResolver formsServiceResolver = getResolver.getFormsServiceResolver();
               
               Resource nodeResource = formsServiceResolver.getResource("/content/newsletters");
           
               UUID uuid = UUID.randomUUID();
               String uuidString = uuid.toString();
               javax.jcr.Node assembledNewsletters = nodeResource.adaptTo(Node.class);
               javax.jcr.Node assembledNewsletter =  assembledNewsletters.addNode(uuidString + ".pdf", "nt:file");
               javax.jcr.Node resNode = assembledNewsletter.addNode("jcr:content", "nt:resource");
               ValueFactory valueFactory = formsServiceResolver.adaptTo(Session.class).getValueFactory();
               Binary contentValue = valueFactory.createBinary(assembledPDF.getInputStream());
               resNode.setProperty("jcr:data", contentValue);
               formsServiceResolver.commit();
               PrintWriter out = response.getWriter();
               response.setContentType("application/json");
               response.setCharacterEncoding("UTF-8");
               JsonObject asset = new JsonObject();
          
               asset.addProperty("assetPath", assembledNewsletter.getPath());
               out.print(new Gson().toJson(asset));
               out.flush();  
               
           } 
           catch (IOException | OperationException | RepositoryException e)
           {
           
               log.error("Error is "+e.getMessage());
           }
   }
```

## 实用程序函数

在组装新闻稿时使用了以下实用程序函数。 这些实用程序函数从文档映射创建DDX，并将org.w3c.dom.Document转换为AEMFD文档对象。


```java
public Document createDDXFromMapOfDocuments(Map<String, Object> mapOfDocuments)
     {
         
        final DocumentBuilderFactory docFactory = DocumentBuilderFactory.newInstance();
        org.w3c.dom.Document ddx = null;
        try
               {
                   DocumentBuilder docBuilder = docFactory.newDocumentBuilder();
                   ddx = docBuilder.newDocument();
                   Element rootElement = ddx.createElementNS("http://ns.adobe.com/DDX/1.0/", "DDX");
                ddx.appendChild(rootElement);
                Element pdfResult = ddx.createElement("PDF");
                pdfResult.setAttribute("result","GeneratedPDF.pdf");
                rootElement.appendChild(pdfResult);
                for (String key : mapOfDocuments.keySet())
                    {
                        log.debug(key + " " + mapOfDocuments.get(key));
                        Element pdfSourceElement = ddx.createElement("PDF");
                        pdfSourceElement.setAttribute("source", key);
                        pdfSourceElement.setAttribute("bookmarkTitle",key);
                        pdfResult.appendChild(pdfSourceElement);
                    }
                return orgw3cDocumentToAEMFDDocument(ddx);
            }
            catch (ParserConfigurationException e)
                {
                    log.debug("Error:"+e.getMessage());
                }
            return null;
     }
```

```java
public Document orgw3cDocumentToAEMFDDocument( org.w3c.dom.Document xmlDocument)
     {
         final ByteArrayOutputStream outputStream = new ByteArrayOutputStream();
         DOMSource source = new DOMSource(xmlDocument);
         log.debug("$$$$In orgW3CDocumentToAEMFDDocument method");
         StreamResult outputTarget = new StreamResult(outputStream);
         try
             {
               TransformerFactory.newInstance().newTransformer().transform(source, outputTarget);
               InputStream is1 = new ByteArrayInputStream(outputStream.toByteArray());
               Document xmlAEMFDDocument = new Document(is1);
               if (log.isDebugEnabled())
                   {
                    xmlAEMFDDocument.copyToFile(new File("dataxmldocument.xml"));
                }
             return xmlAEMFDDocument;
            }
            catch (Exception e)
                 {
                    log.error("Error in generating ddx " + e.getMessage());
                    return null;
                 }
        }
```

## 后续步骤

[在系统上部署示例资源](./deploy-on-your-system.md)