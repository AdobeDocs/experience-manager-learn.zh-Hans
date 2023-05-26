---
title: AEM Dispatcher虚URL功能
description: 了解AEM如何处理虚名URL，以及使用重写规则映射更靠近投放边缘的内容的其他技术。
version: 6.5
topic: Administration, Performance
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
exl-id: 53baef9c-aa4e-4f18-ab30-ef9f4f5513ee
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '994'
ht-degree: 4%

---

# Dispatcher虚URL

[目录](./overview.md)

[&lt; — 上一步： Dispatcher刷新](./disp-flushing.md)

## 概述

本文档将帮助您了解AEM如何处理虚名URL，以及其他一些使用重写规则映射更接近投放边缘的内容技术

## 什么是虚URL

当您的内容位于合理的文件夹结构中时，它并不总是位于易于引用的URL中。  虚URL类似于快捷键。  引用真实内容所在位置的较短或唯一URL。

示例： `/aboutus` 指向 `/content/we-retail/us/en/about-us.html`

AEM作者可以选择在AEM中为一段内容设置虚URL属性并将其发布。

要使用此功能，您必须调整Dispatcher筛选器以允许虚URL通过。  以作者设置这些虚页面条目所需的速率调整Dispatcher配置文件变得不合理。

因此，Dispatcher模块具有自动允许内容树中列出的任何虚值的功能。


## 工作原理

### 创作虚URL

作者在AEM中访问页面，访问页面属性，并在虚URL部分添加条目。

保存更改并激活页面后，会将虚值分配到此页面。

#### 触屏 UI:

![网站编辑器屏幕上AEM创作UI的下拉对话框菜单](assets/disp-vanity-url/aem-page-properties-drop-down.png "aem-page-properties-drop-down")

![aem页面属性对话框页面](assets/disp-vanity-url/aem-page-properties.png "aem-page-properties")

#### 经典内容查找器：

![AEM siteadmin classic ui sidekick页面属性](assets/disp-vanity-url/aem-page-properties-sidekick.png "aem-page-properties-sidekick")

![经典UI页面属性对话框](assets/disp-vanity-url/aem-page-properties-classic.png "aem-page-properties-classic")

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>注意：</b>
请注意，这很容易出现名称空间问题。

虚条目对于所有页面都是全局的，这只是您需要规划的短期代码之一，我们将稍后解释一些解决方法。
</div>

## 资源解析/映射

每个虚条目都是用于内部重定向的sling映射条目。

通过访问AEM实例Felix控制台( `/system/console/jcrresolver` )

以下是由虚条目创建的映射条目的屏幕截图：
![资源解析规则中虚条目的控制台屏幕截图](assets/disp-vanity-url/vanity-resource-resolver-entry.png "vanity-resource-resolver-entry")

在上例中，当我们请求AEM实例访问时 `/aboutus` 它将 `/content/we-retail/us/en/about-us.html`

## Dispatcher自动允许过滤器

处于安全状态的Dispatcher过滤掉路径上的请求 `/` 通过Dispatcher，因为这是JCR树的根。

务必确保发布者仅允许来自 `/content` 和其他安全路径等。  而不是像这样的路径 `/system` 等等……

下面是基础文件夹中存在的问题和虚URL `/` 那么，我们如何允许他们在保持安全的同时联系出版商呢？

简单Dispatcher具有自动过滤器允许机制，您必须安装AEM包，然后将Dispatcher配置为指向该包页面。

[https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html?package=/content/software-distribution/en/details.html/content/dam/aem/public/adobe/packages/granite/vanityurls-components](https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html?package=/content/software-distribution/en/details.html/content/dam/aem/public/adobe/packages/granite/vanityurls-components)

Dispatcher的场文件中有一个配置部分：

```
/vanity_urls { 
    /url    "/libs/granite/dispatcher/content/vanityUrls.html" 
    /file   "/tmp/vanity_urls" 
    /delay  300 
}
```

此配置会告知Dispatcher每300秒从它的AEM实例中获取此URL，以获取我们希望允许通过的项目列表。

它将响应的缓存存储在 `/file` 参数，因此在此示例中 `/tmp/vanity_urls`

因此，如果您通过URI访问AEM实例，将看到它获取的内容：
![从/libs/granite/dispatcher/content/vanityUrls.html呈现的内容屏幕截图](assets/disp-vanity-url/vanity-url-component.png "vanity-url-component")

它实际上是一个非常简单的列表

## 将规则重写为虚规则

我们为什么要提到使用重写规则，而不使用如上所述内置到AEM中的默认机制？

简单来说，命名空间问题、性能和可以更好地处理的更高级别逻辑。

让我们再看一下虚条目示例 `/aboutus` 内容 `/content/we-retail/us/en/about-us.html` 使用Apache的 `mod_rewrite` 模块来完成此操作。

```
RewriteRule ^/aboutus /content/we-retail/us/en/about-us.html [PT,L,NC]
```

此规则将查找虚名 `/aboutus` 并从带有PT标志（通过）的渲染器中获取完整路径。

它还将停止处理所有其他规则L标记（最后），这意味着它不必遍历大型规则列表，如JCR解析必须执行的规则。

同时不必代理请求并等待AEM发布者响应此方法的这两个元素，因此提高了其性能。

此处锦上添花的是NC标志（不区分大小写），这意味着如果客户使用以下方式缓冲URI `/AboutUs` 而不是 `/aboutus` 它仍然有效，并允许获取正确的页面。

要创建重写规则以执行此操作，您应在Dispatcher上创建配置文件(示例： `/etc/httpd/conf.d/rewrites/examplevanity_rewrite.rules`)，并将其包含在 `.vhost` 用于处理需要应用这些虚URL的域的文件。

以下是include inside的示例代码段 `/etc/httpd/conf.d/enabled_vhosts/we-retail.vhost`

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

## 何种方法和何处

使用AEM控制虚条目具有以下好处
- 作者可以动态创建它们
- 它们与内容共存，并可与内容一起打包

使用 `mod_rewrite` 控制虚条目具有以下优点
- 更快地解析内容
- 更接近最终用户内容请求的边缘
- 更多可扩展性和选项，用于控制如何在其他条件上映射内容
- 可以不区分大小写

请同时使用这两种方法，但以下是有关在下列情况下使用哪种方法的建议和标准：
- 如果虚值是临时的，并且规划的流量级别较低，则使用AEM内置功能
- 如果虚值是不经常更改且频繁使用的固定端点，则使用 `mod_rewrite` 规则。
- 如果虚命名空间(例如： `/aboutus`)必须在同一AEM实例上为多个品牌重用，然后使用重写规则。

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>注意:</b>

如果要使用AEM虚功能并避免命名空间，可以制定命名约定。  使用类似嵌套的虚URL `/brand1/aboutus`， `brand2/aboutus`， `brand3/aboutus`.
</div>

[下一步 — >常用日志记录](./common-logs.md)
