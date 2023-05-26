---
title: AMS Dispatcher只读或不可变文件
description: 了解为什么某些文件是只读的或不可编辑的，以及如何进行所需的功能更改
version: 6.5
topic: Administration, Development
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
exl-id: 7be6b3f9-cd53-41bc-918d-5ab9b633ffb3
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '826'
ht-degree: 1%

---

# AMS中的只读或不可变文件

[目录](./overview.md)

[&lt; — 上一页：常用日志](./common-logs.md)

## 描述

本文档将介绍哪些文件被锁定且不能更改，以及如何正确设置所需的配置。

当AMS设置系统时，会推出基线配置，使所有功能都能够正常运行并且安全。  AMS希望确保将这些内容作为功能和安全性的基准。  要完成此操作，某些文件将标记为只读和不可变，以避免您更改它们。

布局不会阻止您更改其行为和覆盖您需要的任何更改。  您无需更改这些文件，而是将覆盖您自己的文件，以取代原始文件。

这还可以确保当AMS使用最新的修复和安全增强为Dispatcher修补补丁时，不会更改您的文件。  然后，您可以继续从这些改进中受益，并仅采用所需的更改。
![显示保龄球在球道上滚动。  球上有一个箭头，上面写着显示你的字。  水槽缓冲器升起，上面有不可变文件。](assets/immutable-files/bowling-file-immutability.png "bowling-file-immutability")
如上图所示，不可变文件不会阻止您玩游戏。  它们只会防止你损害自己的表现，让你不脱离正轨。  此方法允许我们使用以下几个非常关键的功能：

- 自定义项在其自身的安全空间中处理
- 自定义更改的覆盖反映了AEM中的覆盖方法
- 无需更改自定义设置，即可对AMS配置进行修补
- 测试基本安装与自定义配置可以同时完成，以帮助确定问题是由自定义还是其他原因引起。哪些文件？


以下是与Dispatcher一起部署的典型文件列表：

```
/etc/httpd/
├── conf
│   ├── httpd.conf
│   └── magic
├── conf.d
│   ├── 000_init_ootb_vars.conf
│   ├── 001_init_ams_vars.conf
│   ├── README
│   ├── autoindex.conf
│   ├── available_vhosts
│   │   ├── 000_unhealthy_author.vhost
│   │   ├── 000_unhealthy_publish.vhost
│   │   ├── aem_author.vhost
│   │   ├── aem_flush.vhost
│   │   ├── aem_flush_author.vhost
│   │   ├── aem_health.vhost
│   │   ├── aem_publish.vhost
│   │   └── ams_lc.vhost
│   ├── dispatcher_vhost.conf
│   ├── enabled_vhosts
│   │   ├── aem_author.vhost -> ../available_vhosts/aem_author.vhost
│   │   ├── aem_flush.vhost -> ../available_vhosts/aem_flush.vhost
│   │   ├── aem_health.vhost -> /etc/httpd/conf.d/available_vhosts/aem_health.vhost
│   │   └── aem_publish.vhost -> ../available_vhosts/aem_publish.vhost
│   ├── logformat.conf
│   ├── mimetypes3d.conf
│   ├── remoteip.conf
│   ├── rewrites
│   │   ├── base_rewrite.rules
│   │   └── xforwarded_forcessl_rewrite.rules
│   ├── security.conf
│   ├── userdir.conf
│   ├── variables
│   │   ├── ams_default.vars
│   │   └── ootb.vars
│   ├── welcome.conf
│   └── whitelists
│       └── 000_base_whitelist.rules
├── conf.dispatcher.d
│   ├── available_farms
│   │   ├── 000_ams_catchall_farm.any
│   │   ├── 001_ams_author_flush_farm.any
│   │   ├── 001_ams_publish_flush_farm.any
│   │   ├── 002_ams_author_farm.any
│   │   ├── 002_ams_lc_farm.any
│   │   └── 002_ams_publish_farm.any
│   ├── cache
│   │   ├── ams_author_cache.any
│   │   ├── ams_author_invalidate_allowed.any
│   │   ├── ams_publish_cache.any
│   │   └── ams_publish_invalidate_allowed.any
│   ├── clientheaders
│   │   ├── ams_author_clientheaders.any
│   │   ├── ams_common_clientheaders.any
│   │   ├── ams_lc_clientheaders.any
│   │   └── ams_publish_clientheaders.any
│   ├── dispatcher.any
│   ├── enabled_farms
│   │   ├── 000_ams_catchall_farm.any -> ../available_farms/000_ams_catchall_farm.any
│   │   ├── 001_ams_author_flush_farm.any -> ../available_farms/001_ams_author_flush_farm.any
│   │   ├── 001_ams_publish_flush_farm.any -> ../available_farms/001_ams_publish_flush_farm.any
│   │   ├── 002_ams_author_farm.any -> ../available_farms/002_ams_author_farm.any
│   │   └── 002_ams_publish_farm.any -> ../available_farms/002_ams_publish_farm.any
│   ├── filters
│   │   ├── ams_author_filters.any
│   │   ├── ams_lc_filters.any
│   │   └── ams_publish_filters.any
│   ├── renders
│   │   ├── ams_author_renders.any
│   │   ├── ams_lc_renders.any
│   │   └── ams_publish_renders.any
│   └── vhosts
│       ├── ams_author_vhosts.any
│       ├── ams_lc_vhosts.any
│       └── ams_publish_vhosts.any
├── conf.modules.d
│   ├── 01-cgi.conf
│   └── 02-dispatcher.conf
└── modules - ../../usr/lib64/httpd/modules
    └── mod_dispatcher.so
```

