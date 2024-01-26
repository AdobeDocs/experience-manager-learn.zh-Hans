---
title: 使用和了解AEM Dispatcher配置中的变量
description: 了解如何在Apache和Dispatcher模块配置文件中使用变量以将它们提升到新的级别。
version: 6.5
topic: Administration, Development
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
doc-type: Article
exl-id: 299b32c3-7922-4eee-aa3a-56039a654f70
duration: 307
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '1090'
ht-degree: 1%

---

# 使用和了解变量

[目录](./overview.md)

[&lt; — 上一页：了解缓存](./understanding-cache.md)

本文档将说明如何在Apache Web Server和Dispatcher模块配置文件中利用变量的强大功能。

## 变量

Apache支持变量，并且从4.1.9版本的Dispather模块开始，它同样支持这些变量！

我们可以利用这些功能做很多有用的事情，例如：

- 确保特定于环境的任何内容不在配置中，而是被提取的，以确保来自开发环境的配置文件在产品中使用相同的功能输出。
- 切换功能并更改AMS提供且不允许您更改的不可变文件的日志级别。
- 根据以下变量更改要使用的包含 `RUNMODE` 和 `ENV_TYPE`
- 匹配 `DocumentRoot`的和 `VirtualHost` Apache配置和模块配置之间的DNS名称。

## 使用基线变量

由于AMS基线文件是只读和不可变的，因此有些功能可以关闭和打开，也可以通过编辑它们使用的变量进行配置。

### 基线变量

在文件中声明AMS默认变量 `/etc/httpd/conf.d/variables/ootb.vars`.  此文件不可编辑，但存在，以确保变量没有null值。  先包括后包括，而不包括 `/etc/httpd/conf.d/variables/ams_default.vars`.  您可以编辑该文件以更改这些变量的值，甚至可以在您自己的文件中包含相同的变量名称和值！

以下是文件内容的示例 `/etc/httpd/conf.d/variables/ams_default.vars`：

```
Define DISP_LOG_LEVEL info
Define AUTHOR_WHITELIST_ENABLED 0
Define PUBLISH_WHITELIST_ENABLED 0
Define AUTHOR_FORCE_SSL 1
Define PUBLISH_FORCE_SSL 0
```

### 示例1 — 强制SSL

上面显示的变量 `AUHOR_FORCE_SSL`，或 `PUBLISH_FORCE_SSL` 可设置为1以启用重写规则，这些规则强制最终用户在发出http请求时重定向到https

以下是允许此切换工作的配置文件语法：

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

如您所见，重写规则包括的代码用于重定向最终用户浏览器，而设置为1的变量则允许是否使用文件

### 示例2 — 日志记录级别

变量 `DISP_LOG_LEVEL` 可用于设置您希望在运行配置中实际使用的日志级别使用的日志。

以下是ams基线配置文件中存在的语法示例：

```
<IfModule disp_apache2.c> 
 DispatcherLog    logs/dispatcher.log  
 DispatcherLogLevel ${DISP_LOG_LEVEL} 
</IfModule>
```

如果您需要提高Dispatcher日志记录级别，只需更新 `ams_default.vars` 变量 `DISP_LOG_LEVEL` 达到您所需的级别。

示例值可以是整数或单词：

| 日志级别 | 整数值 | Word值 |
| --- | --- | --- |
| Trace | 4 | trace |
| 调试 | 3 | 调试 |
| 信息 | 2 | 信息 |
| 警告 | 1 | 警告 |
| 错误 | 0 | 错误 |

### 示例3 — 白名单

变量 `AUTHOR_WHITELIST_ENABLED` 和 `PUBLISH_WHITELIST_ENABLED` 可以设置为1以参与重写规则，这些规则包括根据IP地址允许或禁止最终用户流量的规则。  在上切换此功能时，需要结合创建白名单规则文件并将其包含。

下面是一些语法示例，说明变量如何启用白名单文件的包含和白名单文件示例

`sample.vhost`：

```
<VirtualHost *:80> 
 <Directory /> 
  <If "${AUTHOR_WHITELIST_ENABLED} == 1"> 
   Include /etc/httpd/conf.d/whitelists/*_whitelist.rules 
  </If> 
 </Directory> 
</VirtualHost>
```

`sample_whitelist.rules`：

```
<RequireAny> 
  Require ip 10.43.0.10/24 
</RequireAny>
```

如您所见 `sample_whitelist.rules` 强制执行IP限制，但切换变量可允许包含在 `sample.vhost`

## 变量的放置位置

### Web服务器启动参数

AMS会将服务器/拓扑特定的变量放在Apache进程的启动参数中的文件中 `/etc/sysconfig/httpd`

此文件具有预定义的变量，如下所示：

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

不能更改这些内容，但可以在配置文件中利用这些内容

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>注意：</b>

这是因为该文件仅在服务启动时包括在内。  需要重新启动服务才能获取更改。  这意味着仅重新加载是不够的，而是需要重新启动
</div>

### 变量文件(`.vars`)

您的代码提供的自定义变量应位于 `.vars` 目录中的文件 `/etc/httpd/conf.d/variables/`

这些文件可以包含所需的任何自定义变量，以下示例文件中提供了一些语法示例

