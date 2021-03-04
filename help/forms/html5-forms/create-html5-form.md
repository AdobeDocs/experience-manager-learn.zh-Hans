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
role: 业务从业者
level: 初学者
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '491'
ht-degree: 1%

---


# 创建HTML5表单

HTML5表单是Adobe Experience Manager中的一项新功能，可优惠HTML5格式的XFA表单模板(xdp)的呈现。 此功能支持在不支持基于XFA的PDF的移动设备和桌面浏览器上呈现表单。 HTML5表单不仅支持XFA表单模板的现有功能，还为移动设备添加了新功能，如涂抹签名。

## 先决条件

请确保您有AEM Forms的工作实例。 请按照[安装指南](https://docs.adobe.com/content/help/en/experience-manager-65/forms/install-aem-forms/osgi-installation/installing-configuring-aem-forms-osgi.html)安装和配置AEM Forms

## 创建您的第一个HTML5表单

1. [下载并解压zip文件的内容](assets/assets.zip)。zip文件包含xdp和数据文件
2. [导航到Forms和文档](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
3. 单击创建 — >文件上传
4. 选择在步骤2中下载的xdp模板

## 预览为HTML

xdp可以采用HTML5格式或PDF格式进行预览。 要预览HTML5格式的xdp，请执行以下步骤

* 点按新上传的xdp，然后单击&#x200B;_预览->预览为HTML_。 您应当看到xdp以HTML5呈现

>[!NOTE]
>当您选择&#x200B;_预览为PDF_&#x200B;选项时，渲染的PDF将不会显示在浏览器中，因为AEM Forms渲染需要Acrobat插件的动态pdf。您必须下载PDF并使用Adobe Acrobat/Reader打开它以视图


## 使用数据预览

要将HTML5格式的xdp与数据文件预览，请执行以下步骤：

* 点按新上载的xdp，然后单击&#x200B;_预览->预览，其中Data_。 浏览并选择预览文件，然后单击&#x200B;_数据_。
* 您应当看到以HTML5格式呈现的模板已预填充数据

## 浏览xdp模板的高级属性

xdp模板的高级属性允许您指定发布日期、提交处理函数、表单的渲染用户档案、预填服务等。 要视图模板的高级属性，请点按xdp，然后单击&#x200B;_属性 — >高级_。 您将在此找到许多属性。 此处介绍其中一些属性。

**提交URL**  — 此URL将处理您的HTML5表单提交。我们将在下一课讨论此内容。 如果未在此处指定提交URL，将调用默认的提交处理函数，该函数将表单数据返回到浏览器。

**HTML渲染用户档案** - HTML5表单具有用户档案的概念，这些作为REST端点公开，以支持表单模板的移动渲染。大多数情况下，默认渲染用户档案应足以渲染表单。 如果默认渲染用户档案不满足您的需求，则可以创建[自定义用户档案](https://docs.adobe.com/content/help/en/experience-manager-64/forms/html5-forms/custom-profile.html)并与表单关联。

**预填服务**  — 预填服务通常用于用从后端数据源获取的数据填充表单。

