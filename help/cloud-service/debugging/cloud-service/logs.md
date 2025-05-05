---
title: 日志
description: 日志是调试AEM as a Cloud Service中AEM应用程序的首选工具，但需要取决于已部署的AEM应用程序中是否有足够的日志记录。
feature: Developer Tools
version: Experience Manager as a Cloud Service
doc-type: Tutorial
jira: KT-5432
thumbnail: kt-5432.jpg
topic: Development
role: Developer
level: Beginner
exl-id: d0bd64bd-9e6c-4a28-a8d9-52bb37b27a09
duration: 229
source-git-commit: 0363505b426d6e4733c57409e17e9d69f7a567c7
workflow-type: tm+mt
source-wordcount: '962'
ht-degree: 1%

---

# 使用日志调试AEM as a Cloud Service

日志是调试AEM as a Cloud Service中AEM应用程序的首选工具，但需要取决于已部署的AEM应用程序中是否有足够的日志记录。

给定环境的AEM服务(创作、发布/发布Dispatcher)的所有日志活动都合并到一个日志文件中，即使该服务中的其他pod生成日志语句也是如此。

每个log语句中均提供面板ID，并允许过滤或整理log语句。 面板ID的格式为：

+ `cm-p<PROGRAM ID>-e<ENVIRONMENT ID>-aem-<author|publish>-<POD NAME>`
+ 示例：`cm-p12345-e56789-aem-author-abcdefabde-98765`

## 自定义日志文件

AEM as a Cloud Service不支持自定义日志文件，但它支持自定义日志记录。

