---
title: AEM Dispatcher虚URL功能
description: 了解AEM如何处理虚URL，以及使用重写规则映射更靠近投放边缘的内容的其他技术。
version: Experience Manager 6.5
topic: Administration, Performance
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
doc-type: Article
exl-id: 53baef9c-aa4e-4f18-ab30-ef9f4f5513ee
duration: 244
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1159'
ht-degree: 0%

---

# Dispatcher虚URL

[目录](./overview.md)

[&lt; — 上一页：Dispatcher刷新](./disp-flushing.md)

## 概述

本文档可帮助您了解AEM如何处理虚URL，以及其他一些使用重写规则映射更靠近交付边缘的内容的技术

## 什么是虚URL

如果您的内容位于合理的文件夹结构中，它并不总是位于易于引用的URL中。 虚URL类似于快捷方式。 引用实际内容所在位置的较短或唯一URL。

示例： `/aboutus`指向`/content/we-retail/us/en/about-us.html`

AEM作者可以选择在AEM中为某段内容设置虚URL属性并将其发布。

要使用此功能，您必须调整Dispatcher过滤器，以允许虚URL通过。 但这会使以作者必须设置这些虚页面条目的速率调整Dispatcher配置文件变得不合理。

因此，Dispatcher模块具有自动允许内容树中列出的任何虚值的功能。


## 工作原理

### 创作虚URL

作者在AEM中访问页面，单击页面属性，并在&#x200B;_虚URL_&#x200B;部分中添加条目。 在保存更改并激活页面时，会将虚值分配给页面。

在添加&#x200B;_虚URL_&#x200B;条目时，作者还可以选中&#x200B;_重定向虚URL_&#x200B;复选框，这将导致虚URL行为为302重定向。 这意味着浏览器被告知转到新URL（通过`Location`响应标头），并且浏览器对新URL发出新请求。

#### 触控UI：

![网站编辑器屏幕上AEM创作UI的下拉对话框菜单](assets/disp-vanity-url/aem-page-properties-drop-down.png "aem-page-properties — 下拉菜单")

![aem页面属性对话框页面](assets/disp-vanity-url/aem-page-properties.png "aem-page-properties")

#### 经典内容查找器：

![AEM siteadmin classic ui sidekick页面属性](assets/disp-vanity-url/aem-page-properties-sidekick.png "aem-page-properties-sidekick")

![经典UI页面属性对话框](assets/disp-vanity-url/aem-page-properties-classic.png "aem-page-properties-classic")


>[!NOTE]
>
>了解这容易导致名称空间问题。 虚条目对于所有页面都是全局的，这只是您需要规划变通方法的简短介绍之一，我们将在后面解释其中的几个变通方法。


## 资源解析/映射

每个虚条目都是用于内部重定向的sling映射条目。

通过访问AEM实例Felix控制台( `/system/console/jcrresolver` )可以看到这些映射

以下是由虚条目创建的映射条目的屏幕截图：
资源解析规则中虚条目的![控制台屏幕截图](assets/disp-vanity-url/vanity-resource-resolver-entry.png "vanity-resource-resolver-entry")

在上例中，当我们要求AEM实例访问`/aboutus`时，它解析为`/content/we-retail/us/en/about-us.html`

## Dispatcher自动允许过滤器

处于安全状态的Dispatcher会通过Dispatcher过滤掉路径`/`上的请求，因为这是JCR树的根目录。

确保发布者仅允许来自`/content`和其他安全路径等的内容，而不允许来自`/system`等路径的内容，这一点很重要。

下面是`/`的基本文件夹中存在的问题和虚URL，那么我们如何在保持安全的同时允许它们访问发布者？

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

`/delay`参数（以秒为单位）不按固定间隔运行，而是按条件检查。 在收到未列出URL的请求时，Dispatcher会评估`/file`（用于存储已识别的虚名URL的列表）的修改时间戳。 如果当前时刻与`/file`最后一次修改之间的时间差小于`/delay`持续时间，则不会刷新`/file`。 刷新`/file`有两种情况：

1. 传入请求针对的URL未缓存或未列在`/file`中。
1. 自上次更新`/file`以来，已过去至少`/delay`秒。

此机制旨在防御拒绝服务(DoS)攻击，这类攻击可能会利用虚名URL功能用请求淹没Dispatcher。

简单来说，仅当请求到达的URL不在`/file`中时，并且如果`/file`的上次修改早于`/delay`时段，才会更新包含虚名URL的`/file`。

要显式触发`/file`的刷新，您可以在确保自上次更新以来经过了所需的`/delay`时间后请求不存在的URL。 用于此目的的示例URL包括：

- `https://dispatcher-host-name.com/this-vanity-url-does-not-exist`
- `https://dispatcher-host-name.com/please-hand-me-that-planet-maestro`
- `https://dispatcher-host-name.com/random-vanity-url`

此方法强制Dispatcher更新`/file`，前提是指定的`/delay`间隔自其上次修改以来已过。

它将响应的缓存存储在`/file`参数中，因此在此示例`/tmp/vanity_urls`中

因此，如果您通过URI访问AEM实例，则可以看到它获取的内容：

从/libs/granite/dispatcher/content/vanityUrls.html](assets/disp-vanity-url/vanity-url-component.png "vanity-url-component")呈现的内容的![屏幕截图

它实际上是一个非常简单的列表

## 将规则重写为虚规则

为什么要提到使用重写规则，而不使用如上所述的内置到AEM中的默认机制？

简单来说，命名空间问题、性能和可以更好地处理的更高级别逻辑。

让我们再看一下虚条目`/aboutus`示例，其内容`/content/we-retail/us/en/about-us.html`使用Apache的`mod_rewrite`模块完成此操作。

```
RewriteRule ^/aboutus /content/we-retail/us/en/about-us.html [PT,L,NC]
```

此规则将查找虚`/aboutus`，并从带有PT标志（通过）的渲染器中获取完整路径。

它还停止处理所有其他规则L标记（最后），这意味着它不必遍历大型规则列表，如JCR解析必须执行的规则列表。

同时不必代理请求，并等待AEM发布者响应此方法的这两个元素，因此提高了其性能。

此处锦上添花的是NC标志（不区分大小写），这意味着如果客户键入的URI是`/AboutUs`而不是`/aboutus`，则它仍然有效。

要创建重写规则以执行此操作，您需要在Dispatcher上创建一个配置文件（示例： `/etc/httpd/conf.d/rewrites/examplevanity_rewrite.rules`），并将其包含在`.vhost`文件中，该文件处理需要应用这些虚URL的域。

以下是`/etc/httpd/conf.d/enabled_vhosts/we-retail.vhost`中包含的示例代码段

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

使用`mod_rewrite`控制虚条目具有以下优点

- 解析内容的速度更快
- 更接近最终用户内容请求的边缘
- 更多可扩展性和选项，用于控制如何在其他条件上映射内容
- 可以不区分大小写

请同时使用这两种方法，但以下是有关何时使用哪种方法的建议和标准：

- 如果虚值是临时的，并且规划的流量级别较低，则使用AEM内置功能
- 如果虚值是不经常更改且频繁使用的固定端点，则使用`mod_rewrite`规则。
- 如果虚命名空间（例如： `/aboutus`）必须对同一AEM实例上的多个品牌重用，则使用重写规则。

>[!NOTE]
>
>如果要使用AEM虚功能并避免命名空间，可以制定命名约定。 使用嵌套如`/brand1/aboutus`、`brand2/aboutus`、`brand3/aboutus`的虚URL。

[下一页 — >常用日志记录](./common-logs.md)
