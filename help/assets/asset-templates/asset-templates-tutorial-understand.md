---
title: '了解AEM Assets的InDesign文件和资产模板 '
description: 此视频教程将逐步介绍如何定义InDesign文件以及所有随附的注意事项，以用于AEM资产的资产模板功能。
feature: catalogs, asset-templates
topics: authoring, renditions, documents
audience: all
doc-type: tutorial
activity: understand
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 22ccd6627a035b37edb180eb4633bc3b57470c0c
workflow-type: tm+mt
source-wordcount: '483'
ht-degree: 1%

---


# 了解AEM Assets{#understanding-indesign-files-and-asset-templates-in-aem-assets}中的InDesign文件和资产模板

此视频教程将逐步介绍如何定义InDesign文件以及所有随附的注意事项，以用于AEM资产的资产模板功能。

## 构建InDesign模板文件{#constructing-the-indesign-template-file}

>[!VIDEO](https://video.tv.adobe.com/v/19293/?quality=9&learn=on)

1. 下载并打开&#x200B;[**InDesign文件模板**](assets/asset-templates-tutorial-video--supporting-files.zip)
2. **打开“标记”** 面板，查看标记命名规范，并注意InDesign文件中可创作的元素已标记。请记住，只有标记元素在AEM中可编辑。

   * **“窗口”>“实用程序”>“标记”**

3. 在页面上，添加新的文本元素，提供文本“Header”并应用&#x200B;**标题**&#x200B;段落样式。

   * **“窗口”>“样式”>“段落样式”**

   然后，创建并应用名为&#x200B;**Page2Heading.**&#x200B;的新标记。

4. 将FPO徽标图像（在zip](assets/asset-templates-tutorial-video--supporting-files.zip)中提供的[）添加到主控页面上的徽标元素。

   * **右键单击**&#x200B;并选&#x200B;**择“管接头”(Fitting)>“框架管接头选项……”(Frame Fitting Options..)>“内容管接头”(Content Fitting)>“按比例填充框架”(Fill Frame Propercy)。**
   [进一步了解“框架适合](https://helpx.adobe.com/indesign/using/frames-objects.html#fitting_objects_to_frames)”选项以及适合您的用例。

5. 通过就地粘贴从页面和页面中的主控模板向下复制标题(徽标和公司名称)。

   * 在第1页上，按住Shift-Cmd键单击macOS或按住Shift-Alt键单击Windows，从主控页面选择暴露的标题，并将其删除。
   * 从主控页面，通过原位粘贴将标题复制到第1页
   * 重复第2页的步骤

6. 通过多次单击每个结构元素，打开“结构”面板，确保所有结构元素与InDesign文件中的真实元素相对应。 删除任何未使用或不需要的元素。 确保所有标记都是语义的，并且元素标记正确。

   >[!NOTE]
   >
   >请记住，构建不良的InDesign文件是AEM资产模板问题的最常见原因，因此，请确保标记和结构清晰、正确。

## 在AEM Assets{#creating-and-authoring-an-asset-template-in-aem-assets}中创建和创作资产模板

>[!VIDEO](https://video.tv.adobe.com/v/19294/?quality=9&learn=on)

1. **开始** InDesign服务器端口8080。
2. 确保将&#x200B;**AEM作者实例配置为与InDesign Server**&#x200B;交互（反之亦然）。

   * [IDS工作Cloud Service配置](http://localhost:4502/etc/cloudservices/proxy/ids.html)
   * [云代理Cloud Service配置](http://localhost:4502/etc/cloudservices/proxy.html)
   * [AEM Externalizer OSGi配置](http://localhost:4502/system/console/configMgr)

3. **已将InDesign文件上传到AEM** 资产，并允许AEM工作流和InDesign Server完全处理资产。
4. **在“资产”** >“模 **板”下创** 建新的模板，并在步骤#4中选择上传到AEM的InDesign文件。
5. **编辑在步骤** #5中创建的资产模板，并创作可编辑字段。
6. 单击&#x200B;**完成**&#x200B;以生成资产模板的最终高保真演绎版。
7. 单击资产模板卡以打开，然后查看资产演绎版以下载高保真演绎版。

## 其他资源 {#additional-resources}

InDesign模板文件和支持图像

下载[InDesign模板文件和支持图像](assets/asset-templates-tutorial-video--supporting-files-1.zip)

* [InDesignCC试用版下载](https://creative.adobe.com/products/download/indesign)
* [InDesign Server试用版下载](https://www.adobe.com/devnet/indesign/indesign-server-trial-downloads.html)