`/etc/httpd/conf.d/variables/weretail_domains_dev.vars`：

```
Define WERETAIL_DOMAIN dev.weretail.com 
Define WERETAIL_ALT_DOMAIN dev.weretail.net
```

`/etc/httpd/conf.d/variables/weretail_domains_stage.vars`：

```
Define WERETAIL_DOMAIN stage.weretail.com
Define WERETAIL_ALT_DOMAIN stage.weretail.net
```

`/etc/httpd/conf.d/variables/weretail_domains_prod.vars`：

```
Define WERETAIL_DOMAIN www.weretail.com 
Define WERETAIL_ALT_DOMAIN www..weretail.net
```

创建自己的变量时，文件会根据其内容对其进行命名，并遵循手册中提供的命名标准 [此处](https://experienceleague.adobe.com/docs/experience-cloud-kcs/kbarticles/KA-17477.html#naming-convention).  在上例中，您可以看到变量文件承载不同的DNS条目，作为要在配置文件中使用的变量。

## 使用变量

现在，您已在变量文件中定义了变量，接下来您将希望了解如何在其他配置文件中正确使用它们。

我们将使用示例 `.vars` 以上文件以说明一个适当的用例。

我们希望在全局范围内包含所有基于环境的变量，我们将创建该文件 `/etc/httpd/conf.d/000_load_env_vars.conf`

```
IncludeOptional /etc/httpd/conf.d/variables/*_${ENV_TYPE}.vars
IncludeOptional /etc/httpd/conf.d/variables/*_${RUNMODE}.vars
```

我们知道，当httpd服务启动时，它会拉取AMS在中设置的变量 `/etc/sysconfig/httpd` 并且具有变量集 `ENV_TYPE` 和 `RUNMODE`

当此全局 `.conf` 文件被拉入，它将提前被拉入，因为文件的包含顺序为 `conf.d` 文件名中的字母数字加载顺序表示为000，可确保它在目录中的其他文件之前加载。

include语句还在文件名中使用变量。  这可以根据中的值更改它将实际加载的文件 `ENV_TYPE` 和 `RUNMODE` 变量。

如果 `ENV_TYPE` 值为 `dev` 则要使用的文件为：

`/etc/httpd/conf.d/variables/weretail_domains_dev.vars`

如果 `ENV_TYPE` 值为 `stage` 则要使用的文件为：

`/etc/httpd/conf.d/variables/weretail_domains_stage.vars`

如果 `RUNMODE` 值为 `preview` 则要使用的文件为：

`/etc/httpd/conf.d/variables/weretail_domains_preview.vars`

当该文件被包含时，它将允许我们使用存储在中的变量名称。

在我们的 `/etc/httpd/conf.d/available_vhosts/weretail.vhost` 文件我们可以置换仅适用于dev的常规语法：

```
<VirtualHost *:80> 
 ServerName dev.weretail.com 
 ServerAlias dev.weretail.net
```

利用新语法将变量的强大功能用于dev、stage和prod：

```
<VirtualHost *:80> 
 ServerName ${WERETAIL_DOMAIN} 
 ServerAlias ${WERETAIL_ALT_DOMAIN}
```

在我们的 `/etc/httpd/conf.dispatcher.d/vhosts/weretail_vhosts.any` 文件我们可以置换仅适用于dev的常规语法：

```
"dev.weretail.com" 
"dev.weretail.net"
```

利用新语法将变量的强大功能用于dev、stage和prod：

```
"${WERETAIL_DOMAIN}" 
"${WERETAIL_ALT_DOMAIN}"
```

无需在每个环境中部署不同的文件，这些变量便可以大量重复用于个性化运行设置。  实际上，您可以使用变量对配置文件进行模板化，并根据变量包含文件。

## 查看变量值

有时，在使用变量时，我们必须搜索以查看配置文件中的值。  通过在服务器上运行以下命令，可以查看已解析的变量：

```
source /etc/sysconfig/httpd;/sbin/httpd -S | grep Define | grep "="
```

变量在编译后的Apache配置中的外观：

```
$ source /etc/sysconfig/httpd;/sbin/httpd -t -D DUMP_CONFIG | grep -v "#"
```

变量在编译后的Dispatcher配置中的查看方式：

```
$ source /etc/sysconfig/httpd;/sbin/httpd -t -D DUMP_ANY
```

从命令输出中，您会看到配置文件中的变量与编译后的输出之间的差异。

示例配置

`/etc/httpd/conf.d/enabled_vhosts/aem_publish.vhost`：

```
<VirtualHost *:80> 
    DocumentRoot    ${PUBLISH_DOCROOT} 
```

现在运行命令以查看编译后的输出

已编译的Apache配置：

```
$ source /etc/sysconfig/httpd;/sbin/httpd -t -D DUMP_CONFIG | grep DocumentRoot
DocumentRoot /mnt/var/www/html
```

已编译的Dispatcher配置：

```
$ source /etc/sysconfig/httpd;/sbin/httpd -t -D DUMP_ANY | grep docroot
/docroot "/mnt/var/www/html"
```

[下一步 — >刷新](./disp-flushing.md)
