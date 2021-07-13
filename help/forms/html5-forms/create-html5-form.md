---
title: 创建HTML5 Forms
description: 创建和配置HTML5表单
feature: 移动设备表单
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
kt: 4419
thumbnail: kt-4419.jpg
topic: 开发
role: User
level: Beginner
source-git-commit: b0bca57676813bd353213b4808f99c463272de85
workflow-type: tm+mt
source-wordcount: '488'
ht-degree: 1%

---


# 创建HTML5表单

HTML5表单是Adobe Experience Manager中的一项新功能，可渲染HTML5格式的XFA表单模板(xdp)。 此功能允许在不支持基于XFA的PDF的移动设备和桌面浏览器上渲染表单。 HTML5表单不仅支持XFA表单模板的现有功能，还为移动设备添加了新功能，如涂写签名。

## 先决条件

请确保您有AEM Forms的工作实例。 请按照[安装指南](https://docs.adobe.com/content/help/en/experience-manager-65/forms/install-aem-forms/osgi-installation/installing-configuring-aem-forms-osgi.html)安装和配置AEM Forms

## 创建您的第一个HTML5表单

1. [下载并解压缩zip文件的内容](assets/assets.zip)。zip文件包含xdp和数据文件
2. [导航到Forms和文档](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
3. 单击创建 — >文件上传
4. 选择在步骤2中下载的xdp模板

## 以HTML形式预览

xdp可以以HTML5格式或PDF格式预览。 要预览HTML5格式的xdp，请执行以下步骤

* 点按新上传的xdp，然后单击&#x200B;_预览 — >预览为HTML_。 您应会看到xdp呈现为HTML5

>[!NOTE]
>当您选择&#x200B;_预览为PDF_&#x200B;选项时，呈现的PDF将不会显示在浏览器中，因为AEM Forms渲染需要Acrobat插件的动态PDF。您必须下载PDF并使用Adobe Acrobat/Reader将其打开才能查看


## 使用数据预览

要使用数据文件预览HTML5格式的xdp，请执行以下步骤：

* 点按新上传的xdp，然后单击&#x200B;_预览 — >使用数据预览_。 浏览并选择数据文件，然后单击&#x200B;_预览_。
* 您应会看到以HTML5格式呈现的模板，其中已预填充数据

## 浏览xdp模板的高级属性

xdp模板的高级属性允许您指定发布日期、提交处理程序、表单的呈现配置文件、预填充服务等。 要查看模板的高级属性，请点按xdp ，然后单击&#x200B;_属性 — >高级_。 在此，您将找到许多资产。 此处介绍了其中一些属性。

**提交URL**  — 这是将处理HTML5表单提交的URL。我们将在下一课中介绍此内容。 如果未在此处指定提交URL，则会调用默认的提交处理程序，该处理程序会将表单数据返回到浏览器。

**HTML渲染配置文件**  - HTML5表单具有“配置文件”概念，这些配置文件作为REST端点公开，以便能够移动渲染表单模板。大多数情况下，默认渲染配置文件应足以渲染表单。 如果默认呈现配置文件不符合您的需求，则可以创建一个[自定义配置文件](https://docs.adobe.com/content/help/en/experience-manager-64/forms/html5-forms/custom-profile.html)并将其与表单关联。

**预填充服务**  — 预填充服务通常用于使用从后端数据源获取的数据填充您的表单。

