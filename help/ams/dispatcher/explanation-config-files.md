---
title: Dispatcher配置文件说明
description: 了解配置文件、命名约定等。
version: 6.5
topic: Administration
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
source-git-commit: cc085af90b9b8ea0e650546c251fbf14cc222989
workflow-type: tm+mt
source-wordcount: '1705'
ht-degree: 0%

---


# 配置文件说明

[目录](./overview.md)

[&lt; — 上一个：基本文件布局](./basic-file-layout.md)

本文档将对部署在Adobe Managed Services中配置的标准构建Dispatcher服务器中的每个配置文件进行划分和说明。 其用法、命名规范等……

## 命名约定

Apache Web Server使用 `Include` 或 `IncludeOptional` 语句。  使用可消除冲突和混淆的名称正确命名它们有助于 <b>ton</b>. 使用的名称将描述文件的应用范围，使其更轻松。 如果所有内容都已命名 `.conf` 这真的让人困惑。 我们希望避免文件和扩展名名称不佳。  以下是典型AMS配置的Dispatcher中使用的不同自定义文件扩展名和命名约定的列表。

## conf.d/中包含的文件

| 文件 | 文件目标 | 描述 |
| ---- | ---------------- | ----------- |
| 文件名`.conf` | `/etc/httpd/conf.d/` | 默认的Enterprise Linux安装使用此文件扩展名和包含文件夹作为覆盖httpd.conf中声明的设置的位置，并允许您在Apache中在全局级别添加其他功能。 |
| 文件名`.vhost` | 暂存： `/etc/httpd/conf.d/available_vhosts/`<br>活动： `/etc/httpd/conf.d/enabled_vhosts/`<br/><br/><div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>注意：</b> .vhost文件不会复制到enabled_vhosts文件夹中，而是使用符号链接指向available_vhosts/\*.vhost文件的相对路径</div></u><br><br> | \*.vhost（虚拟主机）文件为 `<VirtualHosts>`  用于匹配主机名的条目，并允许Apache使用不同的规则处理每个域流量。 从 `.vhost` 文件，其他文件，如 `rewrites`, `whitelisting`, `etc` 中。 |
| 文件名`_rewrite.rules` | `/etc/httpd/conf.d/rewrites/` | `*_rewrite.rules` 文件存储 `mod_rewrite` 要由 `vhost` 文件 |
| 文件名`_whitelist.rules` | `/etc/httpd/conf.d/whitelists/` | `*_ipwhitelist.rules` 文件从内部包含 `*.vhost` 文件。 它包含IP正则表达式或允许拒绝规则以允许将IP列入白名单。 如果您尝试根据IP地址限制查看虚拟主机，则将生成其中一个文件，并将其包含在 `*.vhost` 文件 |

## conf.dispatcher.d/中包含的文件

| 文件 | 文件目标 | 描述 |
| --- | --- | --- |
| 文件名`.any` | `/etc/httpd/conf.dispatcher.d/` | AEM Dispatcher Apache模块从 `*.any` 文件。 默认父包含文件为 `conf.dispatcher.d/dispatcher.any` |
| 文件名`_farm.any` | 暂存： `/etc/httpd/conf.dispatcher.d/available_farms/`<br>活动： `/etc/httpd/conf.dispatcher.d/enabled_farms/`<br><br><div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>注意：</b> 这些场文件将不会复制到 `enabled_farms` 文件夹，但使用 `symlinks` 到的相对路径 `available_farms/*_farm.any` 文件 </div> <br/>`*_farm.any` 文件包含在内 `conf.dispatcher.d/dispatcher.any` 文件。 这些父场文件用于控制每种呈现类型或网站类型的模块行为。 文件是在 `available_farms` 目录和 `symlink` 到 `enabled_farms` 目录访问Advertising Cloud的帮助。  <br/>它会按名称从 `dispatcher.any` 文件。<br/><b>基线</b> 场文件以开头 `000_` 以确保先装好。<br><b>自定义</b> 场文件应在之后加载，方法是在 `100_` 以确保正确的包含行为。 |
| 文件名`_filters.any` | `/etc/httpd/conf.dispatcher.d/filters/` | `*_filters.any` 文件从内部包含 `conf.dispatcher.d/enabled_farms/*_farm.any` 文件。 每个场都有一组规则，这些规则会更改应过滤掉的流量，而不是将流量发送给渲染器。 |
| 文件名`_vhosts.any` | `/etc/httpd/conf.dispatcher.d/vhosts/` | `*_vhosts.any` 文件从内部包含 `conf.dispatcher.d/enabled_farms/*_farm.any` 文件。 这些文件是主机名或URI路径的列表，要通过Blob匹配进行匹配，以确定要用于提供该请求的呈现器 |
| 文件名`_cache.any` | `/etc/httpd/conf.dispatcher.d/cache/` | `*_cache.any` 文件从内部包含 `conf.dispatcher.d/enabled_farms/*_farm.any` 文件。 这些文件会指定哪些项目已缓存，哪些项目未缓存 |
| 文件名`_invalidate_allowed.any` | `/etc/httpd/conf.dispatcher.d/cache/` | `*_invalidate_allowed.any` 文件包含在内 `conf.dispatcher.d/enabled_farms/*_farm.any` 文件。 它们指定允许哪些IP地址发送刷新和失效请求。 |
| 文件名`_clientheaders.any` | `/etc/httpd/conf.dispatcher.d/clientheaders/` | `*_clientheaders.any` 文件包含在内 `conf.dispatcher.d/enabled_farms/*_farm.any` 文件。 它们指定应将哪些客户端标头传递到每个渲染器。 |
| 文件名`_renders.any` | `/etc/httpd/conf.dispatcher.d/renders/` | `*_renders.any` 文件包含在内 `conf.dispatcher.d/enabled_farms/*_farm.any` 文件。 它们为每个渲染器指定IP、端口和超时设置。 正确的呈现器可以是livecycle服务器或任何AEM系统，Dispatcher可在其中获取/代理来自的请求 |

