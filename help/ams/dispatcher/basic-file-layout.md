---
title: AMS Dispatcher基本文件布局
description: 了解Apache和Dispatcher的基本文件布局。
version: 6.5
topic: Administration, Development
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
source-git-commit: 7815b1a78949c433f2c53ff752bf39dd55f9ac94
workflow-type: tm+mt
source-wordcount: '1161'
ht-degree: 1%

---


# 基本文件布局

[目录](./overview.md)

[&lt; — 上一个：什么是“调度程序”](./what-is-the-dispatcher.md)

本文档介绍AMS标准配置文件集以及此配置标准背后的思想

## 默认Enterprise Linux文件夹结构

在AMS中，基本安装使用Enterprise Linux作为基本操作系统。 安装Apache Webserver时，它具有默认安装文件集。 以下是通过安装yum存储库提供的基本RPM来安装的默认文件

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

在遵循并遵守安装设计/结构时，我们将获得以下好处：

- 更易于支持可预测的布局
- 过去曾使用过Enterprise Linux HTTPD安装的人员自动熟悉
- 允许操作系统完全支持的修补周期，不发生任何冲突或手动调整
- 避免SELinux违反错误标记的文件上下文

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>注意：</b>
Adobe Managed Services服务器映像通常具有较小的操作系统根驱动器。  我们将数据放在一个单独的卷中，该卷通常装入“/mnt”中。然后，我们使用该卷，而不是下列默认目录的默认卷

`DocumentRoot`
- 默认:`/var/www/html`
- AMS:`/mnt/var/www/html`

`Log Directory`
- 默认: `/var/log/httpd`
- AMS: `/mnt/var/log/httpd`

请记住，旧目录和新目录会映射回原始装入点，以消除混淆。
使用单独的卷并不重要，但值得一提
</div>

## AMS附加组件

AMS添加到Apache Web Server的基础安装中。

### 文档根

AMS默认文档根：
- 创作:
   - `/mnt/var/www/author/`
- 发布:
   - `/mnt/var/www/html/`
- 全面和运行状况检查维护
   - `/mnt/var/www/default/`

### 暂存和启用的VirtualHost目录

以下目录允许您构建配置文件，其中有一个暂存区域，您可以处理文件，并且只在文件准备就绪后启用暂存区域。
- `/etc/httpd/conf.d/available_vhosts/`
   - 此文件夹托管您所有名为 `.vhost`
- `/etc/httpd/conf.d/enabled_vhosts/`
   - 当您准备好使用 `.vhost` 文件，您内部 `available_vhosts` 文件夹会使用相对路径将他们符号链接到 `enabled_vhosts` 目录

### 其他 `conf.d` 目录

Apache配置中还有一些常见的子目录，我们创建了子目录，以便以简洁的方式分隔这些文件，而不是将所有文件都放在一个目录中

#### 重写目录

