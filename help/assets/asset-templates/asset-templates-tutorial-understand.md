---
title: 了解AEM Assets中的InDesign文件和资产模板
description: 本视频教程将介绍如何定义一个InDesign文件以及所有随附的注意事项，以便在AEM Assets的“资产模板”功能中使用。
version: 6.4, 6.5
topic: Content Management
role: User
level: Intermediate
exl-id: c418e94a-b18e-429a-b41c-2bf32e158598
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '507'
ht-degree: 1%

---

# 了解AEM Assets中的InDesign文件和资产模板 {#understanding-indesign-files-and-asset-templates-in-aem-assets}

本视频教程将介绍如何定义一个InDesign文件以及所有随附的注意事项，以便在AEM Assets的“资产模板”功能中使用。

## 构建InDesign模板文件 {#constructing-the-indesign-template-file}

>[!VIDEO](https://video.tv.adobe.com/v/19293/?quality=9&learn=on)

1. 下载并打开 [**InDesign文件模板**](assets/asset-templates-tutorial-video--supporting-files.zip)
2. **打开“标记”面板，** 请查看标记命名约定，并注意InDesign文件中的可创作元素已标记。 请记住，只有标记的元素才可在AEM中编辑。

   * **“窗口”>“实用程序”>“标记”**

3. 在页面上，添加新文本元素，提供文本“标题”并应用 **标题** 段落样式。

   * **窗口>样式>段落样式**

   然后，创建并应用一个名为 **页面2标题。**

4. 添加FPO徽标图像([在zip文件中提供](assets/asset-templates-tutorial-video--supporting-files.zip))到主控页面上的Logo元素。

   * **右键单击**&#x200B;选择&#x200B;**“管接头”(Fitting)>“框架管接头选项……”(Fitting)>“内容管接头”(Content Fitting)>“按比例填充框架”(Fill Frame)**
   [了解有关“框架拟合”选项的更多信息](https://helpx.adobe.com/indesign/using/frames-objects.html#fitting_objects_to_frames)，并且适用于您的用例。

5. 通过就地粘贴，从页面和页面的主控模板中向下复制标题（徽标和公司名称）。

   * 在第1页上，按住Shift键并单击macOS，或按住Shift键并单击Windows，以选择从主控页面公开的标题，并将其删除。
   * 从主控页面中，通过就地粘贴将标题复制到第1页
   * 对第2页重复步骤

6. 通过双击每个元素，打开“结构”面板，确保所有结构元素都与InDesign文件中的实际元素相对应。 删除任何未使用或未需要的元素。 确保所有标记都具有语义，且元素标记正确。

   >[!NOTE]
   >
   >请记住，构造不当的InDesign文件是AEM资产模板问题的最常见原因，因此请确保标记和结构清晰正确。

## 在AEM Assets中创建和创作资产模板 {#creating-and-authoring-an-asset-template-in-aem-assets}

>[!VIDEO](https://video.tv.adobe.com/v/19294/?quality=9&learn=on)

1. **开始InDesign Server** 在8080端口。
2. 确保 **AEM创作实例配置为与您的InDesign Server交互**（反之亦然）。

   * [IDS工作Cloud Service配置](http://localhost:4502/etc/cloudservices/proxy/ids.html)
   * [云代理Cloud Service配置](http://localhost:4502/etc/cloudservices/proxy.html)
   * [AEM Externalizer OSGi配置](http://localhost:4502/system/console/configMgr)

3. **已将InDesign文件上传到AEM Assets** 并允许AEM工作流和InDesign Server完全处理资产。
4. **创建新模板** 在 **资产>模板** ，然后在步骤#4中选择已上传到AEM的InDesign文件。
5. **编辑资产模板** 创建#5，并创作可编辑的字段。
6. 单击 **完成** 以生成资产模板的最终高保真演绎版。
7. 单击资产模板卡以打开，然后查看资产演绎版以下载高保真演绎版。

## 其他资源 {#additional-resources}

InDesign模板文件和支持图像

下载 [InDesign模板文件和支持图像](assets/asset-templates-tutorial-video--supporting-files-1.zip)

* [InDesignCC试用版下载](https://creative.adobe.com/products/download/indesign)
* InDesign Server试用版可从 [Adobe预发行网站](https://www.adobeprerelease.com/) 或 [CC企业客户可联系其客户经理以请求获得InDesign Server试用许可证](https://www.adobe.com/products/indesignserver/faq.html)
