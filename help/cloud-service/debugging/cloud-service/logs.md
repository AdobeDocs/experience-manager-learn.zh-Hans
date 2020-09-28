---
title: 日志
description: 日志充当调试AEM中AEM应用程序的前线Cloud Service，但依赖于部署的AEM应用程序中的适当日志记录。
feature: null
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5432
thumbnail: kt-5432.jpg
translation-type: tm+mt
source-git-commit: 1eb15af3d9d2904856218aaad4d5c52233603a71
workflow-type: tm+mt
source-wordcount: '925'
ht-degree: 1%

---


# 使用日志将AEM作为Cloud Service进行调试

日志充当调试AEM中AEM应用程序的前线Cloud Service，但依赖于部署的AEM应用程序中的适当日志记录。

给定环境的AEM服务（作者，发布／发布调度程序）的所有日志活动都整合到单个日志文件中，即使该服务中的不同窗格生成日志语句也是如此。

窗格ID在每个日志语句中提供，并允许过滤或整理日志语句。 窗格ID的格式为：

+ `cm-p<PROGRAM ID>-e<ENVIRONMENT ID>-aem-<author|publish>-<POD NAME>`
+ 示例: `cm-p12345-e56789-aem-author-abcdefabde-98765`

## AEM作者和发布服务日志

AEM作者服务和发布服务都提供AEM运行时服务器日志：