要确定哪些文件不可变，您可以在Dispatcher上运行以下命令以查看：

```
$ lsattr -Rl /etc/httpd 2>/dev/null | grep Immutable
```

以下是不可更改文件的示例响应：

```
/etc/httpd/conf/httpd.conf   Immutable
/etc/httpd/conf.d/available_vhosts/aem_author.vhost Immutable
/etc/httpd/conf.d/available_vhosts/aem_publish.vhost Immutable
/etc/httpd/conf.d/available_vhosts/aem_flush.vhost Immutable
/etc/httpd/conf.d/available_vhosts/aem_health.vhost Immutable
/etc/httpd/conf.d/available_vhosts/000_unhealthy_author.vhost Immutable
/etc/httpd/conf.d/available_vhosts/000_unhealthy_publish.vhost Immutable
/etc/httpd/conf.d/available_vhosts/aem_flush_author.vhost Immutable
/etc/httpd/conf.d/available_vhosts/ams_lc.vhost Immutable
/etc/httpd/conf.d/rewrites/base_rewrite.rules Immutable
/etc/httpd/conf.d/rewrites/xforwarded_forcessl_rewrite.rules Immutable
/etc/httpd/conf.d/whitelists/000_base_whitelist.rules Immutable
/etc/httpd/conf.d/variables/ootb.vars Immutable
/etc/httpd/conf.d/dispatcher_vhost.conf Immutable
/etc/httpd/conf.d/logformat.conf Immutable
/etc/httpd/conf.d/security.conf Immutable
/etc/httpd/conf.d/mimetypes3d.conf Immutable
/etc/httpd/conf.d/remoteip.conf Immutable
/etc/httpd/conf.d/000_init_ootb_vars.conf Immutable
/etc/httpd/conf.d/001_init_ams_vars.conf Immutable
/etc/httpd/conf.modules.d/02-dispatcher.conf Immutable
/etc/httpd/conf.dispatcher.d/available_farms/000_ams_catchall_farm.any Immutable
/etc/httpd/conf.dispatcher.d/available_farms/001_ams_author_flush_farm.any Immutable
/etc/httpd/conf.dispatcher.d/available_farms/001_ams_publish_flush_farm.any Immutable
/etc/httpd/conf.dispatcher.d/available_farms/002_ams_author_farm.any Immutable
/etc/httpd/conf.dispatcher.d/available_farms/002_ams_lc_farm.any Immutable
/etc/httpd/conf.dispatcher.d/available_farms/002_ams_publish_farm.any Immutable
/etc/httpd/conf.dispatcher.d/cache/ams_author_cache.any Immutable
/etc/httpd/conf.dispatcher.d/cache/ams_author_invalidate_allowed.any Immutable
/etc/httpd/conf.dispatcher.d/cache/ams_publish_cache.any Immutable
/etc/httpd/conf.dispatcher.d/cache/ams_publish_invalidate_allowed.any Immutable
/etc/httpd/conf.dispatcher.d/clientheaders/ams_author_clientheaders.any Immutable
/etc/httpd/conf.dispatcher.d/clientheaders/ams_publish_clientheaders.any Immutable
/etc/httpd/conf.dispatcher.d/clientheaders/ams_common_clientheaders.any Immutable
/etc/httpd/conf.dispatcher.d/clientheaders/ams_lc_clientheaders.any Immutable
/etc/httpd/conf.dispatcher.d/filters/ams_author_filters.any Immutable
/etc/httpd/conf.dispatcher.d/filters/ams_publish_filters.any Immutable
/etc/httpd/conf.dispatcher.d/filters/ams_lc_filters.any Immutable
/etc/httpd/conf.dispatcher.d/renders/ams_author_renders.any Immutable
/etc/httpd/conf.dispatcher.d/renders/ams_lc_renders.any Immutable
/etc/httpd/conf.dispatcher.d/renders/ams_publish_renders.any Immutable
/etc/httpd/conf.dispatcher.d/vhosts/ams_author_vhosts.any Immutable
/etc/httpd/conf.dispatcher.d/vhosts/ams_publish_vhosts.any Immutable
/etc/httpd/conf.dispatcher.d/vhosts/ams_lc_vhosts.any Immutable
/etc/httpd/conf.dispatcher.d/dispatcher.any Immutable
```

