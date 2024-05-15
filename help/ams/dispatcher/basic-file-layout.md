---
title: AMS Dispatcher基本文件布局
description: 了解基本Apache和Dispatcher文件布局。
version: 6.5
topic: Administration, Development
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
doc-type: Article
exl-id: 8a3f2bb9-3895-45c6-8bb5-15a6d2aac50e
duration: 308
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1130'
ht-degree: 0%

---

# 基本文件布局

[目录](./overview.md)

[&lt; — 上一页：“调度程序”是什么](./what-is-the-dispatcher.md)

本文档介绍AMS标准配置文件集以及此配置标准背后的思路

## 默认Enterprise Linux文件夹结构

在AMS中，基本安装使用Enterprise Linux作为基本操作系统。 安装Apache Webserver时，它有一个默认安装文件集。 以下是通过安装yum资料档案库提供的基本RPM而安装的默认文件

```
/etc/httpd/ 
├── conf 
│   ├── httpd.conf 
│   └── magic 
├── conf.d 
│   ├── autoindex.conf 
│   ├── README 
│   ├── userdir.conf 
│   └── welcome.conf 
├── conf.modules.d 
│   ├── 00-base.conf 
│   ├── 00-dav.conf 
│   ├── 00-lua.conf 
│   ├── 00-mpm.conf 
│   ├── 00-proxy.conf 
│   ├── 00-systemd.conf 
│   └── 01-cgi.conf 
├── logs -> ../../var/log/httpd 
├── modules -> ../../usr/lib64/httpd/modules 
└── run -> /run/httpd
```

当遵循并遵循安装设计/结构时，我们将获得以下好处：

- 更容易支持可预测的布局
- 以前在Enterprise Linux HTTPD安装中工作过的人都会自动熟悉
- 允许修补完全受操作系统支持的周期，不会出现任何冲突或手动调整
- 避免错误标记的文件上下文的SELinux违规

>[!BEGINSHADEBOX &quot;Note&quot;]

AdobeManaged Services服务器映像通常具有小型操作系统根驱动器。  我们将数据放入一个单独的卷中，该卷通常装载在 `/mnt`
然后，我们将使用该卷而不是下列默认目录的默认值

`DocumentRoot`
- 默认：`/var/www/html`
- AMS：`/mnt/var/www/html`

`Log Directory`
- 默认： `/var/log/httpd`
- AMS： `/mnt/var/log/httpd`

请记住，旧目录和新目录将映射回原始装载点，以消除混淆。
使用单独卷并不重要，但值得注意

>[!ENDSHADEBOX]

## AMS插件

AMS将添加到Apache Web Server的基本安装中。

### 文档根

AMS默认文档根：
- 作者：
   - `/mnt/var/www/author/`
- 发布：
   - `/mnt/var/www/html/`
- 全面覆盖和健康检查维护
   - `/mnt/var/www/default/`

### 暂存和启用的虚拟主机目录

以下目录允许您构建配置文件，这些文件具有暂存区，您可以处理文件，并且只能在文件准备就绪时启用。
- `/etc/httpd/conf.d/available_vhosts/`
   - 此文件夹托管所有名为的VirtualHost /文件 `.vhost`
- `/etc/httpd/conf.d/enabled_vhosts/`
   - 当您准备好使用 `.vhost` 文件，您拥有 `available_vhosts` 使用相对路径将它们与文件夹符号链接 `enabled_vhosts` 目录

### 其他 `conf.d` 目录

还有一些在Apache配置中常见的片段，我们创建了子目录，以便采用简洁的方式分隔这些文件，而不是将所有文件放在一个目录中

#### 重写目录