+ `aemerror` 是Java错误日志(位于AEM `/crx-quickstart/error.log` SDK本地快速启动中)。 以下是针对每个 [环境类型的自定](#log-levels) 义日志记录器的推荐日志级别：
   + 开发: `DEBUG`
   + 暂存: `WARN`
   + 生产: `ERROR`
+ `aemaccess` 列表发送给AEM服务的HTTP请求并提供详细信息
+ `aemrequest` 列表发往AEM服务的HTTP请求及其相应的HTTP响应

## AEM发布调度程序日志

只有AEM Publish Dispatcher提供Apache Web服务器和Dispatcher日志，因为这些方面仅存在于AEM Publish层中，而不存在于AEM Author层中。

+ `httpdaccess` 列表向AEM服务的Apache Web Server/Dispatcher发出的HTTP请求。
+ `httperror`  列表从Apache Web服务器记录消息，并帮助调试支持的Apache模块，如 `mod_rewrite`。
   + 开发: `DEBUG`
   + 暂存: `WARN`
   + 生产: `ERROR`
+ `aemdispatcher` 列表来自调度程序模块的日志消息，包括从缓存消息过滤和提供服务。
   + 开发: `DEBUG`
   + 暂存: `WARN`
   + 生产: `ERROR`

## Cloud Manager

Adobe云管理器允许通过环境的“下载日志”操作按日下载日志。

![云管理器——下载日志](./assets/logs/download-logs.png)

这些日志可通过任何日志分析工具进行下载和检查。

## AdobeI/O CLI（带有Cloud Manager插件）

Adobe云管理器支持通过Cloud ServiceI/O CLI以 [AdobeI/O CLI的形式](https://github.com/adobe/aio-cli) , [通过AdobeI/O CLI的Cloud Manager插件访问AEM作为日志](https://github.com/adobe/aio-cli-plugin-cloudmanager)。

首先， [使用Cloud Manager插件设置AdobeI/O](../../local-development-environment/development-tools.md#aio-cli)。

确保已识别相关的项目ID和环境ID，并使 [用列表可用日志选项](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerlist-available-log-options-environmentid) 列表用于跟踪或下载日志 [的日](#aio-cli-tail-logs) 志选 [项](#aio-cli-download-logs) 。

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

AdobeI/O CLI能够使用tail-logs命令从AEM作为Cloud Service实时跟踪 [日志](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagertail-log-environmentid-service-name) 。 跟踪功能对于在AEM上作为Cloud Service环境执行操作时监视实时日志活动很有用。

```
$ aio config:set cloudmanager_programid <PROGRAM ID>
$ aio cloudmanager:tail-logs <ENVIRONMENT ID> <SERVICE> <NAME>
```

其他命令行工具(如 `grep` 可与相关的 `tail-logs` 日志语句一起使用)，例如：

```
$ aio cloudmanager:tail-logs 12345 author | grep com.example.MySlingModel
```

...仅显示从中生成或包含 `com.example.MySlingModel` 该字符串的日志语句。

### 下载日志{#aio-cli-download-logs}

AdobeI/O CLI能够使用download-logs命令从AEM作为Cloud Service [下载日志](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerdownload-logs-environmentid-service-name-days)。 这与从Cloud Manager Web UI下载日志的结果相同，区别在于该命令根据请求的日 `download-logs` 志天数，跨天合并日志。

```
$ aio config:set cloudmanager_programid <PROGRAM ID>
$ aio cloudmanager:download-logs <ENVIRONMENT> <SERVICE> <NAME> <DAYS>
```

## 了解日志

作为Cloud Service登录AEM时，有多个窗格将日志语句写入其中。 由于多个AEM实例写入同一日志文件，因此了解调试时如何分析和减少噪声非常重要。 要说明，将使 `aemerror` 用以下日志片段：

```
01.01.2020 12:00:00.000 [cm-p12345-e56789-aem-author-abcdefg-1111] *DEBUG* [qtp2078364989-269] com.example.components.impl.ExampleModelImpl Preparing to collect resources
01.01.2020 12:00:01.002 [cm-p12345-e56789-aem-author-abcdefg-2222] *WARN*  [qtp40782847611-87] com.example.services.impl.ExampleServiceImpl Unable to resolve resource [ /content/example ] to a resource. Aborting.
01.01.2020 12:00:02.003 [cm-p12345-e56789-aem-author-abcdefg-1111] *ERROR* [qtp2078364989-269] com.example.components.impl.ExampleModelImpl Unable to collect any resources
```

使用Pod Id（日期和时间之后的数据点），日志可以由Pod或服务中的AEM实例进行整理，从而更容易跟踪和了解代码执行。

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

Adobe对AEM作为Cloud Service环境的日志级别的一般指导是：

+ 本地开发(AEM SDK): `DEBUG`
+ 开发: `DEBUG`
+ 暂存: `WARN`
+ 生产: `ERROR`

为每个环境类型设置最合适的日志级别是AEM作为Cloud Service，日志级别在代码中保持

+ Java日志配置在OSGi配置中得到维护
+ 调度程序项目中的Apache Web服务器和调度程序日志级别

...因此，需要部署才能改变。

### 环境特定变量以设置Java日志级别

为每个环境设置静态的已知Java日志级别的替代方法是使用AEM作为Cloud Service的特 [定变量](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#environment-specific-configuration-values) ，对日志级别进行参数化，允许通过带Cloud Manager插件 [的AdobeI/O CLI动态更改这些值](#aio-cli)。

这需要更新日志记录OSGi配置以使用环境特定的变量占位符。 [日志级](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#default-values) （日志级）的默认值应根据Adobe建 [议设置](#log-levels)。 例如：

`/apps/example/config/org.apache.sling.commons.log.LogManager.factory.config-example.cfg.json`

```
{
    "org.apache.sling.commons.log.names": ["com.example"],
    "org.apache.sling.commons.log.level": "$[env:LOG_LEVEL;default=DEBUG]"
}
```

这种方法的缺点是必须考虑：

+ [允许有限数量的环境变量](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#number-of-variables)，创建用于管理日志级别的变量将使用一个变量。
+ 环境变量只能通过 [AdobeI/O CLI](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerset-environment-variables-environmentid)[或Cloud Manager HTTP API以编程方式管理](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#cloud-manager-api-format-for-setting-properties)。
+ 对环境变量所做的更改必须由支持的工具手动重置。 忘记将高流量环境（如生产）重置为较详细的日志级别可能会淹没日志并影响AEM性能。

_环境特定变量对Apache Web服务器或调度程序日志配置无效，因为这些变量未通过OSGi配置进行配置。_