## 如何进行更改

### 变量

变量允许您在不更改配置文件本身的情况下进行功能更改。  可以通过调整变量的值来调整某些配置元素。  举个例子，我们可以从文件中突出显示 `/etc/httpd/conf.d/dispatcher_vhost.conf` 如下所示：

```
Include /etc/httpd/conf.d/variables/ams_default.vars
IfModule disp_apache2.c
    DispatcherConfig conf.dispatcher.d/dispatcher.any
    DispatcherLog    logs/dispatcher.log
    DispatcherLogLevel ${DISP_LOG_LEVEL}
    DispatcherDeclineRoot 0
    DispatcherUseProcessedURL 1
/IfModule
```

了解DispatcherLogLevel指令如何将 `DISP_LOG_LEVEL` 而不是你看到的正常值。  在该部分代码的上方，您还将看到变量文件的include语句。  变量文件 `/etc/httpd/conf.d/variables/ams_default.vars` 是我们接下来要看的地方。  以下是该variables文件的内容：

```
Define DISP_LOG_LEVEL info
Define AUTHOR_WHITELIST_ENABLED 0
Define PUBLISH_WHITELIST_ENABLED 0
Define LIVECYCLE_WHITELIST_ENABLED 0
Define AUTHOR_FORCE_SSL 1
Define PUBLISH_FORCE_SSL 0
Define LIVECYCLE_FORCE_SSL 1
```

您在上面看到的当前值 `DISP_LOG_LEVEL` 变量为 `info`.  我们可以将其调整为trace或debug，或者您选择的数值/级别。  现在，控制日志级别的所有位置都将自动调整。

### 叠加方法

请了解顶层include文件，因为这将是您进行任何自定义的起点。  首先，举个简单的例子，我们希望在场景中添加一个新域名，并指向此Dispatcher。  我们将使用的域示例为we-retail.adobe.com。  我们首先会将现有配置文件复制到新的配置文件中，以便在其中添加更改：

