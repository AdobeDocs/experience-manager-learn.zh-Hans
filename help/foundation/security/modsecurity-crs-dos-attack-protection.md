---
title: 使用 ModSecurity 保护您的 AEM 网站免受 DoS 攻击
description: 了解如何启用 ModSecurity 来使用 OWASP ModSecurity 核心规则集 (CRS) 保护您的网站免受拒绝服务 (DoS) 攻击。
feature: Security
version: Experience Manager 6.5, Experience Manager as a Cloud Service
topic: Security, Development
role: Admin, Developer
level: Experienced
jira: KT-10385
thumbnail: KT-10385.png
doc-type: Article
last-substantial-update: 2023-08-18T00:00:00Z
exl-id: 9f689bd9-c846-4c3f-ae88-20454112cf9a
duration: 783
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '1171'
ht-degree: 100%

---

# 使用 ModSecurity 保护您的 AEM 网站免受 DoS 攻击

了解如何使用 Adobe Experience Manager (AEM) Publish Dispatcher 上的 **OWASP ModSecurity 核心规则集 (CRS)** 使 ModSecurity 可保护您的网站免受拒绝服务 (DoS) 攻击。


>[!VIDEO](https://video.tv.adobe.com/v/3452141?captions=chi_hans&quality=12&learn=on)

## 概述

[开放 Web 应用程序安全项目® (OWASP)](https://owasp.org/) 基金会发布了 [**OWASP Top 10**](https://owasp.org/www-project-top-ten/)，概述了 Web 应用程序的十大最关键安全问题。

ModSecurity 是一款开源的跨平台解决方案，能够为 Web 应用程序提供一系列攻击防护。此外，它还支持 HTTP 流量监控、日志记录和实时分析。

OWSAP® 还提供 [OWASP® ModSecurity 核心规则集 (CRS)](https://github.com/coreruleset/coreruleset)。CRS 是一套用于 ModSecurity 的通用&#x200B;**攻击检测**&#x200B;规则。因此，CRS 旨在保护 Web 应用程序免受各种攻击（包括 OWASP 十大攻击），同时还会尽量减少误报。

本教程将演示如何启用和配置 **DOS-PROTECTION** CRS 规则，以保护您的网站免受潜在的 DoS 攻击。

>[!TIP]
>
>值得注意的是，AEM as a Cloud Service 的[托管内容分发网络 (CDN) ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn.html?lang=zh-Hans)能够满足大多数客户的性能和安全需求。但是，ModSecurity 提供了额外的安全层，并允许客户自定义规则和配置。

## 将 CRS 添加到 Dispatcher 项目模块

1. 下载并提取[最新的 OWASP ModSecurity 核心规则集](https://github.com/coreruleset/coreruleset/releases)。

   ```shell
   # Replace the X.Y.Z with relevent version numbers.
   $ wget https://github.com/coreruleset/coreruleset/archive/refs/tags/vX.Y.Z.tar.gz
   
   # For version v3.3.5 when this tutorial is published
   $ wget https://github.com/coreruleset/coreruleset/archive/refs/tags/v3.3.5.tar.gz
   
   # Extract the downloaded file
   $ tar -xvzf coreruleset-3.3.5.tar.gz
   ```

1. 在 AEM 项目代码中创建`modsec/crs`文件夹`dispatcher/src/conf.d/`。例如，在 [AEM WKND Sites 项目](https://github.com/adobe/aem-guides-wknd)的本地副本中。

   ![AEM 项目代码中的 CRS 文件夹 - ModSecurity](assets/modsecurity-crs/crs-folder-in-aem-dispatcher-module.png){width="200" zoomable="yes"}

1. 将下载的 CRS 发布包中的 `coreruleset-X.Y.Z/rules` 文件夹复制到 `dispatcher/src/conf.d/modsec/crs` 文件夹中。
1. 将下载的 CRS 发布包中的 `coreruleset-X.Y.Z/crs-setup.conf.example` 文件复制到 `dispatcher/src/conf.d/modsec/crs` 文件夹中，并将其重命名为 `crs-setup.conf`。
1. 将 `dispatcher/src/conf.d/modsec/crs/rules` 目录下所有复制的 CRS 规则，通过重命名为 `XXXX-XXX-XXX.conf.disabled` 的形式全部禁用。您可以使用以下命令一次重命名所有文件。

   ```shell
   # Go inside the newly created rules directory within the dispathcher module
   $ cd dispatcher/src/conf.d/modsec/crs/rules
   
   # Rename all '.conf' extension files to '.conf.disabled'
   $ for i in *.conf; do mv -- "$i" "$i.disabled"; done
   ```

   请参阅 WKND 项目代码中重命名的 CRS 规则和配置文件。

   ![在 AEM 项目代码中禁用 CRS 规则 - ModSecurity ](assets/modsecurity-crs/disabled-crs-rules.png){width="200" zoomable="yes"}

## 启用并配置拒绝服务 (DoS) 保护规则

要启用和配置拒绝服务 (DoS) 保护规则，请按照以下步骤操作：

1. 通过将 `REQUEST-912-DOS-PROTECTION.conf.disabled` 重命名为 `REQUEST-912-DOS-PROTECTION.conf`（或将 `.disabled`从规则名称扩展中移除）来启用 DoS 保护规则。`dispatcher/src/conf.d/modsec/crs/rules`
1. 通过定义 **DOS_COUNTER_THRESHOLD、DOS_BURST_TIME_SLICE和DOS_BLOCK_TIMEOUT** 变量来配置规则。
   1. 在 `dispatcher/src/conf.d/modsec/crs` 文件夹中创建一个 `crs-setup.custom.conf` 文件。
   1. 将以下规则片段添加到新创建的文件中。

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

在此示例规则配置中，**DOS_COUNTER_THRESHOLD** 为 25，**DOS_BURST_TIME_SLICE** 为 60 秒，**DOS_BLOCK_TIMEOUT** 超时时间为 600 秒。此配置将 60 秒内出现两次以上（不包括静态文件）的 25 个请求视为 DoS 攻击，导致请求客户端被阻塞 600 秒（即 10 分钟）。

>[!WARNING]
>
>要为您的需求确定合适的值，请与您的 Web 安全团队进行协作。

## 初始化 CRS

要初始化 CRS，请先排除常见的误报，然后为您的网站添加本地例外情况，具体步骤如下：

1. 要初始化 CRS，请从 **REQUEST-901-INITIALIZATION** 文件中移除 `.disabled`。 换句话说，将 `REQUEST-901-INITIALIZATION.conf.disabled` 文件重命名为 `REQUEST-901-INITIALIZATION.conf`。
1. 要消除常见的误报，如本地 IP (127.0.0.1) 的 ping 操作，请从 **REQUEST-905-COMMON-EXCEPTIONS** 文件中移除 `.disabled`。
1. 要添加本地例外情况（如 AEM 平台或网站特定路径），请将 `REQUEST-900-EXCLUSION-RULES-BEFORE-CRS.conf.example` 重命名为 `REQUEST-900-EXCLUSION-RULES-BEFORE-CRS.conf`
   1. 将特定于 AEM 平台的路径例外情况添加到新重命名的文件中。

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

1. 此外，请删除 **REQUEST-910-IP-REPUTATION.conf.disabled**（用于 IP 信誉阻止检查）和 `REQUEST-949-BLOCKING-EVALUATION.conf.disabled` 中的 `.disabled`。

>[!TIP]
>
>在 AEM 6.5 上进行配置时，请确保将上述路径替换为相应的 AMS 路径或本地路径，以验证 AEM 的运行状况（即心跳路径）。

## 添加 ModSecurity Apache 配置

要启用 ModSecurity（又名 `mod_security` Apache 模块），请按照以下步骤操作：

1. 使用以下关键配置在 `dispatcher/src/conf.d/modsec/modsecurity.conf` 创建 `modsecurity.conf`。

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

1. 从 AEM 项目的 Dispatcher 模块 `dispatcher/src/conf.d/available_vhosts` 中选择所需的 `.vhost`，例如 `wknd.vhost`，并在 `<VirtualHost>` 区块外添加以下条目。

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

上述所有 _ModSecurity CRS_ 和 _DOS-PROTECTION_ 配置均可在 AEM WKND Sites 项目的 [tutorial/enable-modsecurity-crs-dos-protection](https://github.com/adobe/aem-guides-wknd/tree/tutorial/enable-modsecurity-crs-dos-protection) 分支上查看。

### 验证 Dispatcher 配置

在使用 AEM as a Cloud Service 时，在部署 _Dispatcher 配置_&#x200B;更改之前，建议使用 [AEM SDK 的 Dispatcher 工具](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/dispatcher-tools.html?lang=zh-Hans)的 `validate` 脚本进行本地验证。

```
# Go inside Dispatcher SDK 'bin' directory
$ cd <YOUR-AEM-SDK-DIR>/<DISPATCHER-SDK-DIR>/bin

# Validate the updated Dispatcher configurations
$ ./validate.sh <YOUR-AEM-PROJECT-CODE-DIR>/dispatcher/src
```

## 部署

使用 Cloud Manager [Web 层](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/configuring-production-pipelines.html?lang=zh-Hans&#web-tier-config)或[全栈](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/configuring-production-pipelines.html?lang=zh-Hans&#full-stack-code)管道部署本地验证的 Dispatcher 配置。您也可以使用[快速开发环境](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/rde/overview.html?lang=zh-Hans)，以缩短周转时间。

## 验证

为了验证 DoS 保护，在此示例中，我们将在 60 秒内发送超过 50 个请求（25 个请求阈值乘以两次出现）。但是，这些请求应通过 AEM as a Cloud Service 的[内置](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn.html?lang=zh-Hans)功能，或任何作为你网站前端的[其他 CDN](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn.html?lang=zh-Hans&#point-to-point-CDN) 进行传递。

实现 CDN 透传的一种方法是&#x200B;**在每个网站页面请求中添加一个带有新的随机值**&#x200B;的查询参数。

若要在短时间内（如 60 秒内）触发大量请求（50 个或更多），可使用 Apache [JMeter](https://jmeter.apache.org/)、[Benchmark 或 ab 工具](https://httpd.apache.org/docs/2.4/programs/ab.html)。

### 使用 JMeter 脚本模拟 DoS 攻击

要使用 JMeter 模拟 DoS 攻击，请按照以下步骤操作：

1. [下载 Apache JMeter](https://jmeter.apache.org/download_jmeter.cgi) 并在本地进行[安装](https://jmeter.apache.org/usermanual/get-started.html#install)
1. 使用位于 `<JMETER-INSTALL-DIR>/bin` 目录下的 `jmeter` 脚本在本地[运行](https://jmeter.apache.org/usermanual/get-started.html#running)它。
1. 使用&#x200B;**打开**&#x200B;工具菜单，将示例 [WKND-DoS-Attack-Simulation-Test](assets/modsecurity-crs/WKND-DoS-Attack-Simulation-Test.jmx) JMX 脚本导入 JMeter。

   ![打开示例 WKND DoS 攻击 JMX 测试脚本 - ModSecurity](assets/modsecurity-crs/open-wknd-dos-attack-jmx-test-script.png)

1. 在&#x200B;_主页_&#x200B;和&#x200B;_冒险页面_ HTTP 请求采样器中更新&#x200B;**服务器名称或 IP** 字段值，使其与您的测试 AEM 环境 URL 相匹配。检查 JMeter 脚本样本的其他详细信息。

   ![AEM 服务器名称 HTTP 请求 JMetere - ModSecurity](assets/modsecurity-crs/aem-server-name-http-request.png)

1. 通过点击工具菜单中的&#x200B;**开始**&#x200B;按钮来执行脚本。该脚本向 WKND 网站的&#x200B;_主页_&#x200B;和&#x200B;_冒险页面_&#x200B;发送 50 个 HTTP 请求（5 个用户和 10 次循环计数）。因此，总共向非静态文件发送了 100 个请求，符合 **DOS-PROTECTION** CRS 规则自定义配置中的 DoS 攻击条件。

   ![执行 JMeter 脚本 - ModSecurity](assets/modsecurity-crs/execute-jmeter-script.png)

1. JMeter 的&#x200B;**View Results in Table** 监听器显示，从第约 53 个请求开始，其响应状态为&#x200B;**失败**。

   ![JMeter 的“View Results in Table”内的失败响应 - ModSecurity](assets/modsecurity-crs/failed-response-jmeter.png)

1. 对于失败的请求，会返回 **503 HTTP 响应码**，您可以使用 JMeter 的 **View Results Tree** 监听器查看详细信息。

   ![503 响应 JMeter - ModSecurity](assets/modsecurity-crs/503-response-jmeter.png)

### 审查日志

ModSecurity 日志配置会记录 DoS 攻击事件的详细信息。要查看这些详细信息，请按照以下步骤操作：

1. 下载并打开&#x200B;**发布 Dispatcher** 的 `httpderror` 日志文件。
1. 在日志文件中搜索单词 `burst`，以查看&#x200B;**错误**&#x200B;行

   ```
   Tue Aug 15 15:19:40.229262 2023 [security2:error] [pid 308:tid 140200050567992] [cm-p46652-e1167810-aem-publish-85df5d9954-bzvbs] [client 192.150.10.209] ModSecurity: Warning. Operator GE matched 2 at IP:dos_burst_counter. [file "/etc/httpd/conf.d/modsec/crs/rules/REQUEST-912-DOS-PROTECTION.conf"] [line "265"] [id "912170"] [msg "Potential Denial of Service (DoS) Attack from 192.150.10.209 - # of Request Bursts: 2"] [ver "OWASP_CRS/3.3.5"] [tag "application-multi"] [tag "language-multi"] [tag "platform-multi"] [tag "paranoia-level/1"] [tag "attack-dos"] [tag "OWASP_CRS"] [tag "capec/1000/210/227/469"] [hostname "publish-p46652-e1167810.adobeaemcloud.com"] [uri "/content/wknd/us/en/adventures.html"] [unique_id "ZNuXi9ft_9sa85dovgTN5gAAANI"]
   
   ...
   
   Tue Aug 15 15:19:40.515237 2023 [security2:error] [pid 309:tid 140200051428152] [cm-p46652-e1167810-aem-publish-85df5d9954-bzvbs] [client 192.150.10.209] ModSecurity: Access denied with connection close (phase 1). Operator EQ matched 0 at IP. [file "/etc/httpd/conf.d/modsec/crs/rules/REQUEST-912-DOS-PROTECTION.conf"] [line "120"] [id "912120"] [msg "Denial of Service (DoS) attack identified from 192.150.10.209 (1 hits since last alert)"] [ver "OWASP_CRS/3.3.5"] [tag "application-multi"] [tag "language-multi"] [tag "platform-multi"] [tag "paranoia-level/1"] [tag "attack-dos"] [tag "OWASP_CRS"] [tag "capec/1000/210/227/469"] [hostname "publish-p46652-e1167810.adobeaemcloud.com"] [uri "/us/en.html"] [unique_id "ZNuXjAN7ZtmIYHGpDEkmmwAAAQw"]
   ```

1. 查看&#x200B;_客户端 IP 地址_、操作、错误消息和请求详情等详细信息。

## ModSecurity 的性能影响

启用 ModSecurity 及其相关规则会对性能产生一定影响，因此需注意哪些规则是必需的、哪些是多余的、哪些可以跳过。请与您的 Web 安全专家合作，以启用并自定义 CRS 规则。

### 其他规则

本教程仅出于演示目的启用并自定义 **DOS-PROTECTION** CRS 规则。建议与网络安全专家合作，以了解、审查和配置适当的规则。
