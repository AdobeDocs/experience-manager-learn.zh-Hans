---
title: 了解AEM Assets中的InDesign文件和资源模板
description: 本视频教程将介绍如何定义AEM Assets资源InDesign功能中使用的模板文件以及随附的所有注意事项。
version: 6.4, 6.5
topic: Content Management
feature: Templates
role: User
level: Intermediate
doc-type: Tutorial
exl-id: c418e94a-b18e-429a-b41c-2bf32e158598
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '507'
ht-degree: 1%

---

# 了解AEM Assets中的InDesign文件和资源模板 {#understanding-indesign-files-and-asset-templates-in-aem-assets}

本视频教程将介绍如何定义AEM Assets资源InDesign功能中使用的模板文件以及随附的所有注意事项。

## 构造InDesign模板文件 {#constructing-the-indesign-template-file}

>[!VIDEO](https://video.tv.adobe.com/v/19293?quality=12&learn=on)

1. 下载并打开 [**InDesign文件模板**](assets/asset-templates-tutorial-video--supporting-files.zip)
2. **打开“标记”面板，** 查看标记命名约定，并注意InDesign文件中的可创作元素已标记。 请记住，在AEM中只能编辑已标记的元素。

   * **“窗口”>“实用程序”>“标记”**

3. 在页面上，添加新文本元素，提供文本“Header”并应用 **标题** 段落样式。

   * **“窗口”>“样式”>“段落样式”**

   然后，创建并应用名为的新标记 **Page2Heading。**

4. 添加FPO徽标图像([在zip文件中提供](assets/asset-templates-tutorial-video--supporting-files.zip))到主页上的Logo元素。

   * **右键单击**&#x200B;并选择&#x200B;**“拟合”>“框架拟合选项……”>“内容拟合”>“按比例填充框架”**

   [了解有关框架拟合选项的更多信息](https://helpx.adobe.com/indesign/using/frames-objects.html#fitting_objects_to_frames)，并且这适合您的用例。

5. 通过原位粘贴从页面和页面中的母版模板向下复制标题（徽标和公司名称）。

   * 在第1页上，按住Shift-Cmd并单击macOS，或按住Shift-Alt并单击Windows，以选择从母版页公开的页眉并将其删除。
   * 从母版页，通过原位粘贴将页眉复制到第1页
   * 对第2页重复这些步骤

6. 打开“结构”面板，通过双击每个面板，确保所有结构元素都对应于InDesign文件中的实际元素。 删除任何未使用或不需要的元素。 确保所有标记都是语义的，并且元素已正确标记。

   >[!NOTE]
   >
   >请记住，构造错误的InDesign文件是AEM Asset Templates出现问题的最常见原因，因此请确保标记和结构干净正确。

## 在AEM Assets中创建和创作资源模板 {#creating-and-authoring-an-asset-template-in-aem-assets}

>[!VIDEO](https://video.tv.adobe.com/v/19294?quality=12&learn=on)

1. **启动InDesign Server** 8080端口。
2. 确保 **AEM创作实例配置为与您的InDesign Server交互**（反之亦然）。

   * [IDS工作程序Cloud Service配置](http://localhost:4502/etc/cloudservices/proxy/ids.html)
   * [云代理Cloud Service配置](http://localhost:4502/etc/cloudservices/proxy.html)
   * [AEM外部化器OSGi配置](http://localhost:4502/system/console/configMgr)

3. **已将InDesign文件上传到AEM Assets** 并允许AEM Workflow和InDesign Server完全处理资源。
4. **创建新模板** 下 **Assets >模板** ，然后在步骤#4中选择上传到AEM的InDesign文件。
5. **编辑资产模板** 在步骤#5中创建，并创作可编辑的字段。
6. 单击 **完成** 以生成资产模板的最终高保真演绎版。
7. 单击资产模板卡以打开，并查看资产演绎版以下载高保真演绎版。

## 其他资源 {#additional-resources}

InDesign模板文件和支持的图像

下载 [InDesign模板文件和支持的图像](assets/asset-templates-tutorial-video--supporting-files-1.zip)

* [InDesignCC试用版下载](https://creative.adobe.com/products/download/indesign)
* InDesign Server试用版可从以下位置下载： [Adobe预发行网站](https://www.adobeprerelease.com/) 或 [CC Enterprise客户可以联系其客户经理以请求InDesign Server试用许可证](https://www.adobe.com/products/indesignserver/faq.html)
