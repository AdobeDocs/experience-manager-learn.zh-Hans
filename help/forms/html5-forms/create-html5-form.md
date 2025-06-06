---
title: 创建HTML5 Forms
description: 创建和配置HTML5表单
feature: Mobile Forms
doc-type: article
version: Experience Manager 6.5
jira: KT-4419
thumbnail: kt-4419.jpg
topic: Development
role: User
level: Beginner
exl-id: 67a01c41-d284-4518-adb5-21702e22ccfa
last-substantial-update: 2019-07-07T00:00:00Z
duration: 101
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '462'
ht-degree: 0%

---

# 创建HTML5表单

HTML5 forms是Adobe Experience Manager中的一项新功能，可渲染HTML5格式的XFA表单模板(xdp)。 凭借此功能，可以在那些不支持基于XFA的PDF的移动设备和桌面浏览器上渲染表单。 HTML5 forms不仅支持XFA表单模板的现有功能，还增加了适用于移动设备的新功能，如涂写签名。

## 先决条件

请确保您具有AEM Forms的工作实例。 请按照[安装指南](https://experienceleague.adobe.com/docs/experience-manager-65/forms/install-aem-forms/osgi-installation/installing-configuring-aem-forms-osgi.html?lang=zh-Hans)安装和配置AEM Forms

## 创建您的第一个HTML5表单

1. [下载并解压缩zip文件的内容](assets/assets.zip)。 zip文件包含xdp和数据文件
2. [导航到Forms和文档](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
3. 单击创建 — >文件上传
4. 选择在步骤2中下载的xdp模板

## 预览为HTML

可以采用HTML5格式或PDF格式预览xdp。 要预览HTML5格式的xdp，请执行以下步骤

* 点按新上传的xdp，然后单击&#x200B;_预览 — >预览为HTML_。 您应该看到xdp呈现为HTML5

>[!NOTE]
>当您选择&#x200B;_作为PDF预览_&#x200B;选项时，已渲染的PDF将不会显示在浏览器中，因为AEM Forms渲染需要Acrobat插件的动态PDF。您必须下载PDF并使用Adobe Acrobat/Reader打开它才能查看


## 使用数据预览

要使用数据文件预览HTML5格式的xdp，请执行以下步骤：

* 点按新上传的xdp并单击&#x200B;_预览 — >使用数据预览_。 浏览并选择数据文件，然后单击&#x200B;_预览_。
* 您应该会看到以HTML5格式呈现的模板预填充了数据

## 浏览xdp模板的高级属性

xdp模板的高级属性允许您指定发布日期、提交处理程序、表单的渲染配置文件、预填充服务等。 要查看模板的高级属性，请点按xdp并单击&#x200B;_属性 — >高级_。 您可以在此处找到许多资产。 此处介绍了其中一些属性。

**提交URL** — 这是将处理您的HTML5表单提交的URL。 我们将在下一课中介绍此内容。 如果未在此指定提交URL，则会调用默认提交处理程序，以便将表单数据返回到浏览器。

**HTML渲染配置文件** - HTML5表单具有配置文件概念，该配置文件公开为REST端点以启用表单模板的移动渲染。 大多数情况下，默认渲染配置文件应足以渲染表单。 如果默认的渲染配置文件不符合您的需求，则可以创建[自定义配置文件](https://experienceleague.adobe.com/docs/experience-manager-65/forms/html5-forms/custom-profile.html?lang=zh-Hans)并与该表单关联。

**预填充服务** — 预填充服务通常用于使用从后端数据源获取的数据填充表单。