为了使Java日志在AEM as a Cloud Service中可用(通过[Cloud Manager](#cloud-manager)或[Adobe I/O CLI](#aio))，必须将自定义日志语句写入`error.log`。 无法从AEM as a Cloud Service访问写入自定义命名日志（如`example.log`）的日志。

可以使用应用程序的`org.apache.sling.commons.log.LogManager.factory.config~example.cfg.json`文件中的Sling LogManager OSGi配置属性将日志写入`error.log`。

```
{
   ...
   "org.apache.sling.commons.log.file": "logs/error.log"
   ...
}
```

## AEM创作和发布服务日志

AEM创作和发布服务都提供AEM运行时服务器日志：

+ `aemerror`是Java错误日志(在AEM SDK本地快速入门上的`/crx-quickstart/logs/error.log`中找到)。 以下是每种环境类型自定义记录器的[推荐的日志级别](#log-levels)：
   + 开发： `DEBUG`
   + 阶段： `WARN`
   + 正式版：`ERROR`
+ `aemaccess`列出了对AEM服务的HTTP请求及详细信息
+ `aemrequest`列出了向AEM服务发出的HTTP请求及其对应的HTTP响应

## AEM发布Dispatcher日志

只有AEM Publish Dispatcher提供Apache Web Server和Dispatcher日志，因为这些方面仅存在于AEM发布层，而不存在于AEM创作层。

+ `httpdaccess`列出了向AEM服务的Apache Web Server/Dispatcher发出的HTTP请求。
+ `httperror`列出来自Apache Web Server的日志消息，并帮助调试支持的Apache模块，例如`mod_rewrite`。
   + 开发： `DEBUG`
   + 阶段： `WARN`
   + 正式版：`ERROR`
+ `aemdispatcher`列出来自Dispatcher模块的日志消息，包括从缓存消息中过滤和提供服务。
   + 开发： `DEBUG`
   + 阶段： `WARN`
   + 正式版：`ERROR`

## Cloud Manager{#cloud-manager}

Adobe Cloud Manager允许通过环境的“下载日志”操作，按天下载日志。

![Cloud Manager — 下载日志](./assets/logs/download-logs.png)

可以通过任何日志分析工具下载并检查这些日志。

## 带Cloud Manager插件的Adobe I/O CLI{#aio}

Adobe Cloud Manager支持使用适用于AEM as a Cloud Service CLI的[Adobe I/O插件](https://github.com/adobe/aio-cli-plugin-cloudmanager)，通过[Adobe I/O CLI](https://github.com/adobe/aio-cli)访问Cloud Manager日志。

首先，[使用Cloud Manager插件](../../local-development-environment/development-tools.md#aio-cli)设置Adobe I/O。

请确保已识别相关的项目ID和环境ID，并使用[list-available-log-options](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerlist-available-log-options-environmentid)列出用于[tail](#aio-cli-tail-logs)或[下载](#aio-cli-download-logs)日志的日志选项。

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

### 尾随日志{#aio-cli-tail-logs}

Adobe I/O CLI提供了使用[tail-logs](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagertail-log-environmentid-service-name)命令实时跟踪AEM as a Cloud Service中日志的功能。 在AEM as a Cloud Service环境中执行操作时，跟踪对于监视实时日志活动很有用。

```
$ aio config:set cloudmanager_programid <PROGRAM ID>
$ aio cloudmanager:tail-logs <ENVIRONMENT ID> <SERVICE> <NAME>
```

其他命令行工具（如`grep`）可以与`tail-logs`配合使用，以帮助隔离感兴趣的日志语句，例如：

```
$ aio cloudmanager:tail-logs 12345 author | grep com.example.MySlingModel
```

...仅显示从`com.example.MySlingModel`生成的或包含该字符串的日志语句。

### 正在下载日志{#aio-cli-download-logs}

Adobe I/O CLI允许使用[download-logs](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerdownload-logs-environmentid-service-name-days))命令从AEM as a Cloud Service下载日志。 这将提供与从Cloud Manager Web UI下载日志相同的最终结果，不同之处在于`download-logs`命令会根据请求的日志天数，跨天合并日志。

```
$ aio config:set cloudmanager_programid <PROGRAM ID>
$ aio cloudmanager:download-logs <ENVIRONMENT> <SERVICE> <NAME> <DAYS>
```

## 了解日志

AEM as a Cloud Service中的日志有多个Pod，用于将Log语句写入其中。 由于多个AEM实例会写入同一日志文件，因此了解如何在调试时分析和减少噪声非常重要。 为了说明，使用了以下`aemerror`日志片段：

```
01.01.2020 12:00:00.000 [cm-p12345-e56789-aem-author-abcdefg-1111] *DEBUG* [qtp2078364989-269] com.example.components.impl.ExampleModelImpl Preparing to collect resources
01.01.2020 12:00:01.002 [cm-p12345-e56789-aem-author-abcdefg-2222] *WARN*  [qtp40782847611-87] com.example.services.impl.ExampleServiceImpl Unable to resolve resource [ /content/example ] to a resource. Aborting.
01.01.2020 12:00:02.003 [cm-p12345-e56789-aem-author-abcdefg-1111] *ERROR* [qtp2078364989-269] com.example.components.impl.ExampleModelImpl Unable to collect any resources
```

使用Pod ID（日期和时间之后的数据点），可以由Pod或服务中的AEM实例整理日志，从而更容易跟踪和了解代码执行。

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

Adobe关于每个AEM as a Cloud Service环境的日志级别的一般指导方针是遵守AEM的默认日志设置（默认日志级别为`INFO`）。 Adobe还建议使用log语句检测自定义代码，以允许使用`INFO`的日志级别运行它。 日志级别在代码中进行维护

+ Java日志配置在OSGi配置中进行维护
+ Dispatcher项目中的Apache Web Server和Dispatcher日志级别

...因此，需要更改部署。

### 用于设置Java日志级别的特定于环境的变量

除了为每个环境设置静态的已知Java日志级别外，另一种方法是使用AEM as Cloud Service的[环境特定变量](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html?lang=zh-Hans#environment-specific-configuration-values)来参数化日志级别，从而允许通过带有Cloud Manager插件的[Adobe I/O CLI动态更改值](#aio-cli)。

这需要更新日志记录OSGi配置以使用特定于环境的变量占位符。 [&#128279;](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html?lang=zh-Hans#default-values)应根据[Adobe建议](#log-levels)设置日志级别的默认值。 例如：

`/apps/example/config/org.apache.sling.commons.log.LogManager.factory.config~example.cfg.json`

```
{
    ...
    "org.apache.sling.commons.log.names": ["com.example"],
    "org.apache.sling.commons.log.level": "$[env:LOG_LEVEL;default=DEBUG]"
    ...
}
```

这一方针的弊端必须被考虑在内：

+ [允许的环境变量数量有限](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html?lang=zh-Hans#number-of-variables)，创建用于管理日志级别的变量将使用一个。
+ 可通过[Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/environment-variables.html?lang=zh-Hans)、[Adobe I/O CLI](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerset-environment-variables-environmentid)和[Cloud Manager HTTP API](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html?lang=zh-Hans#cloud-manager-api-format-for-setting-properties)以编程方式管理环境变量。
+ 对环境变量的更改必须由支持的工具手动重置。 如果忘记将高流量环境（例如生产）重置为较不详细的日志级别，可能会淹没日志并影响AEM的性能。

_特定于环境的变量不适用于Apache Web Server或Dispatcher日志配置，因为这些配置未通过OSGi配置进行配置。_