## 避免的问题

遵循命名规范时，您可以避免一些非常容易的错误，这些错误可能会产生灾难性的结果。  我们将介绍几个例子。

### 问题示例

例如，ExampleCo的两个配置文件是由调度程序配置的开发人员创建的。

<b>/etc/httpd/conf.d/exampleco.conf</b>

```
<VirtualHost *:80> 
    ServerName  "exampleco" 
    ServerAlias "www.exampleco.com" 
    .......... SNIP ............... 
    <IfModule mod_rewrite.c> 
        ReWriteEngine   on 
        LogLevel warn rewrite:trace1 
        Include /etc/httpd/conf.d/rewrites/exampleco.conf 
    </IfModule> 
</VirtualHost>
```

<b>/etc/httpd/conf.d/rewrites/exampleco.conf</b>

```
RewriteRule ^/$ /content/exampleco/en.html [PT,L] 
RewriteRule ^/robots.txt$ /content/dam/exampleco/robots.txt [PT,L]
```

#### `POTENTIAL DANGER - The file names are the same`

如果 `vhost` 文件被意外放入 `rewrites` 文件夹和 `rewrites file` 被放入 `vhosts` 文件夹。  该文件似乎按文件名正确部署，但Apache将引发 *错误* 问题不会立即显现。

<b>这通常如何成为问题</b>

如果 `two files` 下载到 `same` 位置 `overwrite themselves` 或者让部署过程变成噩梦，这一点没有区别。

<b>文件扩展名相同，并且易于自动包含</b>

文件扩展名相同，并使用Apache将包含的自动扩展名 `auto include` any `.conf` 许多默认文件夹中的文件。

<b>这通常如何成为问题</b>

如果vhost文件的扩展名为 `.conf` 放入 `/etc/httpd/conf.d/` 文件夹，它会尝试将其加载到Apache上的内存中，这通常是正常的，但如果重写规则文件的扩展名为 `.conf` 被放入 `/etc/httpd/conf.d/` 文件夹，则会自动包含该变量并在全局范围内应用，从而导致结果混乱和不希望。

## 解决方法

根据文件的操作命名文件，并安全地从自动包含规则命名空间中命名。

如果是虚拟主机文件名，则使用 `.vhost` 作为扩展。

如果是重写规则文件，请将其命名为site`_rewrite.rules` 作为后缀和扩展名。 此命名约定将明确其用于哪个站点，并且是一组重写规则。

如果它是IP白名单规则文件，请将其命名为description`_whitelist.rules` 作为后缀和扩展名。 此命名约定将对其用途进行一些描述，并且是一组IP匹配规则。

如果文件被移动到它不属于的自动包含目录中，使用这些命名约定将避免出现问题。

例如，放置名为的文件 `.rules`, `.any`或 `.vhost` 在的自动包含文件夹中 `/etc/httpd/conf.d/` 不会有任何影响。

如果部署更改请求显示“请将exampleco_rewrite.rules部署到生产Dispatcher”，则部署更改的人员已经知道他们没有添加新站点，他们只是在更新文件名指示的重写规则。

### 包含订单

在企业Linux上安装的Apache Webserver中扩展功能和配置时，您有一些重要的包含订单，您需要了解这些订单

