---
title: 在AEM Forms使用监视文件夹
seo-title: 在AEM Forms使用监视文件夹
description: 在AEM Forms配置和使用监视文件夹
seo-description: 在AEM Forms配置和使用监视文件夹
uuid: 32c4bda2-363d-4294-925e-405a176f7f8d
feature: output-service
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
discoiquuid: a40e2381-0dc8-4784-9b80-15e27b244035
translation-type: tm+mt
source-git-commit: 3d54a8158d0564a3289a2100bbbc59e5ae38f175
workflow-type: tm+mt
source-wordcount: '439'
ht-degree: 0%

---


# 在AEM Forms使用监视文件夹{#using-watched-folders-in-aem-forms}

管理员可以配置网络文件夹（称为“监视文件夹”），当用户将文件（如PDF文件）放在“监视文件夹”中时，会启动预先配置的工作流、服务或脚本操作以处理添加的文件。 服务执行指定操作后，会将结果文件保存到指定的输出文件夹中。 有关工作流、服务和脚本的详细信息。

要了解有关创建监视文件夹的更多信息，请单击此处[](https://helpx.adobe.com/experience-manager/6-4/forms/using/Creating-Configure-watched-folder.html)

“监视文件夹”用于在批处理模式下生成文档。 使用监视的文件夹机制，您可以为打印渠道生成交互通信，或使用输出服务将数据与模板合并。

本文将介绍利用输出服务通过监视的文件夹机制将数据与模板合并的用例。

输出服务是AEM文档服务的一部分的OSGi服务。 输出服务支持AEM Forms设计器的各种输出格式和输出设计功能。 输出服务可以转换XFA模板和XML数据以生成各种格式的打印文档。

要进一步了解输出服务，请单击此处[。
](https://helpx.adobe.com/aem-forms/6/output-service.html)要在系统上设置监视文件夹，请按照以下步骤操作：
* [下载并解压zip文件的内容](assets/outputservicewatchedfolderkt.zip)。此zip文件包含用于创建监视文件夹的包以及使用监视文件夹机制测试输出服务的示例文件
   * Windows系统

      * 使用包管理器将outputservicewatchedfolder.zip导入AEM
      * 这将在您的C驱动器上创建一个名为outputservicewatchedfolder的监视文件夹。
   * 非Windows系统
      * [打开已监视文件夹的配置设置](http://localhost:4502/crx/de/index.jsp#/etc/fd/watchfolder/config/outputservice)
      * 设置外部服务节点的文件夹路径属性以指向合适的位置
      * 保存更改
      * 上述位置将是您的监视文件夹。

将SamplePdfFile和SampleXdpFile文件夹拖放到监视文件夹的输入文件夹中。 成功处理文件后，结果将放在监视文件夹的结果文件夹下。


>[!NOTE]
>
>如果与监视文件夹关联的脚本需要多个文件，您需要创建一个文件夹并将所有所需文件放入该文件夹中，然后将该文件夹放入监视文件夹的输入文件夹中。
>
>如果与监视文件夹关联的脚本只需要一个输入文件，则可以将该文件直接放入监视文件夹的输入文件夹中