此目录可以包含 `_rewrite.rules` 您创建的文件，其中包含与Apache Web服务器交互的典型重写规则语法 [mod_rewrite](https://httpd.apache.org/docs/current/mod/mod_rewrite.html) 模块

- `/etc/httpd/conf.d/rewrites/`

#### 白名单目录

此目录可以包含 `_whitelist.rules` 您创建的文件，其中包含 `IP Allow` 或 `Require IP`使Apache Web服务器互动的语法 [访问控制](https://httpd.apache.org/docs/2.4/howto/access.html)

- `/etc/httpd/conf.d/whitelists/`

#### 变量目录

此目录可以包含 `.vars` 您创建的文件包含可在配置文件中使用的变量

- `/etc/httpd/conf.d/variables/`

### 特定于调度程序模块的配置目录

Apache Web Server非常可扩展，当模块具有大量配置文件时，最佳做法是在安装基目录下创建您自己的配置目录，而不是堆满默认目录。

我们遵循最佳实践并创建自己的

#### 模块配置文件目录

- `/etc/httpd/conf.dispatcher.d/`

#### 暂存和启用的场

以下目录允许您构建配置文件，其中有一个暂存区域，您可以处理文件，并且只在文件准备就绪后启用暂存区域。
- `/etc/httpd/conf.dispatcher.d/available_farms/`
   - 此文件夹托管 `/myfarm {` 已调用 `_farm.any`
- `/etc/httpd/conf.dispatcher.d/enabled_farms/`
   - 准备好使用场文件后，在available_farms文件夹内有一个使用相对路径将它们符号链接到enabled_farms目录中

### 其他 `conf.dispatcher.d` 目录

还有一些是Dispatcher场文件配置的子部分，我们创建了子目录，以便以简洁的方式分隔这些文件，而不是将所有文件都放在一个目录中

#### 缓存目录

此目录包含 `_cache.any`, `_invalidate.any` 您创建的文件包含您希望模块如何处理来自AEM的缓存元素以及失效规则语法的规则。  有关此部分的更多详细信息，请访问此处 [此处](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#configuring-the-dispatcher-cache-cache)

- `/etc/httpd/conf.dispatcher.d/cache/`

#### 客户端头目录

此目录可以包含 `_clientheaders.any` 您创建的文件包含请求传入时要传递到AEM的Client Headers列表。  有关此部分的更多详细信息包括 [此处](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=zh-Hans)

- `/etc/httpd/conf.dispatcher.d/clientheaders/`

#### 筛选器目录

此目录可以包含 `_filters.any` 您创建的文件，其中包含所有用于阻止或允许流量通过Dispatcher到达AEM的过滤器规则

- `/etc/httpd/conf.dispatcher.d/filters/`

#### 呈现目录

此目录可以包含 `_renders.any` 您创建的文件，其中包含与调度程序将从中使用的内容的每个后端服务器的连接详细信息

- `/etc/httpd/conf.dispatcher.d/renders/`

#### Vhosts目录

此目录可以包含 `_vhosts.any` 您创建的文件，其中包含与特定场与特定后端服务器匹配的域名和路径列表

- `/etc/httpd/conf.dispatcher.d/vhosts/`

## 完整文件夹结构

AMS已使用自定义文件扩展名对每个文件进行了结构化，以避免命名空间问题/冲突和任何混淆。

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

Enterprise Linux具有Apache Webserver包(httpd)的修补周期。

如果默认文件安装较少，则更改的默认文件越好，因为如果有任何修补的安全修复或配置改进是通过RPM / Yum命令应用的，则不会在更改文件顶部应用这些修补。

它而是会创建一个 `.rpmnew` 文件。  这意味着您会错过一些可能需要的更改，并在您的配置文件夹中创建更多垃圾。

例如，在更新安装期间， RPM将查看 `httpd.conf` 如果它在 `unaltered` 声明 *替换* 文件，你就会得到重要的更新。  如果 `httpd.conf` was `altered` 然后 *不会替换* 该文件将创建一个名为 `httpd.conf.rpmnew` 而且，该文件中将包含许多所需的修复，这些修复不适用于服务启动。

已正确设置Enterprise Linux，以便更好地处理此用例。  它们会为您提供可扩展或覆盖其为您设置的默认值的区域。  在httpd的基本安装中，您将找到该文件 `/etc/httpd/conf/httpd.conf`，其语法如下：

```
Include conf.modules.d/.conf
IncludeOptional conf.d/.conf
```

其构思是，Apache希望您在向 `/etc/httpd/conf.d/` 和 `/etc/httpd/conf.modules.d/` 文件扩展名为 `.conf`

作为将Dispatcher模块添加到Apache时创建模块的完美示例 `.so` 文件 ` /etc/httpd/modules/` 然后，通过将文件添加到 `/etc/httpd/conf.modules.d/02-dispatcher.conf` 包含要加载模块的内容 `.so` 文件

```
LoadModule dispatcher_module modules/mod_dispatcher.so
```

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>注意：</b>
我们未修改Apache提供的任何现有文件。  而只是将我们的目录添加到他们要转到的目录中。
</div><br/>

现在，我们在文件中使用我们的模块 <b>`/etc/httpd/conf.d/dispatcher_vhost.conf`</b> 初始化模块并加载初始模块特定的配置文件

```
<IfModule disp_apache2.c> 
    DispatcherConfig conf.dispatcher.d/dispatcher.any 
    ...SNIP... 
</IfModule>
```

您会再次注意到，我们已添加了文件和模块，但未更改任何原始文件。  这为我们提供了所需的功能，并保护我们不会丢失所需的修补程序修复，同时与软件包的每次升级保持最高级别的兼容性。

[下一步 — >配置文件说明](./explanation-config-files.md)