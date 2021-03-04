---
title: 实施自定义进程步骤
seo-title: 实施自定义进程步骤
description: 使用自定义流程步骤将自适应表单附件写入文件系统
seo-description: 使用自定义流程步骤将自适应表单附件写入文件系统
feature: workflow
topics: development
audience: developer
doc-type: tutorial
activity: understand
version: 6.5
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '896'
ht-degree: 0%

---


# 自定义流程步骤

本教程面向需要实施自定义流程步骤的AEM Forms客户。 进程步骤可以执行ECMA脚本或调用自定义Java代码以执行操作。 本教程将说明实现由进程步骤执行的WorkflowProcess所需的步骤。

实施自定义流程步骤的主要原因是扩展AEM工作流。 例如，如果您在工作流模型中使用AEM Forms组件，则可能需要执行以下操作

* 将自适应表单附件保存到文件系统
* 处理提交的数据

要完成上述用例，您通常会编写一个由进程步骤执行的OSGi服务。

## 创建Maven项目

第一步是使用适当的AdobeMaven Archetype创建一个主项目。 本[文章](https://helpx.adobe.com/experience-manager/using/maven_arch13.html)中列出了详细步骤。 将主项目导入Eclipse后，您就可以开始编写可在流程步骤中使用的第一个OSGi组件。


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

```
@Component(property = { Constants.SERVICE_DESCRIPTION + "=Write Adaptive Form Attachments to File System",
        Constants.SERVICE_VENDOR + "=Adobe Systems",
        "process.label" + "=Save Adaptive Form Attachments to File System" })
public class WriteFormAttachmentsToFileSystem implements WorkflowProcess {
     private static final Logger log = LoggerFactory.getLogger(WriteFormAttachmentsToFileSystem.class);
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
 
        String attachmentsFilePath = payloadPath + "/" + attachmentsPath + "/attachments";
        log.debug("The data file path is " + attachmentsFilePath);
 
        ResourceResolver resourceResolver = workflowSession.adaptTo(ResourceResolver.class);
 
        Resource attachmentsNode = resourceResolver.getResource(attachmentsFilePath);
        Iterator<Resource> attachments = attachmentsNode.listChildren();
        while (attachments.hasNext()) {
            Resource attachment = attachments.next();
            String attachmentPath = attachment.getPath();
            String attachmentName = attachment.getName();
 
            log.debug("The attachmentPath is " + attachmentPath + " and the attachmentname is " + attachmentName);
            com.adobe.aemfd.docmanager.Document attachmentDoc = new com.adobe.aemfd.docmanager.Document(attachmentPath,
                    attachment.getResourceResolver());
            try {
                File file = new File(saveToLocation + File.separator + workItem.getId());
                if (!file.exists()) {
                    file.mkdirs();
                }
 
                attachmentDoc.copyToFile(new File(file + File.separator + attachmentName));
 
                log.debug("Saved attachment" + attachmentName);
                attachmentDoc.close();
 
            } catch (IOException e) {
                // TODO Auto-generated catch block
                e.printStackTrace();
            }
 
        }
 
    }
```

第1行 — 定义组件的属性。 如以下某个屏幕截图所示，将OSGi组件与进程步骤关联时，将看到process.label属性。

第13-15行 — 传递给此OSGi组件的进程参数使用“，”分隔符进行拆分。 随后，将从字符串数组中提取attachmentPath和saveToLocation的值。

* attachmentPath — 这是您在自适应表单中指定的位置，当您配置自适应表单的提交操作以调用AEM工作流时。 这是您希望将附件保存在AEM中的文件夹的名称，该文件夹与工作流的有效负荷相关。

* saveToLocation — 这是您希望将附件保存在AEM服务器文件系统中的位置。

这两个值将作为进程参数传递，如以下屏幕截图所示。

![ProcessStep](assets/implement-process-step.gif)


第19行：然后构建attachmentFilePath。 附件文件路径类似

    /var/fd/仪表板/payload/server0/2018-11-19/3EF6ENASOQTHCPLNDYVNAM7OKA_7/Attachments/attachments

* “附件”是配置自适应表单的提交选项时指定的相对于工作流有效负荷的文件夹名称。

   ![提交选项](assets/af-submit-options.gif)

第24-26行 — 获取ResourceResolver，然后是指向attachmentFilePath的资源。

其余代码通过使用API在指向attachmentFilePath的资源的子对象中迭代来创建文档对象。 此文档对象特定于AEM Forms。 然后，我们使用文档对象的copyToFile方法来保存文档对象。

>[!NOTE]
>
>由于我们使用的是特定于AEM Forms的文档对象，因此您需要在您的主项目中包含aemfd-client-sdk依赖关系。 组ID是com.adobe.aemfd，对象ID是aemfd-client-sdk。

#### 构建和部署

[按照此处所述构建](https://helpx.adobe.com/experience-manager/using/maven_arch13.html#BuildtheOSGibundleusingMaven)
[捆绑包确保捆绑包已部署并处于活动状态](http://localhost:4502/system/console/bundles)

创建工作流模型。 在工作流模型中拖放流程步骤。 将流程步骤与“将自适应表单附件保存到文件系统”关联。

提供以逗号分隔的必要进程参数。 例如，附件c:\\scrappp\\。 第一个参数是将相对于工作流有效负荷存储自适应表单附件时的文件夹。 此值必须与配置自适应表单的提交操作时指定的值相同。 第二个参数是您希望存储附件的位置。

创建自适应表单。 将文件附件组件拖放到表单中。 配置表单的提交操作，以调用在前面的步骤中创建的工作流。 提供适当的附件路径。

保存设置。

预览表单。 添加几个附件并提交表单。 附件应在您在工作流中指定的位置保存到文件系统。

