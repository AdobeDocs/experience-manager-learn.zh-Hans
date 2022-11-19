---
title: 在AEM Dispatcher配置中使用和了解变量
description: 了解如何在Apache和Dispatcher模块配置文件中使用变量，将它们提升到新的级别。
version: 6.5
topic: Administration, Development
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
source-git-commit: 04cd4002af7028ee9e3b1e1455b6346c56446245
workflow-type: tm+mt
source-wordcount: '1089'
ht-degree: 1%

---


# 使用和了解变量

[目录](./overview.md)

[&lt; — 上一个：了解缓存](./understanding-cache.md)

本文档将介绍如何在Apache Web服务器和Dispatcher模块配置文件中利用变量的功能。

## 变量

Apache支持变量，并且自Dispather模块版本4.1.9起，它也支持这些变量！

我们可以利用这些工具执行大量有用的操作，例如：

- 确保特定于环境的任何内容不在配置中内联，而是已提取，以确保开发中的配置文件在具有相同功能输出的生产环境中工作。
- 切换功能并更改AMS提供的不可变文件的日志级别，并且不允许您进行更改。
- 根据变量(如 `RUNMODE` 和 `ENV_TYPE`
- 匹配 `DocumentRoot`s和 `VirtualHost` Apache配置和模块配置之间的DNS名称。

## 使用基线变量

由于AMS基线文件是只读的和不可变的，因此有一些功能可以切换为打开和关闭，以及通过编辑它们使用的变量进行配置。

### 基线变量

AMS默认变量在文件中声明 `/etc/httpd/conf.d/variables/ootb.vars`.  此文件不可编辑，但存在以确保变量没有空值。  它们先包含，然后包含，而不是包含 `/etc/httpd/conf.d/variables/ams_default.vars`.  您可以编辑该文件以更改这些变量的值，甚至可以在自己的文件中包含相同的变量名称和值！

以下是文件内容的示例 `/etc/httpd/conf.d/variables/ams_default.vars`:

```
Define DISP_LOG_LEVEL info
Define AUTHOR_WHITELIST_ENABLED 0
Define PUBLISH_WHITELIST_ENABLED 0
Define AUTHOR_FORCE_SSL 1
Define PUBLISH_FORCE_SSL 0
```

### 示例1 — 强制SSL

上面显示的变量 `AUHOR_FORCE_SSL`或 `PUBLISH_FORCE_SSL` 可以设置为1以引入重写规则，这些规则会在最终用户通过http请求进入时强制将其重定向到https

以下是允许此切换的配置文件语法：

```
</VirtualHost *:80> 
 <IfModule mod_rewrite.c> 
  ReWriteEngine on 
  <If "${PUBLISH_FORCE_SSL} == 1"> 
   Include /etc/httpd/conf.d/rewrites/forcessl_rewrite.rules 
  </If> 
 </IfModule> 
</VirtualHost>
```

如您所见，重写规则包括具有重定向最终用户浏览器的代码的内容，但设置为1的变量是允许使用或不使用文件的内容

### 示例2 — 日志记录级别

变量 `DISP_LOG_LEVEL` 可用于设置您希望对运行配置中实际使用的日志级别拥有的内容。

以下是ams基线配置文件中存在的语法示例：

```
<IfModule disp_apache2.c> 
 DispatcherLog    logs/dispatcher.log  
 DispatcherLogLevel ${DISP_LOG_LEVEL} 
</IfModule>
```

如果需要提高Dispatcher日志记录级别，只需更新 `ams_default.vars` 变量 `DISP_LOG_LEVEL` 达到你想要的水平。

示例值可以是整数或单词：

| 日志级别 | 整数值 | 字值 |
| --- | --- | --- |
| 跟踪 | 4 | trace |
| 调试 | 3 | 调试 |
| 信息 | 2 | 信息 |
| 警告 | 1 | 警告 |
| 错误 | 0 | 错误 |

### 示例3 — 白名单

变量 `AUTHOR_WHITELIST_ENABLED` 和 `PUBLISH_WHITELIST_ENABLED` 可设置为1以包含重写规则，该规则包含允许或禁止基于IP地址的最终用户流量的规则。  打开此功能时，需要将创建白名单规则文件并将其包含在内。

以下是一些语法示例，说明变量如何启用包含白名单文件和白名单文件示例

`sample.vhost`:

```
<VirtualHost *:80> 
 <Directory /> 
  <If "${AUTHOR_WHITELIST_ENABLED} == 1"> 
   Include /etc/httpd/conf.d/whitelists/*_whitelist.rules 
  </If> 
 </Directory> 
</VirtualHost>
```

`sample_whitelist.rules`:

```
<RequireAny> 
  Require ip 10.43.0.10/24 
</RequireAny>
```

如您所见 `sample_whitelist.rules` 强制实施IP限制，但切换变量允许将其包含在 `sample.vhost`

## 变量放置位置

### Web服务器启动参数

AMS会将服务器/拓扑特定变量放入Apache进程的启动参数中的文件内 `/etc/sysconfig/httpd`

此文件中预先定义了变量，如下所示：

```
AUTHOR_IP="10.43.0.59" 
AUTHOR_PORT="4502" 
AUTHOR_DOCROOT='/mnt/var/www/author' 
PUBLISH_IP="10.43.0.20" 
PUBLISH_PORT="4503" 
PUBLISH_DOCROOT='/mnt/var/www/html' 
ENV_TYPE='dev' 
RUNMODE='sites'
```

这些文件不是您可以更改的，但在配置文件中非常有用

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>注意:</b>

由于仅在服务启动时才包含此文件。  需要重新启动服务才能进行更改。  这意味着重新加载是不够的，而是需要重新启动
</div>

### 变量文件(`.vars`)

您的代码提供的自定义变量应位于 `.vars` 目录中的文件 `/etc/httpd/conf.d/variables/`

这些文件可以包含您需要的任何自定义变量，并且在以下示例文件中可以看到一些语法示例

`/etc/httpd/conf.d/variables/weretail_domains_dev.vars`:

```
Define WERETAIL_DOMAIN dev.weretail.com 
Define WERETAIL_ALT_DOMAIN dev.weretail.net
```

`/etc/httpd/conf.d/variables/weretail_domains_stage.vars`:

```
Define WERETAIL_DOMAIN stage.weretail.com
Define WERETAIL_ALT_DOMAIN stage.weretail.net
```

`/etc/httpd/conf.d/variables/weretail_domains_prod.vars`:

```
Define WERETAIL_DOMAIN www.weretail.com 
Define WERETAIL_ALT_DOMAIN www..weretail.net
```

在创建自己的变量文件时，根据变量文件的内容对它们进行命名，并遵循手册中提供的命名标准 [此处](https://experienceleague.adobe.com/docs/experience-cloud-kcs/kbarticles/KA-17477.html#naming-convention).  在上例中，您可以看到变量文件将不同的DNS条目作为变量托管在配置文件中。

## 使用变量

现在，您已在变量文件中定义了变量，接下来您将需要了解如何在其他配置文件中正确使用这些变量。

我们举个例子 `.vars` 文件，以说明正确的用例。

我们希望全局包含所有基于环境的变量，我们将创建文件 `/etc/httpd/conf.d/000_load_env_vars.conf`

```
IncludeOptional /etc/httpd/conf.d/variables/*_${ENV_TYPE}.vars
IncludeOptional /etc/httpd/conf.d/variables/*_${RUNMODE}.vars
```

我们知道，当httpd服务启动时，它会提取AMS在 `/etc/sysconfig/httpd` 和的变量集为 `ENV_TYPE` 和 `RUNMODE`

当此全局 `.conf` 文件会提前提取，因为文件的包含顺序 `conf.d` 文件名中的字母数字加载顺序为000，这将确保加载顺序早于目录中的其他文件。

include语句在文件名中也使用变量。  这可以根据 `ENV_TYPE` 和 `RUNMODE` 变量。

如果 `ENV_TYPE` 值 `dev` 然后使用的文件是：

`/etc/httpd/conf.d/variables/weretail_domains_dev.vars`

如果 `ENV_TYPE` 值 `stage` 然后使用的文件是：

`/etc/httpd/conf.d/variables/weretail_domains_stage.vars`

如果 `RUNMODE` 值 `preview` 然后使用的文件是：

`/etc/httpd/conf.d/variables/weretail_domains_preview.vars`

当该文件被包含在内时，它将允许我们使用存储在内的变量名称。

在 `/etc/httpd/conf.d/available_vhosts/weretail.vhost` 文件可以交换仅适用于dev的常规语法：

```
<VirtualHost *:80> 
 ServerName dev.weretail.com 
 ServerAlias dev.weretail.net
```

使用新语法，该语法使用变量的强大功能来用于dev、stage和prod:

```
<VirtualHost *:80> 
 ServerName ${WERETAIL_DOMAIN} 
 ServerAlias ${WERETAIL_ALT_DOMAIN}
```

在 `/etc/httpd/conf.dispatcher.d/vhosts/weretail_vhosts.any` 文件可以交换仅适用于dev的常规语法：

```
"dev.weretail.com" 
"dev.weretail.net"
```

使用新语法，该语法使用变量的强大功能来用于dev、stage和prod:

```
"${WERETAIL_DOMAIN}" 
"${WERETAIL_ALT_DOMAIN}"
```

这些变量在个性化运行设置时会有大量的重复使用，而无需在每个环境中部署不同的文件。  实际上，您可以使用变量来模板配置文件，并包含基于变量的文件。

## 查看变量值

有时，在使用变量时，我们必须进行搜索以查看配置文件中的值。  可通过在服务器上运行以下命令来查看已解析的变量：

```
source /etc/sysconfig/httpd;/sbin/httpd -S | grep Define | grep "="
```

变量在编译的Apache配置中的外观：

```
$ source /etc/sysconfig/httpd;/sbin/httpd -t -D DUMP_CONFIG | grep -v "#"
```

变量在编译的Dispatcher配置中的外观：

```
$ source /etc/sysconfig/httpd;/sbin/httpd -t -D DUMP_ANY
```

在命令的输出中，您将看到配置文件中变量与编译输出的差异。

配置示例

`/etc/httpd/conf.d/enabled_vhosts/aem_publish.vhost`:

```
<VirtualHost *:80> 
	DocumentRoot	${PUBLISH_DOCROOT} 
```

现在，运行命令以查看编译的输出

编译的Apache配置：

```
$ source /etc/sysconfig/httpd;/sbin/httpd -t -D DUMP_CONFIG | grep DocumentRoot
DocumentRoot /mnt/var/www/html
```

编译的调度程序配置：

```
$ source /etc/sysconfig/httpd;/sbin/httpd -t -D DUMP_ANY | grep docroot
/docroot "/mnt/var/www/html"
```

[下一个 — >刷新](./disp-flushing.md)