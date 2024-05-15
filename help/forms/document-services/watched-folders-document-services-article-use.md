---
title: 使用AEM Forms中的Watched文件夹
description: 在AEM Forms中配置和使用观察文件夹
feature: Output Service
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: abb74d44-d1b9-44d6-a49f-36c01acfecb4
last-substantial-update: 2020-07-07T00:00:00Z
duration: 86
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '406'
ht-degree: 0%

---

# 使用AEM Forms中的Watched文件夹{#using-watched-folders-in-aem-forms}

管理员可以配置网络文件夹（称为Watched文件夹），以便当用户将文件(如PDF文件)放入Watched文件夹时，可以启动预先配置的工作流、服务或脚本操作来处理添加的文件。 服务执行指定的操作后，将结果文件保存在指定的输出文件夹中。 有关工作流、服务和脚本的详细信息。

要了解有关创建watched文件夹的详细信息， [单击此处](https://helpx.adobe.com/experience-manager/6-4/forms/using/Creating-Configure-watched-folder.html)

Watched文件夹用于在批处理模式下生成文档。 使用观察文件夹机制，可以为打印渠道生成交互式通信，或者使用输出服务将数据与模板合并。

本文将介绍通过watched文件夹机制使用输出服务将数据与模板合并的用例。

Output服务是AEM Document Services中的OSGi服务。 输出服务支持AEM Forms Designer的各种输出格式和输出设计功能。 输出服务可以转换XFA模板和XML数据以生成各种格式的打印文档。

要了解有关输出服务的更多信息， [请单击此处](https://helpx.adobe.com/aem-forms/6/output-service.html).
要在系统上设置watched文件夹，请执行以下步骤：
* [下载并解压缩zip文件的内容](assets/outputservicewatchedfolderkt.zip).此zip文件包含用于创建watched文件夹的软件包和使用watched文件夹机制测试输出服务的示例文件
   * Windows系统

      * 使用包管理器将outputservicewatchedfolder.zip导入AEM
      * 这会在C驱动器上创建一个名为outputservicewatchedfolder的受监视文件夹。
   * 非Windows系统
      * [打开watched文件夹的配置设置](http://localhost:4502/crx/de/index.jsp#/etc/fd/watchfolder/config/outputservice)
      * 将外服务节点的文件夹路径属性设置为指向适当的位置
      * 保存更改
      * 上面提到的位置是您的观察文件夹。

将SamplePdfFile和SampleXdpFile文件夹放入watched文件夹的输入文件夹中。 成功处理文件后，结果将放置在观察文件夹的结果文件夹下。


>[!NOTE]
>
>如果与watched文件夹关联的脚本需要多个文件，则需要创建一个文件夹，并将所有必需的文件放置在该文件夹中，并将该文件夹放入watched文件夹的输入文件夹中。
>
>如果与watched文件夹关联的脚本只需要一个输入文件，则可以将该文件直接拖放到观察文件夹的输入文件夹中
