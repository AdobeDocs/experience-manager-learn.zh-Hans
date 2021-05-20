---
title: '了解AEM Assets中的InDesign文件和资产模板 '
description: 本视频教程将介绍如何定义一个InDesign文件以及所有随附的注意事项，以便在AEM Assets的“资产模板”功能中使用。
version: 6.3, 6.4, 6.5
topic: 内容管理
role: Business Practitioner
level: Intermediate
source-git-commit: b4fa992abe22e3a546d651e465d6ffc9e415aee2
workflow-type: tm+mt
source-wordcount: '496'
ht-degree: 1%

---


# 了解AEM Assets中的InDesign文件和资产模板{#understanding-indesign-files-and-asset-templates-in-aem-assets}

本视频教程将介绍如何定义一个InDesign文件以及所有随附的注意事项，以便在AEM Assets的“资产模板”功能中使用。

## 构建InDesign模板文件{#constructing-the-indesign-template-file}

>[!VIDEO](https://video.tv.adobe.com/v/19293/?quality=9&learn=on)

1. 下载并打开&#x200B;[**InDesign文件模板**](assets/asset-templates-tutorial-video--supporting-files.zip)
2. **打开“标记”面板，** 查看标记命名约定，并注意InDesign文件中的可创作元素已进行标记。请记住，只有标记的元素才可在AEM中编辑。

   * **“窗口”>“实用程序”>“标记”**

3. 在页面上，添加新文本元素，提供文本“Header”并应用&#x200B;**Heading**&#x200B;段落样式。

   * **窗口>样式>段落样式**

   然后，创建并应用名为&#x200B;**Page2Heading.**&#x200B;的新标记

4. 将FPO徽标图像（在zip](assets/asset-templates-tutorial-video--supporting-files.zip)中提供的[）添加到主控页面上的徽标元素。

   * **右键单**&#x200B;击并&#x200B;**按比例选择“管接头”>“框架管接头选项……”>“内容管接头”>“填充框架”**
   [进一步了解“框架拟合”选项](https://helpx.adobe.com/indesign/using/frames-objects.html#fitting_objects_to_frames)，它适合您的用例。

5. 通过就地粘贴，从页面和页面的主控模板中向下复制标题（徽标和公司名称）。

   * 在第1页上，按住Shift键并单击macOS，或在Windows上按住Shift键并单击Alt键，以选择从主控页面公开的标题，然后将其删除。
   * 从主控页面中，通过就地粘贴将标题复制到第1页
   * 对第2页重复步骤

6. 通过双击每个元素，打开“结构”面板，确保所有结构元素都与InDesign文件中的实际元素相对应。 删除任何未使用或未需要的元素。 确保所有标记都具有语义，且元素标记正确。

   >[!NOTE]
   >
   >请记住，构造不当的InDesign文件是AEM资产模板问题的最常见原因，因此请确保标记和结构清晰正确。

## 在AEM Assets中创建和创作资产模板{#creating-and-authoring-an-asset-template-in-aem-assets}

>[!VIDEO](https://video.tv.adobe.com/v/19294/?quality=9&learn=on)

1. **启动InDesign** 服务器端口8080。
2. 确保将&#x200B;**AEM创作实例配置为与InDesign Server**&#x200B;交互（反之亦然）。

   * [IDS工作Cloud Service配置](http://localhost:4502/etc/cloudservices/proxy/ids.html)
   * [云代理Cloud Service配置](http://localhost:4502/etc/cloudservices/proxy.html)
   * [AEM Externalizer OSGi配置](http://localhost:4502/system/console/configMgr)

3. **已将InDesign文件上传到AEM** 资产，并允许AEM工作流和InDesign Server完全处理资产。
4. **在资产>模** 板下 **创建新** 的InDesign文件，并在步骤#4中选择上传到AEM的模板文件。
5. **编辑在第#5步** 中创建的资产模板，并创作可编辑的字段。
6. 单击&#x200B;**完成**&#x200B;以生成资产模板的最终高保真演绎版。
7. 单击资产模板卡以打开，然后查看资产演绎版以下载高保真演绎版。

## 其他资源 {#additional-resources}

InDesign模板文件和支持图像

下载[InDesign模板文件并支持Images](assets/asset-templates-tutorial-video--supporting-files-1.zip)

* [InDesignCC试用版下载](https://creative.adobe.com/products/download/indesign)
* [CC企业客户可联系其客户经理以请求获取InDesign Server试用许可证](https://www.adobe.com/products/indesignserver/faq.html)
