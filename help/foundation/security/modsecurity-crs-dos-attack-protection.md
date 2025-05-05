---
title: 使用ModSecurity保护AEM站点免受DoS攻击
description: 了解如何使用OWASP ModSecurity核心规则集(CRS)启用ModSecurity以保护您的站点免受拒绝服务(DoS)攻击。
feature: Security
version: Experience Manager 6.5, Experience Manager as a Cloud Service
topic: Security, Development
role: Admin, Architect
level: Experienced
jira: KT-10385
thumbnail: KT-10385.png
doc-type: Article
last-substantial-update: 2023-08-18T00:00:00Z
exl-id: 9f689bd9-c846-4c3f-ae88-20454112cf9a
duration: 783
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1171'
ht-degree: 0%

---

# 使用ModSecurity保护AEM站点免受DoS攻击

了解如何使用Adobe Experience Manager (AEM) Publish Dispatcher上的&#x200B;**OWASP ModSecurity核心规则集(CRS)**&#x200B;启用ModSecurity以保护您的站点免受拒绝服务(DoS)攻击。


>[!VIDEO](https://video.tv.adobe.com/v/3452141?quality=12&learn=on&captions=chi_hans)

## 概述

[Open Web Application Security Project® (OWASP)](https://owasp.org/)基础提供了&#x200B;[**OWASP Top 10**](https://owasp.org/www-project-top-ten/)，概述了Web应用程序十大最关键的安全问题。

ModSecurity是一种开源、跨平台的解决方案，可针对针对Web应用程序的一系列攻击提供保护。 它还支持HTTP流量监控、日志记录和实时分析。

OWSAP®还提供[OWASP® ModSecurity核心规则集(CRS)](https://github.com/coreruleset/coreruleset)。 CRS是一组用于ModSecurity的通用&#x200B;**攻击检测**&#x200B;规则。 因此，CRS旨在保护Web应用程序免受各种攻击(包括OWASP十大攻击)，同时尽量减少虚假警报。

本教程演示如何启用和配置&#x200B;**DOS-PROTECTION** CRS规则以保护您的站点免受可能的DoS攻击。

>[!TIP]
>
>需要注意的是，AEM as a Cloud Service的[托管CDN](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn.html?lang=zh-Hans)满足了大多数客户的性能和安全要求。 但是， ModSecurity提供了额外的安全层，并允许使用特定于客户的规则和配置。

## 将CRS添加到Dispatcher项目模块

1. 下载并解压缩[最新的OWASP ModSecurity核心规则集](https://github.com/coreruleset/coreruleset/releases)。

   ```shell
   # Replace the X.Y.Z with relevent version numbers.
   $ wget https://github.com/coreruleset/coreruleset/archive/refs/tags/vX.Y.Z.tar.gz
   
   # For version v3.3.5 when this tutorial is published
   $ wget https://github.com/coreruleset/coreruleset/archive/refs/tags/v3.3.5.tar.gz
   
   # Extract the downloaded file
   $ tar -xvzf coreruleset-3.3.5.tar.gz
   ```

1. 在您的AEM项目代码的`dispatcher/src/conf.d/`中创建`modsec/crs`文件夹。 例如，在[AEM WKND Sites项目](https://github.com/adobe/aem-guides-wknd)的本地副本中。

   AEM项目代码中的![CRS文件夹 — ModSecurity](assets/modsecurity-crs/crs-folder-in-aem-dispatcher-module.png){width="200" zoomable="yes"}

1. 将下载的CRS发行包中的`coreruleset-X.Y.Z/rules`文件夹复制到`dispatcher/src/conf.d/modsec/crs`文件夹中。
1. 将下载的CRS发行包中的`coreruleset-X.Y.Z/crs-setup.conf.example`文件复制到`dispatcher/src/conf.d/modsec/crs`文件夹中，并将其重命名为`crs-setup.conf`。
1. 通过将所有CRS规则重命名为`XXXX-XXX-XXX.conf.disabled`，禁用从`dispatcher/src/conf.d/modsec/crs/rules`复制的所有CRS规则。 可以使用以下命令一次重命名所有文件。

   ```shell
   # Go inside the newly created rules directory within the dispathcher module
   $ cd dispatcher/src/conf.d/modsec/crs/rules
   
   # Rename all '.conf' extension files to '.conf.disabled'
   $ for i in *.conf; do mv -- "$i" "$i.disabled"; done
   ```

   请参阅WKND项目代码中重命名的CRS规则和配置文件。

   ![已禁用AEM项目代码中的CRS规则 — ModSecurity ](assets/modsecurity-crs/disabled-crs-rules.png){width="200" zoomable="yes"}

## 启用和配置拒绝服务(DoS)保护规则

要启用和配置拒绝服务(DoS)保护规则，请执行以下步骤：

1. 通过将`dispatcher/src/conf.d/modsec/crs/rules`文件夹中的`REQUEST-912-DOS-PROTECTION.conf.disabled`重命名为`REQUEST-912-DOS-PROTECTION.conf`（或从rulename扩展中删除`.disabled`）来启用DoS保护规则。
1. 通过定义&#x200B;**DOS_COUNTER_THRESHOLD、DOS_BURST_TIME_SLICE、DOS_BLOCK_TIMEOUT**&#x200B;变量来配置规则。
   1. 在`dispatcher/src/conf.d/modsec/crs`文件夹中创建`crs-setup.custom.conf`文件。
   1. 将以下规则片段添加到新创建的文件。

   ```
   # The Denial of Service (DoS) protection against clients making requests too quickly.
   # When a client is making more than 25 requests (excluding static files) within
   # 60 seconds, this is considered a 'burst'. After two bursts, the client is
   # blocked for 600 seconds.
   SecAction \
       "id:900700,\
       phase:1,\
       nolog,\
       pass,\
       t:none,\
       setvar:'tx.dos_burst_time_slice=60',\
       setvar:'tx.dos_counter_threshold=25',\
       setvar:'tx.dos_block_timeout=600'"    
   ```

在此示例规则配置中，**DOS_COUNTER_THRESHOLD**&#x200B;为25，**DOS_BURST_TIME_SLICE**&#x200B;为60秒，**DOS_BLOCK_TIMEOUT**&#x200B;超时为600秒。 此配置标识在60秒内出现的25个请求（不包括静态文件）中有两次以上符合DoS攻击条件，从而导致请求客户端被阻止600秒（或10分钟）。

>[!WARNING]
>
>要根据您的需求定义适当的值，请与Web安全团队协作。

## 初始化CRS

要初始化CRS，请删除常见的误报并为站点添加本地例外，请执行以下步骤：

1. 要初始化CRS，请从&#x200B;**REQUEST-901-INITIALIZATION**&#x200B;文件中删除`.disabled`。 换句话说，将`REQUEST-901-INITIALIZATION.conf.disabled`文件重命名为`REQUEST-901-INITIALIZATION.conf`。
1. 若要删除常见的误报，如本地IP (127.0.0.1) ping，请从&#x200B;**REQUEST-905-COMMON-EXCEPTIONS**&#x200B;文件中删除`.disabled`。
1. 要添加本地异常(如AEM平台或网站特定的路径)，请将`REQUEST-900-EXCLUSION-RULES-BEFORE-CRS.conf.example`重命名为`REQUEST-900-EXCLUSION-RULES-BEFORE-CRS.conf`
   1. 将特定于AEM平台的路径异常添加到新重命名的文件。

   ```
   ########################################################
   # AEM as a Cloud Service exclusions                    #
   ########################################################
   # Ignoring AEM-CS Specific internal and reserved paths
   
   SecRule REQUEST_URI "@beginsWith /systemready" \
       "id:1010,\
       phase:1,\
       pass,\
       nolog,\
       ctl:ruleEngine=Off"    
   
   SecRule REQUEST_URI "@beginsWith /system/probes" \
       "id:1011,\
       phase:1,\
       pass,\
       nolog,\
       ctl:ruleEngine=Off"
   
   SecRule REQUEST_URI "@beginsWith /gitinit-status" \
       "id:1012,\
       phase:1,\
       pass,\
       nolog,\
       ctl:ruleEngine=Off"
   
   ########################################################
   # ADD YOUR SITE related exclusions                     #
   ########################################################
   ...
   ```

1. 此外，从&#x200B;**REQUEST-910-IP-PRESSIBILITY.conf.disabled**&#x200B;中删除`.disabled`以进行IP信誉块检查，并从`REQUEST-949-BLOCKING-EVALUATION.conf.disabled`中删除异常分数检查。

>[!TIP]
>
>在AEM 6.5上配置时，请确保将上述路径替换为验证AEM运行状况的相应AMS或内部部署路径（也称为心率路径）。

## 添加ModSecurity Apache配置

要启用ModSecurity（又称`mod_security` Apache模块），请执行以下步骤：

1. 使用以下关键配置在`dispatcher/src/conf.d/modsec/modsecurity.conf`处创建`modsecurity.conf`。

   ```
   # Include the baseline crs setup
   Include conf.d/modsec/crs/crs-setup.conf
   
   # Include your customizations to crs setup if exist
   IncludeOptional conf.d/modsec/crs/crs-setup.custom.conf
   
   # Select all available CRS rules:
   #Include conf.d/modsec/crs/rules/*.conf
   
   # Or alternatively list only specific ones you want to enable e.g.
   Include conf.d/modsec/crs/rules/REQUEST-900-EXCLUSION-RULES-BEFORE-CRS.conf
   Include conf.d/modsec/crs/rules/REQUEST-901-INITIALIZATION.conf
   Include conf.d/modsec/crs/rules/REQUEST-905-COMMON-EXCEPTIONS.conf
   Include conf.d/modsec/crs/rules/REQUEST-910-IP-REPUTATION.conf
   Include conf.d/modsec/crs/rules/REQUEST-912-DOS-PROTECTION.conf
   Include conf.d/modsec/crs/rules/REQUEST-949-BLOCKING-EVALUATION.conf
   
   # Start initially with engine off, then switch to detection and observe, and when sure enable engine actions
   #SecRuleEngine Off
   #SecRuleEngine DetectionOnly
   SecRuleEngine On
   
   # Remember to use relative path for logs:
   SecDebugLog logs/httpd_mod_security_debug.log
   
   # Start with low debug level
   SecDebugLogLevel 0
   #SecDebugLogLevel 1
   
   # Start without auditing
   SecAuditEngine Off
   #SecAuditEngine RelevantOnly
   #SecAuditEngine On
   
   # Tune audit accordingly:
   SecAuditLogRelevantStatus "^(?:5|4(?!04))"
   SecAuditLogParts ABIJDEFHZ
   SecAuditLogType Serial
   
   # Remember to use relative path for logs:
   SecAuditLog logs/httpd_mod_security_audit.log
   
   # You might still use /tmp for temporary/work files:
   SecTmpDir /tmp
   SecDataDir /tmp
   ```

1. 从您的AEM项目的Dispatcher模块`dispatcher/src/conf.d/available_vhosts`中选择所需的`.vhost`，例如`wknd.vhost`，在`<VirtualHost>`块外添加以下条目。

   ```
   # Enable the ModSecurity and OWASP CRS
   <IfModule mod_security2.c>
       Include conf.d/modsec/modsecurity.conf
   </IfModule>
   
   ...
   
   <VirtualHost *:80>
       ServerName    "publish"
       ...
   </VirtualHost>
   ```

上述&#x200B;_ModSecurity CRS_&#x200B;和&#x200B;_DOS-PROTECTION_&#x200B;配置均可在AEM WKND Sites项目的[tutorial/enable-modsecurity-crs-dos-protection](https://github.com/adobe/aem-guides-wknd/tree/tutorial/enable-modsecurity-crs-dos-protection)分支中获取，以供您审阅。

### 验证Dispatcher配置

在使用AEM as a Cloud Service时，在部署&#x200B;_Dispatcher配置_&#x200B;更改之前，建议使用[AEM SDK的Dispatcher工具](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/dispatcher-tools.html?lang=zh-Hans)的`validate`脚本在本地验证这些更改。

```
# Go inside Dispatcher SDK 'bin' directory
$ cd <YOUR-AEM-SDK-DIR>/<DISPATCHER-SDK-DIR>/bin

# Validate the updated Dispatcher configurations
$ ./validate.sh <YOUR-AEM-PROJECT-CODE-DIR>/dispatcher/src
```

## 部署

使用Cloud Manager [Web层](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/configuring-production-pipelines.html?lang=zh-Hans&#web-tier-config)或[全栈栈](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/configuring-production-pipelines.html?lang=zh-Hans&#full-stack-code)管道部署本地验证的Dispatcher配置。 您还可以使用[快速开发环境](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/rde/overview.html?lang=zh-Hans)来加快周转时间。

## 验证

为了验证DoS保护，在此示例中，我们将在60秒的范围内发送超过50个请求（25个请求阈值乘以两次发生次数）。 但是，这些请求应通过AEM as a Cloud Service [内置](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn.html?lang=zh-Hans)或您网站前面的任何[其他CDN](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn.html?lang=zh-Hans&#point-to-point-CDN)。

实现CDN传递的一种技术是在每个网站页面请求&#x200B;**上添加一个查询参数，该参数具有**&#x200B;个新的随机值。

要在较短的时段（如60秒）内触发较多的请求（50或更多），可以使用Apache [JMeter](https://jmeter.apache.org/)或[Benchmark或ab工具](https://httpd.apache.org/docs/2.4/programs/ab.html)。

### 使用JMeter脚本模拟DoS攻击

要使用JMeter模拟DoS攻击，请执行以下步骤：

1. [下载Apache JMeter](https://jmeter.apache.org/download_jmeter.cgi)并在本地[安装](https://jmeter.apache.org/usermanual/get-started.html#install)
1. [使用`<JMETER-INSTALL-DIR>/bin`目录中的`jmeter`脚本在本地运行](https://jmeter.apache.org/usermanual/get-started.html#running)。
1. 使用&#x200B;**Open**&#x200B;工具菜单在JMeter中打开示例[WKND-DoS-Attack-Simulation-Test](assets/modsecurity-crs/WKND-DoS-Attack-Simulation-Test.jmx) JMX脚本。

   ![打开示例WKND DoS攻击JMX测试脚本 — ModSecurity](assets/modsecurity-crs/open-wknd-dos-attack-jmx-test-script.png)

1. 更新&#x200B;_主页_&#x200B;和&#x200B;_冒险页_ HTTP请求取样器中与您的测试AEM环境URL匹配的&#x200B;**服务器名称或IP**&#x200B;字段值。 查看示例JMeter脚本的其他详细信息。

   ![AEM服务器名称HTTP请求JMetere - ModSecurity](assets/modsecurity-crs/aem-server-name-http-request.png)

1. 按工具菜单中的&#x200B;**开始**&#x200B;按钮执行脚本。 该脚本针对WKND网站的&#x200B;_主页_&#x200B;和&#x200B;_冒险页面_&#x200B;发送50个HTTP请求（5个用户和10个循环计数）。 因此，它总共向非静态文件发出100个请求，从而符合每个&#x200B;**DOS-PROTECTION** CRS规则自定义配置的DoS攻击。

   ![执行JMeter脚本 — ModSecurity](assets/modsecurity-crs/execute-jmeter-script.png)

1. 表&#x200B;**JMeter侦听器中的**&#x200B;查看结果显示请求编号~53及更高版本的&#x200B;**失败**&#x200B;响应状态。

   在表JMeter - ModSecurity中![查看结果中的响应失败](assets/modsecurity-crs/failed-response-jmeter.png)

1. 为失败的请求返回&#x200B;**503 HTTP响应代码**，您可以使用&#x200B;**查看结果树** JMeter侦听器查看详细信息。

   ![503响应JMeter - ModSecurity](assets/modsecurity-crs/503-response-jmeter.png)

### 查看日志

ModSecurity记录器配置会记录DoS攻击事件的详细信息。 要查看详细信息，请执行以下步骤：

1. 下载并打开&#x200B;**发布Dispatcher**&#x200B;的`httpderror`日志文件。
1. 在日志文件中搜索单词`burst`，以查看&#x200B;**错误**&#x200B;行

   ```
   Tue Aug 15 15:19:40.229262 2023 [security2:error] [pid 308:tid 140200050567992] [cm-p46652-e1167810-aem-publish-85df5d9954-bzvbs] [client 192.150.10.209] ModSecurity: Warning. Operator GE matched 2 at IP:dos_burst_counter. [file "/etc/httpd/conf.d/modsec/crs/rules/REQUEST-912-DOS-PROTECTION.conf"] [line "265"] [id "912170"] [msg "Potential Denial of Service (DoS) Attack from 192.150.10.209 - # of Request Bursts: 2"] [ver "OWASP_CRS/3.3.5"] [tag "application-multi"] [tag "language-multi"] [tag "platform-multi"] [tag "paranoia-level/1"] [tag "attack-dos"] [tag "OWASP_CRS"] [tag "capec/1000/210/227/469"] [hostname "publish-p46652-e1167810.adobeaemcloud.com"] [uri "/content/wknd/us/en/adventures.html"] [unique_id "ZNuXi9ft_9sa85dovgTN5gAAANI"]
   
   ...
   
   Tue Aug 15 15:19:40.515237 2023 [security2:error] [pid 309:tid 140200051428152] [cm-p46652-e1167810-aem-publish-85df5d9954-bzvbs] [client 192.150.10.209] ModSecurity: Access denied with connection close (phase 1). Operator EQ matched 0 at IP. [file "/etc/httpd/conf.d/modsec/crs/rules/REQUEST-912-DOS-PROTECTION.conf"] [line "120"] [id "912120"] [msg "Denial of Service (DoS) attack identified from 192.150.10.209 (1 hits since last alert)"] [ver "OWASP_CRS/3.3.5"] [tag "application-multi"] [tag "language-multi"] [tag "platform-multi"] [tag "paranoia-level/1"] [tag "attack-dos"] [tag "OWASP_CRS"] [tag "capec/1000/210/227/469"] [hostname "publish-p46652-e1167810.adobeaemcloud.com"] [uri "/us/en.html"] [unique_id "ZNuXjAN7ZtmIYHGpDEkmmwAAAQw"]
   ```

1. 查看详细信息，如&#x200B;_客户端IP地址_、操作、错误消息和请求详细信息。

## ModSecurity的性能影响

启用ModSecurity和相关规则会对性能产生影响，因此请注意哪些规则是必需的、多余的以及已跳过。 与您的Web安全专家合作，共同启用和自定义CRS规则。

### 其他规则

本教程仅出于演示目的启用和自定义&#x200B;**DOS-PROTECTION** CRS规则。 建议与Web安全专家合作，了解、审查和配置适当的规则。
