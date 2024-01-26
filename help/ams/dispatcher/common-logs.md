---
title: AEM Dispatcher常用日志
description: 从Dispatcher中了解常见的日志条目，并了解它们的含义以及如何解决它们。
version: 6.5
topic: Administration, Performance
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
doc-type: Article
exl-id: 7fe1b4a5-6813-4ece-b3da-40af575ea0ed
duration: 252
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '799'
ht-degree: 0%

---

# 通用日志

[目录](./overview.md)

[&lt; — 上一页：虚URL](./disp-vanity-url.md)

## 概述

本文档将描述您将看到的常见日志条目，以及它们的含义以及如何处理它们。

## GLOB警告

示例日志条目：

```
Fri Jul 20 03:35:09 2018 W pid 8300 (tid 139937910880384) /etc/httpd/conf/publish-filters.any:5: Allowing requests with globs is considered unsafe.
Please consult the documentation at 'https://www.adobe.com/go/docs_dispatcher_config_en' on how to use attributes 
method/url/query/protocol/path/selectors/extension/suffix instead.
```

自Dispatcher模块4.2.x起，他们开始阻止人员在其过滤器文件中使用以下类型的匹配：

```
/0041 { /type "allow" /glob "* *.css *"   }
```

最好改用新语法，如：

```
/0041 { /type "allow" /url "*.css" }
```

或者，最好根本不通过通配符匹配器进行匹配：

```
/0041 { /type "allow" /extension "css" }
```

执行任何一个建议的方法都将从日志中删除该错误消息。

## 过滤器拒绝


<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>注意：</b>
如果日志级别设置得过低，即使发生拒绝时，也不会始终显示这些条目。 将其设置为“信息”或“调试”，以确保您能够查看过滤器是否拒绝请求。
</div>

示例日志条目：

```
Fri Jul 20 17:25:48 2018 D pid 25939 (tid 139937517123328) Filter rejects: GET /libs/granite/core/content/login.html HTTP/1.1
```

或：

```
Fri Jul 20 22:16:55 2018 I pid 128803 "GET /system/console/" ! - 8ms publishfarm/-
```

<div style="color: #000;border-left: 6px solid red;background-color:#ddffff;"><b>注意：</b>

了解Dispatcher的规则已设置为过滤掉该请求。 在这种情况下，尝试访问的页面会被有意拒绝，我们对此不必执行任何操作。
</div>

如果您的日志类似于以下条目：

```
Fri Jul 20 17:26:47 2018 D pid 20051 (tid 139937517123328) Filter rejects: 
GET /etc/designs/exampleco/fonts/montserrat-regular/montserrat-regular-webfont.eot HTTP/1.1
```

这让我们知道我们的设计文件 `.eot` 已被阻止，我们将需要修复此问题。
因此，我们应该查看过滤器文件并添加以下行以允许 `.eot` 文件通过

```
/0011 { /type "allow" /method "GET" /extension 'eot' /path "/etc/designs/*" }
```

这将允许文件通过并停止记录。
如果要查看过滤掉的内容，可以对日志文件运行以下命令：

```
$ grep "Filter rejects\|\!" /var/log/httpd/dispatcher.log | awk 'match($0, /\/.*\//, m){ print m0 }' | awk '{ print $1 }'| sort | uniq -c | sort -rn
```

## 渲染超时

套接字超时示例日志条目：

```
Fri Jul 20 22:31:15 2018 W pid 3648 Unable to connect socket to 10.43.3.40:4502: Connection timed out 
Fri Jul 20 22:31:15 2018 W pid 3648 Unable to connect to any backend in farm authorfarm
```

当您在场的渲染部分配置了错误的IP地址时，会发生这种情况。 该AEM实例停止响应或侦听，Dispatcher无法访问。

检查防火墙规则，确保AEM实例正在运行且运行正常。

网关超时示例日志条目：

```
Fri Jul 20 22:32:42 2018 I pid 3648 "GET /favicon.ico" 502 - 54034ms authorfarm/- 
Fri Jul 20 22:35:45 2018 I pid 3648 "GET /favicon.ico" 503 - 54234ms authorfarm/-
```

这意味着AEM实例具有一个打开的套接字，它可以通过响应访问并超时。 这意味着您的AEM实例运行过慢或不正常，Dispatcher已在场的渲染部分中访问其配置的超时设置。 增加超时设置或使AEM实例正常。