### Apache基线包含

![](assets/explanation-config-files/Apache-Webserver-Baseline-Includes.png)

如上图所示，httpd二进制文件仅将httpd.conf文件作为其配置文件进行查看。  该文件中包含以下语句：

```
Include conf.modules.d/*.conf 
IncludeOptional conf.d/*.conf
```

### AMS顶级包含项

在应用标准时，我们添加了一些其他文件类型并包含我们自己的文件类型。

以下是AMS基线目录和顶级包含项
![AMS基线包含以dispatcher_vhost.conf开头，该参数将包含/etc/httpd/conf.d/enabled_vhosts/目录中任何带*.vhost的文件。  /etc/httpd/conf.d/enabled_vhosts/目录中的项目是指向/etc/httpd/conf.d/available_vhosts/中实际配置文件的符号链接](assets/explanation-config-files/Apache-Webserver-AMS-Baseline-Includes.png "Apache-Webserver-AMS-Baseline-Include")

基于Apache的基线，我们展示了AMS如何为创建其他文件夹和顶级包含 `conf.d` 文件夹以及嵌套在 `/etc/httpd/conf.dispatcher.d/`

加载Apache时，它将加入 `/etc/httpd/conf.modules.d/02-dispatcher.conf` 该文件将包含二进制文件 `/etc/httpd/modules/mod_dispatcher.so` 进入运行状态。

```
LoadModule dispatcher_module modules /mod_dispatcher .so
```

在 `<VirtualHost />` 我们将配置文件放入 `/etc/httpd/conf.d/` 已命名 `dispatcher_vhost.conf` 在此文件中，您将看到使用设置模块工作所需的基本参数：

```
<IfModule disp_apache2.c> 
    DispatcherConfig conf.dispatcher.d/dispatcher.any 
    ...SNIP... 
</IfModule>
```

如您所见，这包括顶级 `dispatcher.any` 文件，以便Dispatcher模块从 `/etc/httpd/conf.dispatcher.d/dispatcher.any`

请注意此文件的内容：

```
/farms { 
    $include "enabled_farms/*_farm.any" 
}
```

顶级 `dispatcher.any` 文件包含位于中的所有已启用场文件 `/etc/httpd/conf.dispatcher.d/enabled_farms/` 文件名为 `FILENAME_farm.any` 符合我们的标准命名约定。

稍后在 `dispatcher_vhost.conf` 前面提到的文件我们还执行include语句以启用位于中的每个已启用虚拟主机文件 `/etc/httpd/conf.d/enabled_vhosts/` 文件名为 `FILENAME.vhost` 符合我们的标准命名约定。

```
IncludeOptional /etc/httpd/conf.d/enabled_vhosts/*.vhost
```

在我们的每个.vhost文件中，您会注意到Dispatcher模块被初始化为目录的默认文件处理程序。  以下是一个.vhost文件示例，用于显示语法：

```
<VirtualHost *:80> 
 ServerName "weretail" 
 ServerAlias www.weretail.com weretail.com 
 <Directory /> 
  <IfModule disp_apache2.c> 
   ....SNIP.... 
   SetHandler dispatcher-handler 
  </IfModule> 
  ....SNIP.... 
 </Directory> 
 ....SNIP.... 
</VirtualHost>
```

在顶级包含解决之后，他们还有其他值得一提的子包含。  下面是一个概要图，介绍场和vhosts文件如何包含其他子元素

### AMS虚拟主机包含

![此图显示了一个.vhost文件如何包含来自变量、白名单和重写文件夹的文件](assets/explanation-config-files/Apache-Webserver-AMS-Vhost-Includes.png "Apache-Webserver-AMS-Vhost-Includes")

如果有 `.vhost` 文件 `/etc/httpd/conf.d/availabled_vhosts/` 目录符号链接到 `/etc/httpd/conf.d/enabled_vhosts/` 将在运行配置中使用的目录。

的 `.vhost` 文件具有基于我们找到的常见片段的子包含。  变量、白名单和重写规则等。

的 `.vhost` 文件将根据每个文件中需要包含的位置为每个文件包含语句 `.vhost` 文件。  以下是 `.vhost` 文件作为良好引用：

```
Include /etc/httpd/conf.d/variables/weretail.vars 
<VirtualHost *:80> 
 ServerName "${MAIN_DOMAIN}" 
 <Directory /> 
  Include /etc/httpd/conf.d/whitelists/weretail*_whitelist.rules 
  <IfModule disp_apache2.c> 
   ....SNIP.... 
   SetHandler dispatcher-handler 
  </IfModule> 
  ....SNIP.... 
 </Directory> 
 ....SNIP.... 
 <IfModule mod_rewrite.c> 
  ReWriteEngine   on 
  LogLevel warn rewrite:trace1 
  Include /etc/httpd/conf.d/rewrites/weretail_rewrite.rules 
 </IfModule> 
</VirtualHost>
```

