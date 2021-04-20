---
title: 日志
description: 日志充当在AEM中调试AEM应用程序的前线Cloud Service，但依赖于部署的AEM应用程序中的适当日志记录。
feature: Developer Tools
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5432
thumbnail: kt-5432.jpg
topic: Development
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '995'
ht-degree: 2%

---


# 使用日志将AEM作为Cloud Service进行调试

日志充当在AEM中调试AEM应用程序的前线Cloud Service，但依赖于部署的AEM应用程序中的适当日志记录。

给定环境的AEM服务（作者、发布/发布调度程序）的所有日志活动都整合到单个日志文件中，即使该服务中的不同窗格生成日志语句也是如此。

Pod Id在每个log语句中提供，并允许对log语句进行过滤或整理。 窗格ID的格式为：

+ `cm-p<PROGRAM ID>-e<ENVIRONMENT ID>-aem-<author|publish>-<POD NAME>`
+ 示例: `cm-p12345-e56789-aem-author-abcdefabde-98765`

## 自定义日志文件

AEM as aCloud Services不支持自定义日志文件，但支持自定义日志。

要在AEM中以Cloud Service形式提供Java日志(通过[Cloud Manager](#cloud-manager)或[Adobe I/O CLI](#aio))，必须在`error.log`中写入自定义日志语句。 写入自定义命名日志（如`example.log`）的日志将无法作为Cloud Service从AEM访问。

## AEM作者和发布服务日志

AEM作者服务和发布服务都提供AEM运行时服务器日志：

+ `aemerror` 是Java错误日志(可在AEM SDK本 `/crx-quickstart/error.log` 地快速启动中找到)。以下是针对每个环境类型的自定义日志程序的[推荐日志级别](#log-levels):
   + 开发: `DEBUG`
   + 暂存: `WARN`
   + 生产: `ERROR`
+ `aemaccess` 列表HTTP请求到AEM服务并提供详细信息
+ `aemrequest` 列表对AEM服务发出的HTTP请求及其相应的HTTP响应

## AEM发布调度程序日志

仅AEM Publish Dispatcher提供Apache Web服务器和Dispatcher日志，因为这些方面仅存在于AEM Publish层中，而不在AEM Author层中。

+ `httpdaccess` 列表对AEM服务的Apache Web Server/Dispatcher发出的HTTP请求。
+ `httperror`  列表从Apache Web服务器记录消息，并帮助调试受支持的Apache模块， `mod_rewrite`如
   + 开发: `DEBUG`
   + 暂存: `WARN`
   + 生产: `ERROR`
+ `aemdispatcher` 列表来自Dispatcher模块的日志消息，包括从缓存消息过滤和服务。
   + 开发: `DEBUG`
   + 暂存: `WARN`
   + 生产: `ERROR`

## Cloud Manager{#cloud-manager}

Adobe Cloud Manager允许通过环境的“下载日志”操作按天下载日志。

![云管理器 — 下载日志](./assets/logs/download-logs.png)

可以通过任何日志分析工具下载和检查这些日志。

## Adobe I/O CLI with Cloud Manager plugin{#aio}

Adobe Cloud Manager支持通过[Adobe I/OCLI](https://github.com/adobe/aio-cli)作为Cloud Service日志访问AEM，该CLI](https://github.com/adobe/aio-cli-plugin-cloudmanager)具有Adobe I/OCLI[云管理器插件。

首先，[使用Cloud Manager plugin](../../local-development-environment/development-tools.md#aio-cli)设置Adobe I/O。

确保已识别相关的项目ID和环境ID，并使用[列表-available-log-options](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerlist-available-log-options-environmentid)列表用于[tail](#aio-cli-tail-logs)或[download](#aio-cli-download-logs)日志的日志选项。

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

Adobe I/O CLI能够使用[tail-logs](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagertail-log-environmentid-service-name)命令以Cloud Service形式实时跟踪AEM日志。 跟踪功能对于在AEM上作为Cloud Service环境执行操作时监视实时日志活动很有用。

```
$ aio config:set cloudmanager_programid <PROGRAM ID>
$ aio cloudmanager:tail-logs <ENVIRONMENT ID> <SERVICE> <NAME>
```

其他命令行工具（如`grep`）可与`tail-logs`一起使用，以帮助隔离感兴趣的日志语句，例如：

```
$ aio cloudmanager:tail-logs 12345 author | grep com.example.MySlingModel
```

...仅显示从`com.example.MySlingModel`生成的日志语句，或在其中包含该字符串。

### 正在下载日志{#aio-cli-download-logs}

Adobe I/O CLI允许使用[download-logs](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerdownload-logs-environmentid-service-name-days)命令从AEM下载日志作为Cloud Service。 这与从Cloud Manager Web UI下载日志的结束结果相同，区别在于`download-logs`命令根据请求的日志天数，跨天合并日志。

```
$ aio config:set cloudmanager_programid <PROGRAM ID>
$ aio cloudmanager:download-logs <ENVIRONMENT> <SERVICE> <NAME> <DAYS>
```

## 了解日志

作为Cloud Service登录AEM时，有多个窗格将日志语句写入其中。 由于多个AEM实例写入同一日志文件，因此了解如何分析和减少调试时的噪声非常重要。 要说明，将使用以下`aemerror`日志片段：

```
01.01.2020 12:00:00.000 [cm-p12345-e56789-aem-author-abcdefg-1111] *DEBUG* [qtp2078364989-269] com.example.components.impl.ExampleModelImpl Preparing to collect resources
01.01.2020 12:00:01.002 [cm-p12345-e56789-aem-author-abcdefg-2222] *WARN*  [qtp40782847611-87] com.example.services.impl.ExampleServiceImpl Unable to resolve resource [ /content/example ] to a resource. Aborting.
01.01.2020 12:00:02.003 [cm-p12345-e56789-aem-author-abcdefg-1111] *ERROR* [qtp2078364989-269] com.example.components.impl.ExampleModelImpl Unable to collect any resources
```

使用Pod Id（日期和时间之后的数据点），可以通过Pod或服务中的AEM实例整理日志，从而更轻松地跟踪和了解代码执行。

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

Adobe作为Cloud Service环境，对每个AEM的日志级别的一般指导是：

+ 本地开发(AEM SDK):`DEBUG`
+ 开发: `DEBUG`
+ 暂存: `WARN`
+ 生产: `ERROR`

为每个环境类型设置最合适的日志级别是以AEM作为Cloud Service，日志级别将保留在代码中

+ Java日志配置在OSGi配置中保持
+ 调度程序项目中的Apache Web服务器和调度程序日志级别

...因此，需要部署才能改变。

### 环境特定变量以设置Java日志级别

为每个环境设置静态的众所周知的Java日志级别的替代方法是使用AEM作为Cloud Service的[环境特定变量](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#environment-specific-configuration-values)来参数化日志级别，允许通过带有Cloud Manager插件](#aio-cli)的[Adobe I/OCLI动态更改这些值。

这需要更新日志记录OSGi配置以使用特定于环境的变量占位符。 [日志](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#default-values) 级别的默认值应根据Adobe建 [议设置](#log-levels)。例如：

`/apps/example/config/org.apache.sling.commons.log.LogManager.factory.config-example.cfg.json`

```
{
    "org.apache.sling.commons.log.names": ["com.example"],
    "org.apache.sling.commons.log.level": "$[env:LOG_LEVEL;default=DEBUG]"
}
```

这种方法有其弊端，必须加以考虑：

+ [允许有限数量的环境变量](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#number-of-variables)，创建用于管理日志级别的变量将使用一个变量。
+ 环境变量只能通过[Adobe I/O CLI](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerset-environment-variables-environmentid)或[ Cloud Manager HTTP API](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#cloud-manager-api-format-for-setting-properties)以编程方式进行管理。
+ 对环境变量所做的更改必须由支持的工具手动重置。 忘记将高流量环境（如生产）重置为较少冗余的日志级别可能会淹没日志并影响AEM性能。

_环境特定的变量不适用于Apache Web服务器或调度程序日志配置，因为这些变量未通过OSGi配置进行配置。_