---
title: 在AEM Forms中使用监视文件夹
description: 在AEM Forms中配置和使用监视文件夹
feature: Output Service
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: abb74d44-d1b9-44d6-a49f-36c01acfecb4
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '423'
ht-degree: 0%

---

# 在AEM Forms中使用监视文件夹{#using-watched-folders-in-aem-forms}

管理员可以配置网络文件夹（称为“监视文件夹”），以便当用户在“监视文件夹”中放置文件(如PDF文件)时，会启动预配置的工作流、服务或脚本操作来处理添加的文件。 服务执行指定操作后，会将结果文件保存在指定的输出文件夹中。 有关工作流、服务和脚本的更多信息。

要了解有关创建监视文件夹的更多信息， [单击此处](https://helpx.adobe.com/experience-manager/6-4/forms/using/Creating-Configure-watched-folder.html)

“监视的文件夹”用于在批处理模式下生成文档。 使用监视文件夹机制，您可以为打印渠道生成交互式通信，或使用输出服务将数据与模板合并。

本文将介绍利用输出服务通过监视文件夹机制将数据与模板合并的用例。

输出服务是AEM Document Services中的一项OSGi服务。 输出服务支持AEM Forms Designer的各种输出格式和输出设计功能。 输出服务可以转换XFA模板和XML数据，以生成各种格式的打印文档。

要进一步了解输出服务， [请单击此处](https://helpx.adobe.com/aem-forms/6/output-service.html).
要在系统上设置监视文件夹，请执行以下步骤：
* [下载并解压缩zip文件的内容](assets/outputservicewatchedfolderkt.zip).此zip文件包含用于创建监视文件夹的包和使用监视文件夹机制测试输出服务的示例文件
   * Windows系统

      * 使用包管理器将outputservicewatchedfolder.zip导入AEM中
      * 这会在您的C驱动器上创建一个名为outputservicewatchedfolder的监视文件夹。
   * 非Windows系统
      * [打开监视文件夹的配置设置](http://localhost:4502/crx/de/index.jsp#/etc/fd/watchfolder/config/outputservice)
      * 设置外部服务节点的文件夹路径属性以指向合适的位置
      * 保存更改
      * 上述位置是您的监视文件夹。

将SamplePdfFile和SampleXdpFile文件夹拖放到已监视文件夹的输入文件夹中。 成功处理文件后，结果会放置在已监视文件夹的结果文件夹下。


>[!NOTE]
>
>如果与监视文件夹关联的脚本需要多个文件，则需要创建一个文件夹，并将所有必需的文件放置在该文件夹中，然后将该文件夹放入监视文件夹的输入文件夹中。
>
>如果与监视文件夹关联的脚本只需要一个输入文件，则可以将该文件直接拖放到监视文件夹的输入文件夹中
