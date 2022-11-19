---
title: AEM Dispatcher虚URL功能
description: 了解AEM如何使用重写规则将内容映射到更靠近交付边缘的位置，从而处理虚URL和其他技术。
version: 6.5
topic: Administration, Performance
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
source-git-commit: 04cd4002af7028ee9e3b1e1455b6346c56446245
workflow-type: tm+mt
source-wordcount: '994'
ht-degree: 4%

---


# 调度程序虚URL

[目录](./overview.md)

[&lt; — 上一个：调度程序刷新](./disp-flushing.md)

## 概述

本文档将帮助您了解AEM如何处理虚URL以及使用重写规则将内容映射到更靠近交付边缘的一些其他技术

## 什么是虚URL

如果您的内容位于合理的文件夹结构中，则并非总是位于易于引用的URL中。  虚URL就像快捷键一样。  引用实际内容所在位置的较短或唯一URL。

示例： `/aboutus` 指向 `/content/we-retail/us/en/about-us.html`

AEM作者可以选择在AEM中为某段内容设置虚URL属性并发布该属性。

要使用此功能，您必须调整调度程序过滤器，以允许虚通过。  对于以作者设置这些虚页面条目所需的速度调整Dispatcher配置文件，这变得不合理。

因此，Dispatcher模块具有自动允许内容树中列为虚值的任何内容的功能。


## 工作原理

### 创作虚URL

作者访问AEM中的页面，访问页面属性并在虚URL部分添加条目。

保存更改并激活页面后，现在会将虚值分配给此页面。

#### 触屏 UI:

![站点编辑器屏幕上用于AEM创作UI的下拉对话框菜单](assets/disp-vanity-url/aem-page-properties-drop-down.png "aem-page-properties — 下拉列表")

![aem页面属性对话框页面](assets/disp-vanity-url/aem-page-properties.png "aem-page-properties")

#### 经典内容查找器：

![AEM siteadmin classic ui sidekick页面属性](assets/disp-vanity-url/aem-page-properties-sidekick.png "aem-page-properties-sidekick")

![经典UI页面属性对话框](assets/disp-vanity-url/aem-page-properties-classic.png "aem-page-properties-classic")

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>注意：</b>
请了解，这很容易导致命名空间问题。

虚条目对所有页面都是全局的，这只是您必须针对解决方法进行规划的不足之一，我们稍后会对其中一些解决方法进行解释。
</div>

## 资源解析/映射

内部重定向的每个虚条目都是sling映射条目。

通过访问AEM实例Felix控制台( `/system/console/jcrresolver` )

以下是由虚条目创建的映射条目的屏幕截图：
![资源解析规则中虚条目的控制台屏幕截图](assets/disp-vanity-url/vanity-resource-resolver-entry.png "vanity-resource-resolver-entry")

在上例中，当我们请求AEM实例访问 `/aboutus` 它将决心 `/content/we-retail/us/en/about-us.html`

## Dispatcher自动允许过滤器

处于安全状态的调度程序在路径上筛选出请求 `/` 通过Dispatcher，因为这是JCR树的根。

务必确保发布者仅允许 `/content` 其他安全路径等……  而不是像 `/system` 等等。

以下是基本文件夹中存在的麻烦和虚URL `/` 那么，我们如何让他们在保持安全的同时接触出版商？

简单的Dispatcher具有自动过滤允许机制，您必须安装AEM包，然后配置Dispatcher以指向该包页面。

[https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html?package=/content/software-distribution/en/details.html/content/dam/aem/public/adobe/packages/granite/vanityurls-components](https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html?package=/content/software-distribution/en/details.html/content/dam/aem/public/adobe/packages/granite/vanityurls-components)

Dispatcher的场文件中有一个配置部分：

```
/vanity_urls { 
    /url    "/libs/granite/dispatcher/content/vanityUrls.html" 
    /file   "/tmp/vanity_urls" 
    /delay  300 
}
```

此配置告知Dispatcher从其AEM实例获取此URL，每300秒会前线一次，以获取我们允许通过的项目列表。

它将响应的缓存存储在 `/file` 参数，在本例中为 `/tmp/vanity_urls`

因此，如果您在URI中访问AEM实例，您将看到其获取的内容：
![从/libs/granite/dispatcher/content/vanityUrls.html呈现的内容屏幕截图](assets/disp-vanity-url/vanity-url-component.png "vanity-url-component")

这真的很简单

## 将规则重写为虚规则

为什么我们要提及使用重写规则，而不是如上所述内置到AEM中的默认机制？

简单地说明了命名空间问题、性能和更高级别的逻辑，这些逻辑可以得到更好的处理。

让我们查看虚条目的示例 `/aboutus` 内容 `/content/we-retail/us/en/about-us.html` 使用Apache的 `mod_rewrite` 模块来完成此操作。

```
RewriteRule ^/aboutus /content/we-retail/us/en/about-us.html [PT,L,NC]
```

此规则将查找虚 `/aboutus` 并从具有PT标志（传递）的渲染器中获取完整路径。

它还将停止处理所有其他规则L标记（最后），这意味着它不必遍历大量规则列表，如JCR解析必须执行的操作。

除了不必代理请求并等待AEM发布者响应此方法的这两个元素之外，还可以让其性能大大提高。

这里锦上添花的是NC标志（不区分大小写），这表示客户是否在 `/AboutUs` 而不是 `/aboutus` 仍然可以正常使用，并允许获取正确的页面。

要创建重写规则以执行此操作，您应在Dispatcher上创建配置文件(示例： `/etc/httpd/conf.d/rewrites/examplevanity_rewrite.rules`)并将其包含在 `.vhost` 用于处理需要应用这些虚url的域的文件。

以下是内部包含的示例代码片段 `/etc/httpd/conf.d/enabled_vhosts/we-retail.vhost`

```
<VirtualHost *:80> 
 ServerName weretail.com 
 ServerAlias www.weretail.com 
        ........ SNIP ........ 
 <IfModule mod_rewrite.c> 
  ReWriteEngine on 
  LogLevel warn rewrite:info 
  Include /etc/httpd/conf.d/rewrites/examplevanity_rewrite.rules 
 </IfModule> 
        ........ SNIP ........ 
</VirtualHost>
```

## 哪种方法和位置

使用AEM控制虚条目具有以下好处
- 作者可以即时创建它们
- 它们与内容一起生活，并可与内容一起打包

使用 `mod_rewrite` 控制虚条目具有以下好处
- 更快地解析内容
- 更接近最终用户内容请求的边缘
- 更多可扩展性和选项，用于控制如何在其他条件上映射内容
- 可能不区分大小写

请同时使用这两种方法，但以下是相关建议和标准，供您在以下情况下使用：
- 如果虚值是临时的，并且计划的流量级别较低，则使用AEM内置功能
- 如果虚值是不经常更改且经常使用的主要端点，则使用 `mod_rewrite` 规则。
- 如果虚命名空间(例如： `/aboutus`)必须对同一AEM实例上的多个品牌重复使用，然后使用重写规则。

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>注意:</b>

如果要使用AEM虚功能并避免命名空间，可以制定命名约定。  使用嵌套为的虚URL `/brand1/aboutus`, `brand2/aboutus`, `brand3/aboutus`.
</div>

[下一步 — >常见日志记录](./common-logs.md)