```
$ cp /etc/httpd/conf.d/available_vhosts/aem_publish.vhost /etc/httpd/conf.d/available_vhosts/weretail_publish.vhost
```

我们复制了现有的aem_publish.vhost文件，因为它已具备了我们需要的内容，没有必要从头开始。  现在，我们编辑新的weretail.vhost文件，并进行所需的更改。

之前:

```
VirtualHost *:80
    ServerName    publish
    ServerAlias    ${PUBLISH_DEFAULT_HOSTNAME}
    DocumentRoot    ${PUBLISH_DOCROOT}
    IfModule mod_headers.c
            Header always add X-Dispatcher ${DISP_ID}
            Header always add X-Vhost "publish"
            Header merge X-Frame-Options SAMEORIGIN "expr=%{resp:X-Frame-Options}!='SAMEORIGIN'"
            Header merge X-Content-Type-Options nosniff "expr=%{resp:X-Content-Type-Options}!='nosniff'"
        Header append Vary User-Agent env=!dont-vary
    /IfModule
....... SNIP.......
/VirtualHost
```

之后:

```
VirtualHost *:80
    ServerName    weretail-publish
    ServerAlias    we-retail.adobe.com
    DocumentRoot    ${PUBLISH_DOCROOT}
    IfModule mod_headers.c
            Header always add X-Dispatcher ${DISP_ID}
            Header always add X-Vhost "werteail-publish"
            Header merge X-Frame-Options SAMEORIGIN "expr=%{resp:X-Frame-Options}!='SAMEORIGIN'"
            Header merge X-Content-Type-Options nosniff "expr=%{resp:X-Content-Type-Options}!='nosniff'"
        Header append Vary User-Agent env=!dont-vary
    /IfModule
....... SNIP.......
/VirtualHost
```

现在，我们更新了 `ServerName` 和 `ServerAlias` 以匹配新域名，并更新其他痕迹导航标头。  现在，让我们启用新文件，以允许Apache知道要使用新文件：

```
$ cd /etc/httpd/conf.d/enabled_vhosts/; ln -s ../available_vhosts/weretail_publish.vhost .
```

现在，Apache Webserver知道该域会产生流量，但我们仍需要通知Dispatcher模块它有一个新的域名需要遵守。  我们首先要创建一个新的 `*_vhost.any` 文件 `/etc/httpd/conf.dispatcher.d/vhosts/weretail_vhosts.any` 在该文件中，我们将输入想要遵循的域名：

```
"we-retail.adobe.com"
```

现在，我们需要创建一个新的场文件，该文件将使用新的vhost条目文件，我们首先会将一个强启动文件复制到我们自己的新文件中。

```
$ cp /etc/httpd/conf.dispatcher.d/available_farms/999_ams_publish_farm.any /etc/httpd/conf.dispatcher.d/available_farms/400_weretail_publish_farm.any
```

让我们显示我们需要对此场文件进行的更改

之前:

```
/publishfarm {  
    /virtualhosts {
        $include "/etc/httpd/conf.dispatcher.d/vhosts/ams_publish_vhosts.any"
    }
........SNIP.........
}
```

之后:

```
/weretailpublishfarm {  
    /virtualhosts {
        $include "/etc/httpd/conf.dispatcher.d/vhosts/weretail_publish_vhosts.any"
    }
........SNIP.........
}
```

现在，我们更新了场名称，以及它在 `/virtualhosts` 场配置的部分。  我们需要启用此新场文件，以便它可以在运行配置中使用：

```
$ cd /etc/httpd/conf.dispatcher.d/enabled_farms/; ln -s ../available_farms/400_weretail_publish_farm.any .
```

现在，我们只需重新加载Web服务器服务并使用新域即可！

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>注意:</b>

请注意，我们仅更改了需要更改的片段，并利用了基线配置文件附带的现有包含和代码。  我们只需勾勒出需要改变的元素。  使操作更加简单，并且允许我们维护更少的代码
</div>

[下一步 — > Dispatcher运行状况检查](./health-check.md)
