---
title: '了解InDesign文件和AEM Assets中的资源模板 '
description: 此视频教程将逐步介绍如何定义一个InDesign文件以及所有随附的注意事项，以用于AEM资产的资产模板功能。
version: 6.3, 6.4, 6.5
topic: 内容管理
role: Business Practitioner
level: Intermediate
source-git-commit: b4fa992abe22e3a546d651e465d6ffc9e415aee2
workflow-type: tm+mt
source-wordcount: '496'
ht-degree: 1%

---


# 了解AEM Assets {#understanding-indesign-files-and-asset-templates-in-aem-assets}中的InDesign文件和资产模板

此视频教程将逐步介绍如何定义一个InDesign文件以及所有随附的注意事项，以用于AEM资产的资产模板功能。

## 构建InDesign模板文件{#constructing-the-indesign-template-file}

>[!VIDEO](https://video.tv.adobe.com/v/19293/?quality=9&learn=on)

1. 下载并打开&#x200B;[**InDesign文件模板**](assets/asset-templates-tutorial-video--supporting-files.zip)
2. **打开“标签”面** 板，查看标签命名规范，并注意，InDesign文件中的可创作元素已添加标签。请记住，只有标记元素可在AEM中编辑。

   * **“窗口”>“实用程序”>“标记”**

3. 在页面上，添加新的文本元素，提供文本“Header”并应用&#x200B;**标题**&#x200B;段落样式。

   * **“窗口”>“样式”>“段落样式”**

   然后，创建并应用名为&#x200B;**Page2Heading.**&#x200B;的新标记

4. 将FPO徽标图像（[在zip](assets/asset-templates-tutorial-video--supporting-files.zip)中提供）添加到主控页面上的“徽标”元素。

   * **右键单击并**&#x200B;选&#x200B;**择“管接头”(Fitting)>“框架管接头选项……”(Frame Fitting Options...)>“内容管接头”(Content Fitting)>“按比例填充框架”(Fill Frame Promecial)**
   [进一步了解“框架适合](https://helpx.adobe.com/indesign/using/frames-objects.html#fitting_objects_to_frames)”选项以及适合您的用例。

5. 通过就地粘贴，从页面和页面中的主控模板向下复制标题(徽标和公司名称)。

   * 在第1页上，按住Shift-Cmd键并单击macOS或按住Shift-Alt键并单击Windows，以选择从主控页面中显示的标题，并将其删除。
   * 从主控页面，通过原位粘贴将标题复制到第1页
   * 为第2页重复上述步骤

6. 通过按住多次单击每个元素，打开“结构”面板，确保所有结构元素都与InDesign文件中的实际元素相对应。 删除任何未使用或不需要的元素。 确保所有标记都是语义的，并正确标记元素。

   >[!NOTE]
   >
   >请记住，构造不良的InDesign文件是AEM资产模板问题的最常见原因，因此请确保标记和结构清晰、正确。

## 在AEM Assets {#creating-and-authoring-an-asset-template-in-aem-assets}中创建和创作资产模板

>[!VIDEO](https://video.tv.adobe.com/v/19294/?quality=9&learn=on)

1. **开始 InDesign** Serveron端口8080。
2. 确保将&#x200B;**AEM作者实例配置为与您的InDesign Server**&#x200B;交互（反之亦然）。

   * [IDS工作Cloud Service配置](http://localhost:4502/etc/cloudservices/proxy/ids.html)
   * [云代理Cloud Service配置](http://localhost:4502/etc/cloudservices/proxy.html)
   * [AEM Externalizer OSGi配置](http://localhost:4502/system/console/configMgr)

3. **已将InDesign文件上传** 到AEM Asset，并允许AEM Workflow和InDesign Server完全处理资产。
4. **在“资产”** >“模 **板”下创** 建新模板，然后在步骤#4中选择上传到AEM的InDesign文件。
5. **编辑在第#5步** 中创建的资产模板，并创作可编辑的字段。
6. 单击&#x200B;**完成**&#x200B;以生成资产模板的最终高保真演绎版。
7. 单击资产模板卡以打开，然后查看资产演绎版以下载高保真演绎版。

## 其他资源 {#additional-resources}

InDesign模板文件和支持图像

下载[InDesign模板文件并支持Images](assets/asset-templates-tutorial-video--supporting-files-1.zip)

* [InDesign CC试用版下载](https://creative.adobe.com/products/download/indesign)
* [CC Enterprise客户可联系其客户经理申请InDesign Server试用许可](https://www.adobe.com/products/indesignserver/faq.html)
