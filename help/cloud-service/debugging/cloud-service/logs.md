---
title: 日志
description: 在AEM as a Cloud Service中，日志是调试AEM应用程序的首选工具，但取决于已部署的AEM应用程序中是否有足够的日志记录。
feature: 开发人员工具
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5432
thumbnail: kt-5432.jpg
topic: 开发
role: Developer
level: Beginner
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '1002'
ht-degree: 2%

---


# 使用日志调试AEM as aCloud Service

在AEM as a Cloud Service中，日志是调试AEM应用程序的首选工具，但取决于已部署的AEM应用程序中是否有足够的日志记录。

给定环境的AEM服务（创作、发布/发布调度程序）的所有日志活动都会合并到单个日志文件中，即使该服务中的不同Pod会生成日志语句。

面板ID在每个log语句中提供，并允许过滤或拼合log语句。 面板ID的格式为：

+ `cm-p<PROGRAM ID>-e<ENVIRONMENT ID>-aem-<author|publish>-<POD NAME>`
+ 示例: `cm-p12345-e56789-aem-author-abcdefabde-98765`

## 自定义日志文件

AEM as aCloud Services不支持自定义日志文件，但支持自定义日志记录。

要使Java日志在AEM中作为Cloud Service可用(通过[Cloud Manager](#cloud-manager)或[Adobe I/OCLI](#aio))，必须在`error.log`中写入自定义日志语句。 写入自定义命名日志（如`example.log`）的日志将无法从AEM as a Cloud Service访问。

日志可以使用应用程序`org.apache.sling.commons.log.LogManager.factory.config~example.cfg.json`文件中的Sling LogManager OSGi配置属性写入`error.log`。

```
{
   ...
   "org.apache.sling.commons.log.file": "logs/error.log"
   ...
}
```

## AEM创作和发布服务日志

AEM创作和发布服务都提供AEM运行时服务器日志：

+ `aemerror` 是Java错误日志(可在AEM SDK本 `/crx-quickstart/logs/error.log` 地快速启动中找到)。以下是每种环境类型的自定义日志记录器的[推荐日志级别](#log-levels):
   + 开发: `DEBUG`
   + 暂存: `WARN`
   + 生产: `ERROR`
+ `aemaccess` 列出发往AEM服务的HTTP请求及详细信息
+ `aemrequest` 列出对AEM服务发出的HTTP请求及其相应的HTTP响应

## AEM发布Dispatcher日志

只有AEM Publish Dispatcher会提供Apache Web服务器和Dispatcher日志，因为这些方面仅存在于AEM发布层中，而不存在于AEM创作层中。

+ `httpdaccess` 列出对AEM服务的Apache Web服务器/调度程序发出的HTTP请求。
+ `httperror`  列出来自Apache Web服务器的日志消息，并帮助调试支持的Apache模块，如 `mod_rewrite`。
   + 开发: `DEBUG`
   + 暂存: `WARN`
   + 生产: `ERROR`
+ `aemdispatcher` 列出来自Dispatcher模块的日志消息，包括从缓存消息中过滤和提供内容。
   + 开发: `DEBUG`
   + 暂存: `WARN`
   + 生产: `ERROR`

## Cloud Manager{#cloud-manager}

Adobe云管理器允许通过环境的下载日志操作按天下载日志。

![Cloud Manager — 下载日志](./assets/logs/download-logs.png)

可以通过任何日志分析工具下载和检查这些日志。

## Adobe I/OCLI和Cloud Manager插件{#aio}

AdobeCloud Manager支持通过[Adobe I/OCLI](https://github.com/adobe/aio-cli)以Adobe I/OCLI](https://github.com/adobe/aio-cli-plugin-cloudmanager)的[Cloud Manager插件访问AEM作为Cloud Service日志。

首先， [使用Cloud Manager插件](../../local-development-environment/development-tools.md#aio-cli)设置Adobe I/O。

确保已识别相关的程序ID和环境ID，并使用[list-available-log-options](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerlist-available-log-options-environmentid)列出用于[tail](#aio-cli-tail-logs)或[download](#aio-cli-download-logs)日志的日志选项。

```
$ aio cloudmanager:list-programs
Program Id Name      Enabled 
14304      Program 1 true    
11454      Program 2 true 
11502      Program 3 true    

$ aio config:set cloudmanager_programid <PROGRAM ID>

$ aio cloudmanager:list-environments        
Environment Id Name            Type  Description 
22295          program-3-dev   dev               
22310          program-3-prod  prod              
22294          program-3-stage stage   

$ aio cloudmanager:list-available-log-options <ENVIRONMENT ID>
Environment Id Service    Name          
22295          author     aemaccess     
22295          author     aemerror      
22295          author     aemrequest    
22295          publish    aemaccess     
22295          publish    aemerror      
22295          publish    aemrequest    
22295          dispatcher httpdaccess   
22295          dispatcher httpderror    
22295          dispatcher aemdispatcher 
```

### 跟踪日志{#aio-cli-tail-logs}

Adobe I/OCLI使用[tail-logs](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagertail-log-environmentid-service-name)命令从AEM作为Cloud Service实时跟踪日志。 跟踪在对AEM as a Cloud Service环境执行操作时，对于监视实时日志活动非常有用。

```
$ aio config:set cloudmanager_programid <PROGRAM ID>
$ aio cloudmanager:tail-logs <ENVIRONMENT ID> <SERVICE> <NAME>
```

其他命令行工具（如`grep`）可与`tail-logs`一起使用，以帮助隔离感兴趣的日志语句，例如：

```
$ aio cloudmanager:tail-logs 12345 author | grep com.example.MySlingModel
```

...仅显示从`com.example.MySlingModel`生成或包含该字符串的日志语句。

### 下载日志{#aio-cli-download-logs}

Adobe I/OCLI使用[download-logs](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerdownload-logs-environmentid-service-name-days)命令能够从AEM作为Cloud Service下载日志。 这与从Cloud Manager Web UI下载日志的结果相同，其区别在于`download-logs`命令会根据请求的日志天数来整合跨天的日志。

```
$ aio config:set cloudmanager_programid <PROGRAM ID>
$ aio cloudmanager:download-logs <ENVIRONMENT> <SERVICE> <NAME> <DAYS>
```

## 了解日志

作为Cloud Service登录的AEM具有多个Pod，可将log语句写入其中。 由于多个AEM实例写入同一日志文件，因此在调试时了解如何分析和减少噪音至关重要。 要进行说明，将使用以下`aemerror`日志代码片段：

```
01.01.2020 12:00:00.000 [cm-p12345-e56789-aem-author-abcdefg-1111] *DEBUG* [qtp2078364989-269] com.example.components.impl.ExampleModelImpl Preparing to collect resources
01.01.2020 12:00:01.002 [cm-p12345-e56789-aem-author-abcdefg-2222] *WARN*  [qtp40782847611-87] com.example.services.impl.ExampleServiceImpl Unable to resolve resource [ /content/example ] to a resource. Aborting.
01.01.2020 12:00:02.003 [cm-p12345-e56789-aem-author-abcdefg-1111] *ERROR* [qtp2078364989-269] com.example.components.impl.ExampleModelImpl Unable to collect any resources
```

使用面板ID（日期和时间之后的数据点），可以通过服务中的Pod或AEM实例来整理日志，从而更便于跟踪和了解代码执行。

__Pod cm-p12345-e56789-aem-author-abcdefg-1111__

```
01.01.2020 12:00:00.000 [cm-p12345-e56789-aem-author-abcdefg-1111] *DEBUG* [qtp2078364989-269] com.example.components.impl.ExampleModelImpl Preparing to collect resources
01.01.2020 12:00:02.003 [cm-p12345-e56789-aem-author-abcdefg-1111] *ERROR* [qtp2078364989-269] com.example.components.impl.ExampleModelImpl Unable to collect any resources
```

__Pod cm-p12345-e56789-aem-author-abcdefg-2222__

```
01.01.2020 12:00:01.002 [cm-p12345-e56789-aem-author-abcdefg-2222] *WARN*  [qtp2078364989-269] com.example.services.impl.ExampleServiceImpl Unable to resolve resource [ /content/example ] to a resource. Aborting.
```

## 建议的日志级别{#log-levels}

Adobe对每个AEM as a Cloud Service环境的日志级别的一般指导如下：

+ 本地开发(AEM SDK):`DEBUG`
+ 开发: `DEBUG`
+ 暂存: `WARN`
+ 生产: `ERROR`

为每个环境类型设置最合适的日志级别是以AEM作为Cloud Service，日志级别将在代码中进行维护

+ OSGi配置中维护了Java日志配置
+ Dispatcher项目中的Apache Web服务器和Dispatcher日志级别

...因此，需要部署才能进行更改。

### 用于设置Java日志级别的环境特定变量

为每个环境设置静态的已知Java日志级别的替代方法是，使用AEM作为Cloud Service的[环境特定变量](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#environment-specific-configuration-values)参数化日志级别，从而允许通过[将CLI与Cloud Manager插件Adobe I/O](#aio-cli)动态更改这些值。

这需要更新日志记录OSGi配置以使用特定于环境的变量占位符。 [日志](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#default-values) 级别的默认值应根据Adobe推荐 [设置](#log-levels)。例如：

`/apps/example/config/org.apache.sling.commons.log.LogManager.factory.config-example.cfg.json`

```
{
    "org.apache.sling.commons.log.names": ["com.example"],
    "org.apache.sling.commons.log.level": "$[env:LOG_LEVEL;default=DEBUG]"
}
```

这种方法有其不利之处，必须加以考虑：

+ [允许使用有限数量的环境变量](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#number-of-variables)，创建用于管理日志级别的变量将使用一个。
+ 只能通过[Adobe I/OCLI](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerset-environment-variables-environmentid)或[Cloud Manager HTTP API](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#cloud-manager-api-format-for-setting-properties)以编程方式管理环境变量。
+ 对环境变量所做的更改必须由支持的工具手动重置。 忘记将高流量环境（如生产环境）重置为较少的日志级别可能会淹没日志并影响AEM性能。

_特定于环境的变量不适用于Apache Web服务器或调度程序日志配置，因为这些变量未通过OSGi配置进行配置。_