## 缓存级别

示例日志条目：

```
Fri Jul 20 23:00:19 2018 I pid 16004 (tid 140134145820416) Current cache hit ratio: 87.94 %
```

这意味着将测量您从渲染级别和从缓存中提取情况。 如果希望从缓存中达到80%以上，您应该按照帮助进行操作 [此处](https://experienceleague.adobe.com/docs/experience-cloud-kcs/kbarticles/KA-17458.html%3Flang%3Den)：

使这一数字尽可能高。

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>注意：</b>
即使您在场文件中将缓存设置为缓存所有内容，您也可能过于频繁或过于激进地刷新，这可能导致发生缓存命中率的百分比降低
</div>

## 缺少目录

示例日志条目：

```
Fri Jul 20 14:02:43 2018 E pid 4728 (tid 140662586435328) Unable to create parent directory /mnt/var/www/author/libs/dam/content/asseteditors/formitems.overlay.infinity.json/application: Not a directory
```

通常，如果在提取项目的同时执行缓存清除，则会显示此日志。

该目录或文档根目录的权限不正确，或者文件夹上的SELinux文件上下文错误，拒绝Apache创建所需的新子目录来进行缓存。

有关权限问题，请查看documentroot的权限，确保其内容与以下内容类似：

```
dispatcher$ ls -Z /var/www/
drwxr-xr-x+ apache apache system_u:object_r:httpd_sys_content_t:s0 html
```

## 未找到虚URL

示例日志条目：

```
Thu Sep 27 17:35:11 2018 D pid 18936 Checking vanity URLs 
Thu Sep 27 17:35:11 2018 D pid 18936 Vanity URL file (/tmp/vanity_urls) not found, fetching... 
Thu Sep 27 17:35:11 2018 W pid 18936 Unable to fetch vanity URLs from 10.43.0.42:4503/libs/granite/dispatcher/content/vanityUrls.html: remote server returned: HTTP/1.1 404 Not Found
```

当您将调度程序配置为使用动态自动过滤器允许虚URL，但未通过在AEM渲染器上安装包来完成设置时，会发生此错误。

要修复此问题，请在AEM实例上安装虚URL功能包，并允许匿名用户随时使用。 详细信息 [此处](https://experienceleague.adobe.com/docs/experience-cloud-kcs/kbarticles/KA-17463.html%3Flang%3Den)

设置的工作虚URL如下所示：

```
Thu Sep 27 17:40:29 2018 D pid 21844 Checking vanity URLs 
Thu Sep 27 17:40:29 2018 D pid 21844 Vanity URL file (/tmp/vanity_urls) not found, fetching... 
Thu Sep 27 17:40:29 2018 D pid 21844 Loaded 18 vanity URLs from file /tmp/vanity_urls
```

## 缺少场

示例日志条目：

```
Wed Nov 13 17:17:26 2019 W pid 19173:tid 140542738364160 No farm matches host 'we-retail.com', selected last farm 'publishfarm'
```

此错误表示从所有场文件 `/etc/httpd/conf.dispatcher.d/enabled_farms/` 他们无法从以下位置找到匹配的条目： `/virtualhost` 部分。

场文件根据请求所包含的域名或路径匹配流量。 它使用glob匹配，如果不匹配，则说明您未正确配置场、键入场中的条目有拼写错误，或完全缺失该条目。 当场与任何条目不匹配时，它最终只默认为场文件栈栈中包含的最后一个场。 在此示例中，它是 `999_ams_publish_farm.any` 其名称为publishfarm的通用名称。

以下是示例场文件 `/etc/httpd/conf.dispatcher.d/enabled_farms/300_weretail_publish_farm.any` 那个被缩小以突出显示相关的部分。

## 提供的项目

示例日志条目：

```
Tue Nov 26 16:41:34 2019 I pid 9208 (tid 140112092391168) "GET /content/we-retail/us/en.html" - + 24034ms publishfarm/0
```

已通过内容的GEThttp方法获取页面 `/content/we-retail/us/en.html` 花了24034毫秒。 最后我们要注意的部分 `publishfarm/0`. 您将看到它定位并匹配 `publishfarm`. 从渲染0中提取请求。 这意味着必须从AEM请求此页面，然后缓存该页面。 现在，让我们再次请求此页面，并查看日志的情况。

[下一个 — >只读文件](./immutable-files.md)