如上例所示，此配置文件中需要的变量有一个包含，供以后使用。

文件内部 `/etc/httpd/conf.d/variables/weretail.vars` 我们可以看到定义的变量：

```
Define MAIN_DOMAIN dev.weretail.com
```

您还可以看到包含 `_whitelist.rules` 根据不同的白名单标准限制谁可以查看此内容的文件。  让我们查看其中一个白名单文件的内容 `/etc/httpd/conf.d/whitelists/weretail_mainoffice_whitelist.rules`:

```
<RequireAny> 
  Require ip 192.150.16.0/23 
</RequireAny>
```

您还可以看到包含一组重写规则的行。  让我们看看 `weretail_rewrite.rules` 文件：

```
RewriteRule ^/robots.txt$ /content/dam/weretail/robots.txt [NC,PT] 
RewriteCond %{SERVER_NAME} brand1.weretail.net [NC] 
RewriteRule ^/favicon.ico$ /content/dam/weretail/favicon.ico [NC,PT] 
RewriteCond %{SERVER_NAME} brand2.weretail.com [NC] 
RewriteRule ^/sitemap.xml$ /content/weretail/general/sitemap.xml [NC,PT] 
RewriteRule ^/logo.jpg$ /content/dam/weretail/general/logo.jpg [NC,PT]
```

### AMS场包含

![<FILENAME>_farms.any将包含子.any文件以完成场配置。  在此图中，您可以看到场将包含每个顶级部分文件缓存、clientheaders、过滤器、renders和vhosts .any文件](assets/explanation-config-files/Apache-Webserver-AMS-Farm-Includes.png "Apache-Webserver-AMS-Farm-Includes")

当从 `/etc/httpd/conf.dispatcher.d/available_farms/` 目录符号链接到 `/etc/httpd/conf.dispatcher.d/enabled_farms/` 将在运行配置中使用的目录。

场文件具有基于 [场的顶级部分](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#defining-farms-farms) 如cache、clientheader、filter、renders和vhosts。

的 `FILENAME_farm.any` 文件将根据需要包含在场文件中的位置为每个文件包含包含语句。  以下是 `FILENAME_farm.any` 文件作为良好引用：

```
/weretailfarm {   
 /clientheaders { 
  $include "/etc/httpd/conf.dispatcher.d/clientheaders/ams_publish_clientheaders.any" 
  $include "/etc/httpd/conf.dispatcher.d/clientheaders/ams_common_clientheaders.any" 
 } 
 /virtualhosts { 
  $include "/etc/httpd/conf.dispatcher.d/vhosts/weretail_vhosts.any" 
 } 
 /renders { 
  $include "/etc/httpd/conf.dispatcher.d/renders/ams_publish_renders.any" 
 } 
 /filter { 
  $include "/etc/httpd/conf.dispatcher.d/filters/ams_publish_filters.any" 
  $include "/etc/httpd/conf.dispatcher.d/filters/weretail_search_filters.any" 
 } 
 ....SNIP.... 
 /cache { 
  ....SNIP.... 
  /rules { 
   $include "/etc/httpd/conf.dispatcher.d/cache/ams_publish_cache.any" 
  } 
  ....SNIP.... 
  /allowedClients { 
   /0000 { 
    /glob "*.*.*.*" 
    /type "deny" 
   } 
   $include "/etc/httpd/conf.dispatcher.d/cache/ams_publish_invalidate_allowed.any" 
  } 
 ....SNIP.... 
 } 
}
```

如您所见，weretail场的每个部分都使用include语句，而不是包含所需的所有语法。

下面我们看一下其中一些包含的语法，以了解每个子包含的外观

`/etc/httpd/conf.dispatcher.d/vhosts/weretail_publish_vhosts.any`:

```
"brand1.weretail.com" 
"brand2.weretail.com" 
"www.weretail.comf"
```

如您所见，这是一个新的、以行分隔的域名列表，应从此场呈现给其他场。

接下来，让我们看一下 `/etc/httpd/conf.dispatcher.d/filters/weretail_search_filters.any`:

```
/400 { /type "allow" /method "GET" /path "/bin/weretail/lists/*" /extension "json" } 
/401 { /type "allow" /method "POST" /path "/bin/weretail/search/' /extension "html" }
```

[下一步 — >了解缓存](./understanding-cache.md)
