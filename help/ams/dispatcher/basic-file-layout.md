---
title: AMS Dispatcher基本文件布局
description: 了解基本Apache和Dispatcher文件布局。
version: Experience Manager 6.5
topic: Administration, Development
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
doc-type: Article
exl-id: 8a3f2bb9-3895-45c6-8bb5-15a6d2aac50e
duration: 308
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1130'
ht-degree: 0%

---

# 基本文件布局

[目录](./overview.md)

[&lt; — 上一页：什么是“Dispatcher”](./what-is-the-dispatcher.md)

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

>[!BEGINSHADEBOX “注释”]

Adobe Managed Services服务器映像通常具有小型操作系统根驱动器。  我们将数据放入一个单独的卷中，该卷通常装载在`/mnt`中
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
   - 此文件夹托管所有名为`.vhost`的VirtualHost /文件
- `/etc/httpd/conf.d/enabled_vhosts/`
   - 准备好使用`.vhost`文件时，`available_vhosts`文件夹内会使用`enabled_vhosts`目录中的相对路径将其符号链接

### 其他`conf.d`目录

还有一些在Apache配置中常见的片段，我们创建了子目录，以便采用简洁的方式分隔这些文件，而不是将所有文件放在一个目录中

#### 重写目录

此目录可以包含您创建的所有`_rewrite.rules`文件，这些文件包含与Apache Web服务器[mod_rewrite](https://httpd.apache.org/docs/current/mod/mod_rewrite.html)模块配合使用的典型RewriteRulesyntax

- `/etc/httpd/conf.d/rewrites/`

#### 白名单目录

此目录可以包含您创建的所有`_whitelist.rules`文件，这些文件包含与Apache Web服务器[访问控制](https://httpd.apache.org/docs/2.4/howto/access.html)有关的典型`IP Allow`或`Require IP`语法

- `/etc/httpd/conf.d/whitelists/`

#### 变量目录

此目录可以包含您创建的所有`.vars`文件，这些文件包含您可以在配置文件中使用的变量

- `/etc/httpd/conf.d/variables/`

### 特定于Dispatcher模块的配置目录

Apache Web Server极具可扩展性，当模块具有大量配置文件时，最佳做法是在安装基目录下创建自己的配置目录，而不是将默认目录乱七八糟。

我们遵循最佳实践，创建自己的

#### 模块配置文件目录

- `/etc/httpd/conf.dispatcher.d/`

#### 暂存和已启用的场

以下目录允许您构建配置文件，这些文件具有暂存区，您可以处理文件，并且只能在文件准备就绪时启用。
- `/etc/httpd/conf.dispatcher.d/available_farms/`
   - 此文件夹承载您所有名为`_farm.any`的`/myfarm {`文件
- `/etc/httpd/conf.dispatcher.d/enabled_farms/`
   - 当您准备好使用场文件时，您可以在available_farms文件夹内使用enabled_farms目录的相对路径将它们与符号链接

### 其他`conf.dispatcher.d`目录

还有一些片段是Dispatcher场文件配置的子部分，我们创建了子目录，以便采用简洁的方式分隔这些文件，并且不会将所有文件放在一个目录中

#### 缓存目录

此目录包含您创建的所有`_cache.any`、`_invalidate.any`文件，这些文件包含您希望模块如何处理来自AEM的缓存元素以及失效规则语法的规则。  有关该部分的更多详细信息，请单击[此处](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=zh-Hans#configuring-the-dispatcher-cache-cache)

- `/etc/httpd/conf.dispatcher.d/cache/`

#### 客户端标头目录

此目录可以包含您创建的所有`_clientheaders.any`文件，这些文件包含您希望在收到请求时传递到AEM的客户端标头列表。  有关此节的更多详细信息位于[此处](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=zh-Hans)

- `/etc/httpd/conf.dispatcher.d/clientheaders/`

#### 筛选器目录

此目录可以包含您创建的所有`_filters.any`文件，这些文件包含要阻止或允许通过Dispatcher的流量到达AEM的所有筛选规则

- `/etc/httpd/conf.dispatcher.d/filters/`

#### 渲染目录

此目录可以包含您创建的所有`_renders.any`文件，这些文件包含到Dispatcher将从中使用内容的每个后端服务器的连接详细信息

- `/etc/httpd/conf.dispatcher.d/renders/`

#### Vhosts目录

此目录可以包含您创建的所有`_vhosts.any`文件，这些文件包含要与特定场匹配到特定后端服务器的域名和路径的列表

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

而是在原始文件旁边创建一个`.rpmnew`文件。  这意味着您将丢失一些您可能想要的更改，并在配置文件夹中创建更多垃圾。

即更新安装期间的RPM将查看`httpd.conf`，如果它处于`unaltered`状态，它将&#x200B;*替换*&#x200B;文件，您将获得重要更新。  如果`httpd.conf`是`altered`，则&#x200B;*不会替换*&#x200B;该文件，而是会创建一个名为`httpd.conf.rpmnew`的参考文件，许多所需的修补程序将位于该文件中，不适用于服务启动。

Enterprise Linux已正确设置，可更好地处理此用例。  它们为您提供了可以扩展或覆盖它们为您设置的默认值的区域。  在httpd的基本安装中，您会找到文件`/etc/httpd/conf/httpd.conf`，其语法如下：

```
Include conf.modules.d/.conf
IncludeOptional conf.d/.conf
```

其想法是，Apache希望您扩展模块和配置，以将新文件添加到文件扩展名为`.conf`的`/etc/httpd/conf.d/`和`/etc/httpd/conf.modules.d/`目录

作为将Dispatcher模块添加到Apache时的完美示例，您将在` /etc/httpd/modules/`中创建模块`.so`文件，然后通过在`/etc/httpd/conf.modules.d/02-dispatcher.conf`中添加包含加载模块`.so`文件内容的文件来包含该模块

```
LoadModule dispatcher_module modules/mod_dispatcher.so
```

>[!NOTE]
>
>我们没有修改Apache提供的任何现有文件。 相反，我们只是将我们的添加到他们将要使用的目录中。

现在，我们在文件<b>`/etc/httpd/conf.d/dispatcher_vhost.conf`</b>中使用了模块，该文件初始化了模块并加载了特定于初始模块的配置文件

```
<IfModule disp_apache2.c> 
    DispatcherConfig conf.dispatcher.d/dispatcher.any 
    ...SNIP... 
</IfModule>
```

您会再次注意到我们添加了文件和模块，但没有更改任何原始文件。  这为我们提供了所需的功能，并保护我们免受缺少所需的修补程序修补程序以及保持与包每次升级的最高兼容性级别的影响。

[下一步 — >配置文件说明](./explanation-config-files.md)
