---
title: AMS Dispatcher运行状况检查
description: AMS提供了一个运行状况检查cgi-bin脚本，云负载均衡器将运行该脚本以查看AEM是否运行状况良好并应为公共流量提供服务。
version: 6.5
topic: Administration
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
doc-type: Article
exl-id: 69b4e469-52cc-441b-b6e5-2fe7ef18da90
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '1139'
ht-degree: 1%

---

# AMS Dispatcher运行状况检查

[目录](./overview.md)

[&lt; — 上一页：只读文件](./immutable-files.md)

安装AMS基准调度程序后，它会随附一些免费赠品。  其中一项功能是一组运行状况检查脚本。
这些脚本允许前端AEM栈栈的负载平衡器知道哪些腿正常，并使它们继续工作。

![显示通信流的动画GIF](assets/load-balancer-healthcheck/health-check.gif "运行状况检查步骤")

## 基本负载平衡器运行状况检查

当客户流量通过Internet到达您的AEM实例时，他们将通过负载平衡器

![该图显示了通过负载平衡器从Internet到aem的流量](assets/load-balancer-healthcheck/load-balancer-traffic-flow.png "load-balancer-traffic-flow")

通过负载平衡器的每个请求都将循环到每个实例。  负载平衡器内置了运行状况检查机制，以确保向正常的主机发送流量。

默认检查通常是端口检查，以查看负载平衡器中的目标服务器是否侦听端口通信进入（即TCP 80和443）

> `Note:` 虽然这项功能可以正常工作，但它没有真正衡量AEM是否健康的指标。  它仅测试Dispatcher (Apache Web Server)是否启动并运行。

## AMS运行状况检查

为避免将流量发送到在不健康的AEM实例前面的健康调度程序，AMS创建了几个额外参数来评估腿部的健康状态，而不仅仅是Dispatcher。

![此图显示了运行状况检查要执行的各个部分](assets/load-balancer-healthcheck/health-check-pieces.png "运行状况检查项")

健康检查包括以下部分
- 1 `Load balancer`
- 1 `Apache web server`
- 3 `Apache *VirtualHost* config files`
- 5 `CGI-Bin scripts`
- 1 `AEM instance`
- 1 `AEM package`

我们将介绍每个部分的设置及其重要性

### AEM包

要指示AEM是否正常运行，您需要它执行一些基本页面编译并为页面提供服务。  AdobeManaged Services创建了一个包含“测试”页面的基本包。  页面会测试存储库是否已启动，以及资源和页面模板是否可以呈现。

![图像显示CRX包管理器中的AMS包](assets/load-balancer-healthcheck/health-check-package.png "healt-check-package")

这是页面。  它将显示安装的存储库ID

![该图显示了“AMS摄取”页](assets/load-balancer-healthcheck/health-check-page.png "health-check-page")

> `Note:` 确保页面不可缓存。  如果每次返回缓存的页面时，它都不会检查实际状态！

这是轻量级端点，我们可以测试以查看AEM是否已启动并运行。

### 负载平衡器配置

我们将负载平衡器配置为指向CGI-BIN端点，而不是使用端口检查。

![图像显示AWS负载平衡器运行状况检查配置](assets/load-balancer-healthcheck/aws-settings.png "aws-lb-settings")

![图像显示Azure负载平衡器运行状况检查配置](assets/load-balancer-healthcheck/azure-settings.png "azure-lb-settings")

### Apache运行状况检查虚拟主机

#### CGI-BIN虚拟主机 `(/etc/httpd/conf.d/available_vhosts/ams_health.vhost)`

这是 `<VirtualHost>` 用于运行CGI-Bin文件的Apache配置文件。

```
Listen 81
<VirtualHost *:81>
    ServerName	"health"
    ...SNIP...
    ScriptAlias /health/ "/var/www/cgi-bin/health/"
</VirtualHost>
```

> `Note:` cgi-bin文件是可以运行的脚本。  这可能是一个易受攻击的攻击向量，并且AMS使用的这些脚本无法仅供负载平衡器测试进行公开访问。


#### 维护不正常的虚拟主机

- `/etc/httpd/conf.d/available_vhosts/000_unhealthy_author.vhost`
- `/etc/httpd/conf.d/available_vhosts/000_unhealthy_publish.vhost`

这些文件已命名 `000_` 用作前缀。  它最初配置为使用与活动站点相同的域名。  其目的是让此文件在运行状况检查检测到某个AEM后端存在问题时启用。  然后提供一个错误页面，而不是一个没有页面的503 HTTP响应代码。  它会偷走正常的流量 `.vhost` 文件，因为该文件是在该日期之前加载的 `.vhost` 文件共享相同 `ServerName` 或 `ServerAlias`.  导致发往特定域的页面转到不正常的主机，而不是正常流量流经的默认主机。

