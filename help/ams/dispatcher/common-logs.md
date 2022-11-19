---
title: AEM Dispatcher常见日志
description: 查看Dispatcher中的常见日志条目，了解它们的含义以及如何解决它们。
version: 6.5
topic: Administration, Performance
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
source-git-commit: 04cd4002af7028ee9e3b1e1455b6346c56446245
workflow-type: tm+mt
source-wordcount: '812'
ht-degree: 0%

---


# 常用日志

[目录](./overview.md)

[&lt; — 上一个：虚URL](./disp-vanity-url.md)

## 概述

文档将描述您将看到的常见日志条目及其含义和处理方法。

## GLOB警告

示例日志条目：

```
Fri Jul 20 03:35:09 2018 W pid 8300 (tid 139937910880384) /etc/httpd/conf/publish-filters.any:5: Allowing requests with globs is considered unsafe.
Please consult the documentation at 'https://www.adobe.com/go/docs_dispatcher_config_en' on how to use attributes 
method/url/query/protocol/path/selectors/extension/suffix instead.
```

自Dispatcher模块4.2.x起，他们开始阻止用户在其筛选器文件中使用以下类型的匹配：

```
/0041 { /type "allow" /glob "* *.css *"   }
```

相反，最好使用新语法，例如：

```
/0041 { /type "allow" /url "*.css" }
```

或者最好不要在通配符匹配程序上进行匹配：

```
/0041 { /type "allow" /extension "css" }
```

执行任一建议的方法都将从日志中删除该错误消息。

## 筛选拒绝


<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>注意：</b>
即使在日志级别设置过低的情况下发生拒绝，这些条目也不会始终显示。 将其设置为“信息”或“调试”，以确保您能够查看过滤器是否拒绝了请求。
</div>

示例日志条目：

```
Fri Jul 20 17:25:48 2018 D pid 25939 (tid 139937517123328) Filter rejects: GET /libs/granite/core/content/login.html HTTP/1.1
```

或:

```
Fri Jul 20 22:16:55 2018 I pid 128803 "GET /system/console/" ! - 8ms publishfarm/-
```

<div style="color: #000;border-left: 6px solid red;background-color:#ddffff;"><b>注意:</b>

了解Dispatcher的规则已设置为过滤该请求。 在这种情况下，尝试访问的页面被有意拒绝，我们不想对此执行任何操作。
</div>

如果您的日志类似于以下条目：

```
Fri Jul 20 17:26:47 2018 D pid 20051 (tid 139937517123328) Filter rejects: 
GET /etc/designs/exampleco/fonts/montserrat-regular/montserrat-regular-webfont.eot HTTP/1.1
```

这让我们知道我们的设计文件 `.eot` 正被阻止，我们将想要解决这个问题。
因此，我们应该查看过滤器文件，并添加以下行以允许 `.eot` 文件通过

```
/0011 { /type "allow" /method "GET" /extension 'eot' /path "/etc/designs/*" }
```

这将允许文件通过并阻止其记录。
如果要查看正在过滤的内容，可以在日志文件中运行以下命令：

```
$ grep "Filter rejects\|\!" /var/log/httpd/dispatcher.log | awk 'match($0, /\/.*\//, m){ print m0 }' | awk '{ print $1 }'| sort | uniq -c | sort -rn
```

## 渲染超时

套接字超时示例日志条目：

```
Fri Jul 20 22:31:15 2018 W pid 3648 Unable to connect socket to 10.43.3.40:4502: Connection timed out 
Fri Jul 20 22:31:15 2018 W pid 3648 Unable to connect to any backend in farm authorfarm
```

当您在场的renders部分中配置了错误的IP地址时，会发生这种情况。 该或AEM实例已停止响应或侦听，且Dispatcher无法访问它。

检查您的防火墙规则，以及AEM实例是否正在运行且运行正常。

网关超时示例日志条目：

```
Fri Jul 20 22:32:42 2018 I pid 3648 "GET /favicon.ico" 502 - 54034ms authorfarm/- 
Fri Jul 20 22:35:45 2018 I pid 3648 "GET /favicon.ico" 503 - 54234ms authorfarm/-
```