此目录可以包含所有 `_rewrite.rules` 您创建的包含与Apache Web服务器相连的典型RewriteRulesyntax的文件 [mod_rewrite](https://httpd.apache.org/docs/current/mod/mod_rewrite.html) 模块

- `/etc/httpd/conf.d/rewrites/`

#### 白名单目录

此目录可以包含所有 `_whitelist.rules` 您创建的包含您的典型 `IP Allow` 或 `Require IP`与Apache Web Server接触的语法 [访问控制](https://httpd.apache.org/docs/2.4/howto/access.html)

- `/etc/httpd/conf.d/whitelists/`

#### 变量目录

此目录可以包含所有 `.vars` 您创建的包含可在配置文件中使用的变量的文件

- `/etc/httpd/conf.d/variables/`

### 特定于Dispatcher模块的配置目录

Apache Web Server极具可扩展性，当模块具有大量配置文件时，最佳做法是在安装基目录下创建自己的配置目录，而不是将默认目录乱七八糟。

我们遵循最佳实践，创建自己的

#### 模块配置文件目录

- `/etc/httpd/conf.dispatcher.d/`

#### 暂存和已启用的场

以下目录允许您构建配置文件，这些文件具有暂存区，您可以处理文件，并且只能在文件准备就绪时启用。
- `/etc/httpd/conf.dispatcher.d/available_farms/`
   - 此文件夹托管您的所有 `/myfarm {` 已调用的文件 `_farm.any`
- `/etc/httpd/conf.dispatcher.d/enabled_farms/`
   - 当您准备好使用场文件时，您可以在available_farms文件夹内使用enabled_farms目录的相对路径将它们与符号链接

### 其他 `conf.dispatcher.d` 目录

还有一些其他片段是Dispatcher场文件配置的子部分，我们创建了子目录，以便以一种简洁的方式分隔这些文件，而不是将所有文件放在一个目录中

#### 缓存目录

此目录包含所有 `_cache.any`， `_invalidate.any` 您创建的文件，其中包含您希望模块如何处理来自AEM的缓存元素以及失效规则语法的规则。  有关本部分的更多详细信息，请参阅此处 [此处](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#configuring-the-dispatcher-cache-cache)

- `/etc/httpd/conf.dispatcher.d/cache/`

#### 客户端标头目录

此目录可以包含所有 `_clientheaders.any` 您创建的文件，其中包含您希望在请求传入时传递到AEM的客户端标头列表。  有关本部分的更多详细信息包括 [此处](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=zh-Hans)

- `/etc/httpd/conf.dispatcher.d/clientheaders/`

#### 筛选器目录

此目录可以包含所有 `_filters.any` 您创建的文件，其中包含要阻止或允许流量通过Dispatcher到达AEM的所有筛选规则

- `/etc/httpd/conf.dispatcher.d/filters/`

#### 渲染目录

此目录可以包含所有 `_renders.any` 您创建的文件，其中包含到Dispatcher将从中使用内容的每个后端服务器的连接详细信息

- `/etc/httpd/conf.dispatcher.d/renders/`

#### Vhosts目录

此目录可以包含所有 `_vhosts.any` 您创建的文件，其中包含要与特定场匹配到特定后端服务器的域名和路径列表

- `/etc/httpd/conf.dispatcher.d/vhosts/`

## 完成文件夹结构

AMS已使用自定义文件扩展名构建每个文件，旨在避免命名空间问题/冲突及任何混淆。

以下是AMS默认部署中的标准文件集示例：

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
│   ├── 00-base.conf
│   ├── 00-dav.conf
│   ├── 00-lua.conf
│   ├── 00-mpm.conf
│   ├── 00-mpm.conf.old
│   ├── 00-proxy.conf
│   ├── 00-systemd.conf
│   ├── 01-cgi.conf
│   └── 02-dispatcher.conf
├── logs -> ../../var/log/httpd
├── modules -> ../../usr/lib64/httpd/modules
└── run -> /run/httpd
```

## 保持理想状态

Enterprise Linux具有Apache Webserver包(httpd)的打补丁周期。

安装的默认文件越少，更改就越好，原因在于，如果通过RPM / Yum命令应用了任何修补的安全修复或配置改进，则不会将修复应用到已更改文件的顶部。

相反，它会创建 `.rpmnew` 文件（在原始文件旁边）。  这意味着您将丢失一些您可能想要的更改，并在配置文件夹中创建更多垃圾。

即，在更新安装过程中，RPM将查看 `httpd.conf` 如果它位于 `unaltered` 声明它将 *replace* 文件将得到重要更新。  如果 `httpd.conf` 是 `altered` 那么它 *不会替换* 文件，而是将创建一个名为的引用文件 `httpd.conf.rpmnew` 并且许多所需的修补程序将位于该文件中，不适用于服务启动。

Enterprise Linux已正确设置，可更好地处理此用例。  它们为您提供了可以扩展或覆盖它们为您设置的默认值的区域。  在httpd的基本安装中，您将找到文件 `/etc/httpd/conf/httpd.conf`，并且其中具有以下语法：

```
Include conf.modules.d/.conf
IncludeOptional conf.d/.conf
```

其想法是，Apache希望您在向添加新文件时扩展模块和配置 `/etc/httpd/conf.d/` 和 `/etc/httpd/conf.modules.d/` 文件扩展名为的目录 `.conf`

作为将Dispatcher模块添加到Apache时的完美示例，您将创建一个模块 `.so` 文件位置 ` /etc/httpd/modules/` 然后通过添加文件将其包含在 `/etc/httpd/conf.modules.d/02-dispatcher.conf` 包含加载模块的内容 `.so` 文件

```
LoadModule dispatcher_module modules/mod_dispatcher.so
```

>[!NOTE]
>
>我们没有修改Apache提供的任何现有文件。 相反，我们只是将我们的添加到他们将要使用的目录中。

现在，我们在文件中使用了模块 <b>`/etc/httpd/conf.d/dispatcher_vhost.conf`</b> 它会初始化模块并加载特定于初始模块的配置文件

```
<IfModule disp_apache2.c> 
    DispatcherConfig conf.dispatcher.d/dispatcher.any 
    ...SNIP... 
</IfModule>
```

您会再次注意到我们添加了文件和模块，但没有更改任何原始文件。  这为我们提供了所需的功能，并保护我们免受缺少所需的修补程序修补程序以及保持与包每次升级的最高兼容性级别的影响。

[下一步 — >配置文件说明](./explanation-config-files.md)