运行运行状况检查脚本时，它们将注销其当前运行状况状态。  每分钟有一次，服务器上会有一个cronjob在日志中查找不健康的条目。  如果检测到创作AEM实例不正常，则会启用符号链接：

日志条目：

```
# grep "ERROR\|publish" /var/log/lb/health_check.log
E, [2022-11-23T20:13:54.984379 #26794] ERROR -- : AUTHOR -- Exception caught: Connection refused - connect(2)
I, [2022-11-23T20:13:54.984403 #26794]  INFO -- : [checkpublish]-author:0-publish:1-[checkpublish]
```

Cron发现错误并做出反应：

```
# grep symlink /var/log/lb/health_check_reload.log
I, [2022-11-23T20:34:19.213179 #2275]  INFO -- : ADDING VHOST symlink /etc/httpd/conf.d/available_vhosts/000_unhealthy_author.vhost => /etc/httpd/conf.d/enabled_vhosts/000_unhealthy_author.vhost
```

您可以通过配置中的重新加载模式设置，来控制作者或发布的站点是否可以加载此错误页面 `/var/www/cgi-bin/health_check.conf`

```
# grep RELOAD_MODE /var/www/cgi-bin/health_check.conf
RELOAD_MODE='author'
```

有效选项：
- 作者
   - 这是默认选项。
   - 这会在作者不正常时为其设置维护页面
- 发布
   - 此选项将在发布服务器状态不佳时为其设置维护页面
- 全部
   - 此选项将为作者或发布者（或两者）设置维护页面，前提是它们运行不正常
- 无
   - 此选项将跳过此运行状况检查功能

查看 `VirtualHost` 对于这些设置，您将看到它们为启用时收到的每个请求加载相同的文档作为错误页面：

```
<VirtualHost *:80>
	ServerName	unhealthyauthor
	ServerAlias	${AUTHOR_DEFAULT_HOSTNAME}
	ErrorDocument	503 /error.html
	DocumentRoot	/mnt/var/www/default
	<Directory />
		Options FollowSymLinks
		AllowOverride None
	</Directory>
	<Directory "/mnt/var/www/default">
		AllowOverride None
		Require all granted
	</Directory>
	<IfModule mod_headers.c>
		Header always add X-Dispatcher ${DISP_ID}
		Header always add X-Vhost "unhealthy-author"
	</IfModule>
	<IfModule mod_rewrite.c>
		ReWriteEngine   on
		RewriteCond %{REQUEST_URI} !^/error.html$
		RewriteRule ^/* /error.html [R=503,L,NC]
	</IfModule>
</VirtualHost>
```

响应代码仍为 `HTTP 503`

```
# curl -I https://we-retail.com/
HTTP/1.1 503 Service Unavailable
X-Dispatcher: dispatcher1useast1
X-Vhost: unhealthy-author
```

他们将会获得此页面，而不是空白页面。

![该图显示了默认的维护页面](assets/load-balancer-healthcheck/unhealthy-page.png "不健康页面")

### CGI-Bin脚本

您的CSE可以在负载平衡器设置中配置5个不同的脚本，这些脚本可更改将Dispatcher拉出负载平衡器的行为或标准。

#### /bin/checkauthor

此脚本在使用时将检查并记录它当前的所有实例，但只有在 `author` AEM实例不正常

> `Note:` 请记住，如果发布AEM实例不正常，Dispatcher将保留服务以允许流量流向创作AEM实例

#### /bin/checkpublish （默认）

此脚本在使用时将检查并记录它当前的所有实例，但只有在 `publish` AEM实例不正常

> `Note:` 请记住，如果创作AEM实例不正常，Dispatcher将保留服务以允许流量流向发布AEM实例

#### /bin/checkeither

此脚本在使用时将检查并记录它当前的所有实例，但只有在 `author` 或 `publisher` AEM实例不正常

> `Note:` 请记住，如果发布AEM实例或创作AEM实例不正常，Dispatcher将退出服务。  也就是说，如果其中一个是健康的，它就不会接收流量

#### /bin/checkboth

此脚本在使用时将检查并记录它当前的所有实例，但只有在 `author` 和 `publisher` AEM实例不正常

> `Note:` 请记住，如果发布AEM实例或创作AEM实例不正常，Dispatcher将不会退出服务。  这意味着，如果其中一个不健康，它将继续接收流量，并在请求资源的人身上出现错误。

#### /bin/healthy

使用此脚本时，将检查并记录它当前所在的任何实例，但无论AEM是否返回错误，该脚本都将正常返回。

> `Note:` 当运行状况检查无法按预期发挥作用并允许覆盖功能将AEM实例保留在负载平衡器中时，将使用此脚本。

[下一页 — > GIT符号链接](./git-symlinks.md)