这意味着AEM实例具有一个打开的套接字，该套接字可能会随响应而到达并超时。 这表示您的AEM实例运行过慢或运行不正常，并且Dispatcher已在场的呈现部分中达到其配置的超时设置。 增加超时设置或使AEM实例正常。

## 缓存级别

示例日志条目：

```
Fri Jul 20 23:00:19 2018 I pid 16004 (tid 140134145820416) Current cache hit ratio: 87.94 %
```

这意味着测量从渲染级别与从缓存获取的数据。 您希望从缓存中达到80%以上，并应按照帮助 [此处](https://experienceleague.adobe.com/docs/experience-cloud-kcs/kbarticles/KA-17458.html%3Flang%3Den):

以尽可能高地获得此数字。

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>注意：</b>
即使您在场文件中设置了缓存设置来缓存所有可能刷新的次数过多或过于频繁，可能会导致缓存命中率降低
</div>

## 缺少目录

示例日志条目：

```
Fri Jul 20 14:02:43 2018 E pid 4728 (tid 140662586435328) Unable to create parent directory /mnt/var/www/author/libs/dam/content/asseteditors/formitems.overlay.infinity.json/application: Not a directory
```

通常，当在获取项目的同时执行缓存清除时，会显示该内容。

该或文档根目录对它的权限不正确，或者文件夹上的SELinux文件上下文错误，该文件上下文拒绝Apache创建新的所需子目录。

有关权限问题，请查看documentroot的权限，并确保它们类似于：

```
dispatcher$ ls -Z /var/www/
drwxr-xr-x+ apache apache system_u:object_r:httpd_sys_content_t:s0 html
```

## 找不到虚URL

示例日志条目：

```
Thu Sep 27 17:35:11 2018 D pid 18936 Checking vanity URLs 
Thu Sep 27 17:35:11 2018 D pid 18936 Vanity URL file (/tmp/vanity_urls) not found, fetching... 
Thu Sep 27 17:35:11 2018 W pid 18936 Unable to fetch vanity URLs from 10.43.0.42:4503/libs/granite/dispatcher/content/vanityUrls.html: remote server returned: HTTP/1.1 404 Not Found
```

当您将Dispatcher配置为使用动态自动过滤允许虚URL，但未通过在AEM渲染器上安装包来完成设置时，会发生此错误。

要修复此问题，请在AEM实例上安装虚URL功能包，并允许匿名用户为其做好准备。 详细信息 [此处](https://experienceleague.adobe.com/docs/experience-cloud-kcs/kbarticles/KA-17463.html%3Flang%3Den)

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

此错误表示在 `/etc/httpd/conf.dispatcher.d/enabled_farms/` 他们无法从 `/virtualhost` 中。

场文件根据请求传入的域名或路径匹配流量。 它使用全局匹配，如果它不匹配，则表示您未正确配置场、键入场中的条目，或者完全缺少该条目。 当场与任何条目不匹配时，它最终将默认为所包含场文件堆栈中包含的最后一个场。 在本例中是 `999_ams_publish_farm.any` 名称为publishfarm的通用名称。

以下是场文件示例 `/etc/httpd/conf.dispatcher.d/enabled_farms/300_weretail_publish_farm.any` 这个部分被缩小以突出相关部分。

## 提供的物料来源

示例日志条目：

```
Tue Nov 26 16:41:34 2019 I pid 9208 (tid 140112092391168) "GET /content/we-retail/us/en.html" - + 24034ms publishfarm/0
```

通过GEThttp方法获取内容 `/content/we-retail/us/en.html` 用了24034毫秒。 我们最后要关注的是 `publishfarm/0`. 您会看到它已定位并与 `publishfarm`. 已从呈现0中获取请求。 这意味着必须先从AEM请求此页面，然后才能缓存。 现在，让我们再次请求此页，并查看日志会发生什么情况。

[下一个 — >只读文件](./immutable-files.md)