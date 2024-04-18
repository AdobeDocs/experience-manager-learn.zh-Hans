---
title: 包含WAF规则的流量过滤器规则的示例和结果分析
description: 了解各种流量过滤器规则，包括WAF规则示例。 此外，如何使用AEMas a Cloud Service(AEMCS) CDN日志分析结果。
version: Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-10-26T00:00:00Z
jira: KT-13148
thumbnail: KT-13148.jpeg
exl-id: 49becbcb-7965-4378-bb8e-b662fda716b7
duration: 626
source-git-commit: 7f0f4d1b739cb63b96afc08eb31ab72a507c4722
workflow-type: tm+mt
source-wordcount: '1472'
ht-degree: 0%

---

# 包含WAF规则的流量过滤器规则的示例和结果分析

了解如何使用Adobe Experience Manager as a Cloud Service (AEMCS) CDN日志和仪表板工具声明各种类型的流量过滤器规则并分析结果。

在此部分中，您将探索流量过滤器规则的实际示例，包括WAF规则。 您将了解如何使用，根据URI（或路径）、IP地址、请求数量和不同的攻击类型来记录、允许和阻止请求。 [AEM WKND站点项目](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project).

此外，您将了解如何使用功能板工具来摄取AEMCS CDN日志，以通过Adobe提供的示例功能板可视化基本指标。

为了与您的特定要求保持一致，您可以增强和创建自定义功能板，从而获取更深入的见解并优化AEM站点的规则配置。

>[!VIDEO](https://video.tv.adobe.com/v/3425404?quality=12&learn=on)

## 示例

让我们探索各种流量过滤器规则的示例，包括WAF规则。 请确保您已完成之前的说明中要求的设置过程 [如何设置](./how-to-setup.md) 章节，而且您已克隆 [AEM WKND站点项目](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project).

### 记录请求

开始方式 **记录WKND登录和注销路径的请求** 针对AEM Publish服务。

- 将以下规则添加到WKND项目的 `/config/cdn.yaml` 文件。

```yaml
kind: CDN
version: '1'
metadata:
  envTypes:
    - dev
    - stage
    - prod
data:
  trafficFilters:
    rules:
    # On AEM Publish service log WKND Login and Logout requests
      - name: publish-auth-requests
        when:
          allOf:
            - reqProperty: tier
              matches: publish
            - reqProperty: path
              in:
                - /system/sling/login/j_security_check
                - /system/sling/logout
        action: log
```

- 提交更改并将其推送到Cloud Manager Git存储库。

- 使用Cloud Manager将更改部署到AEM开发环境 `Dev-Config` 配置管道 [先前创建](how-to-setup.md#deploy-rules-through-cloud-manager).

  ![Cloud Manager配置管道](./assets/cloud-manager-config-pipeline.png)

- 通过在Publish服务上登录并退出项目的WKND站点来测试规则(例如， `https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html`)。 您可以使用 `asmith/asmith` 作为用户名和密码。

  ![WKND登录](./assets/wknd-login.png)

#### 正在分析{#analyzing}

让我们来分析以下各项的结果 `publish-auth-requests` 通过从Cloud Manager下载AEMCS CDN日志并使用 [仪表板工具](how-to-setup.md#analyze-results-using-elk-dashboard-tool)，即您在前一章中设置的字段。

- 从 [Cloud Manager](https://my.cloudmanager.adobe.com/)的 **环境** 卡，下载AEMCS **Publish** 服务的CDN日志。

  ![Cloud Manager CDN日志下载](./assets/cloud-manager-cdn-log-downloads.png)

  >[!TIP]
  >
  >    新请求可能最多需要5分钟才能显示在CDN日志中。

- 复制下载的日志文件(例如， `publish_cdn_2023-10-24.log` （如下面的屏幕截图所示） `logs/dev` “弹性”操控板工具项目的文件夹。

  ![ELK工具日志文件夹](./assets/elk-tool-logs-folder.png){width="800" zoomable="yes"}

- 刷新弹性仪表板工具页面。
   - 在顶部 **全局筛选器** 部分，编辑 `aem_env_name.keyword` 筛选并选择 `dev` 环境值。

     ![ELK工具全局过滤器](./assets/elk-tool-global-filter.png)

   - 要更改时间间隔，请单击右上角的日历图标，然后选择所需的时间间隔。

     ![ELK工具时间间隔](./assets/elk-tool-time-interval.png)

- 查看已更新仪表板的  **分析的请求**， **已标记的请求**、和 **已标记的请求详细信息** 面板。 为了匹配CDN日志条目，它应显示每个条目的客户端IP (cli_ip)、主机、URL、操作(waf_action)和规则名称(waf_match)的值。

  ![铅笔工具仪表板](./assets/elk-tool-dashboard.png)


### 阻止请求

在本例中，我们来添加一个页面， _内部_ 路径下的文件夹 `/content/wknd/internal` 在已部署的WKND项目中。 然后声明一个流量过滤器规则，该规则 **阻止流量** 从与您的组织匹配的指定IP地址以外的任何位置（例如，公司VPN）的子页面。

您可以创建自己的内部页面(例如， `demo-page.html`)或使用 [附加的包](./assets/demo-internal-pages-package.zip).

- 在WKND项目的 `/config/cdn.yaml` 文件：

```yaml
kind: CDN
version: '1'
metadata:
  envTypes:
    - dev
    - stage
    - prod
data:
  trafficFilters:
    rules:
    ...

    # Block requests to (demo) internal only page/s from public IP address but allow from internal IP address.
    # Make sure to replace the IP address with your own IP address.
      - name: block-internal-paths
        when:
          allOf:
            - reqProperty: path
              matches: /content/wknd/internal
            - reqProperty: clientIp
              notIn: [192.150.10.0/24]
        action: block
```

- 提交更改并将其推送到Cloud Manager Git存储库。

- 使用将更改部署到AEM开发环境 [更早创建](how-to-setup.md#deploy-rules-through-cloud-manager) `Dev-Config` Cloud Manager中的配置管道。

- 通过访问WKND站点的内部页面测试规则，例如 `https://publish-pXXXX-eYYYY.adobeaemcloud.com/content/wknd/internal/demo-page.html` 或使用下面的CURL命令：

  ```bash
  $ curl -I https://publish-pXXXX-eYYYY.adobeaemcloud.com/content/wknd/internal/demo-page.html
  ```

- 根据规则中使用的IP地址，然后使用不同的IP地址（例如，使用手机）重复上述步骤。

#### 正在分析

要分析的结果，请执行以下操作 `block-internal-paths` 规则，请按照 [前面的示例](#analyzing).

但是，这次您应会看到 **阻止的请求** 以及客户端IP (cli_ip)、主机、URL、操作(waf_action)和规则名称(waf_match)列中的相应值。

![ELK工具仪表板阻止的请求](./assets/elk-tool-dashboard-blocked.png)


### 防御DoS攻击

让我们 **防御DoS攻击** 阻止来自IP地址的请求，每秒发出100个请求，导致其被阻止5分钟。

- 添加以下内容 [速率限制流量过滤器规则](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf.html#ratelimit-structure) 在WKND项目的 `/config/cdn.yaml` 文件。

```yaml
kind: CDN
version: '1'
metadata:
  envTypes:
    - dev
    - stage
    - prod
data:
  trafficFilters:
    rules:
    ...
    #  Prevent DoS attacks by blocking client for 5 minutes if they make more than 100 requests in 1 second.
      - name: prevent-dos-attacks
        when:
          reqProperty: path
          like: '*'
        rateLimit:
          limit: 100
          window: 1
          penalty: 300
          groupBy:
            - reqProperty: clientIp
        action: block
```

>[!WARNING]
>
>对于您的生产环境，请与Web安全团队协作，确定以下项的适当值： `rateLimit`，

- 如中所述，提交、推送和部署更改 [以前的示例](#logging-requests).

- 要模拟DoS攻击，请使用以下命令 [韦盖塔](https://github.com/tsenart/vegeta) 命令。

  ```shell
  $ echo "GET https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html" | vegeta attack -rate=120 -duration=5s | vegeta report
  ```

  此命令在5秒内发出120个请求并输出报告。 如您所见，成功率为32.5%；其余部分接收到406 HTTP响应代码，表明流量被阻止。

  ![Vegeta DoS攻击](./assets/vegeta-dos-attack.png)

#### 正在分析

要分析的结果，请执行以下操作 `prevent-dos-attacks` 规则，请按照 [前面的示例](#analyzing).

这一次，您应该会看到许多 **阻止的请求** 以及客户端IP (cli_ip)、主机、URL、操作(waf_action)和规则名称(waf_match)列中的相应值。

![学习工具功能板DoS请求](./assets/elk-tool-dashboard-dos.png)

此外， **按客户端IP、国家/地区和用户代理列出的100大攻击** 面板会显示其他详细信息，可用于进一步优化规则配置。

![ELK工具功能板DoS前100项请求](./assets/elk-tool-dashboard-dos-top-100.png)

有关如何防止DoS和DDoS攻击的更多信息，请参阅 [使用流量过滤规则阻止DoS和DDoS攻击](../dos/blocking-dos-attack-using-traffic-filter-rules.md) 教程。

### WAF规则

迄今为止，流量过滤器规则示例可以由所有Sites和Forms客户配置。

接下来，让我们探讨一下已购买增强安全性或WAF-DDoS保护许可证的客户的体验，该许可证允许他们配置高级规则以保护网站免受更复杂的攻击。

在继续操作之前，请按照流量过滤器规则文档中的说明，为程序启用WAF-DDoS保护 [设置步骤](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf.html?lang=en#setup).

#### 不带WAFFlags

让我们先看看在声明WAF规则之前的体验。 在您的程序上启用WAF-DDoS后，您的CDN会默认记录任何匹配的恶意流量，因此您拥有适当的信息来制定适当的规则。

让我们从攻击WKND站点开始，但不添加WAF规则(或使用 `wafFlags` 属性)并分析结果。

- 要模拟攻击，请使用 [Nikto](https://github.com/sullo/nikto) 命令中，它会在6分钟内发送约700个恶意请求。

  ```shell
  $ ./nikto.pl -useragent "AttackSimulationAgent (Demo/1.0)" -D V -Tuning 9 -ssl -h https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html
  ```

  ![Nikto攻击模拟](./assets/nikto-attack.png)

  要了解攻击模拟，请查看 [Nikto — 扫描调整](https://github.com/sullo/nikto/wiki/Scan-Tuning) 文档，该文档说明了如何指定要包含或排除的测试攻击的类型。

##### 正在分析

要分析攻击模拟的结果，请按照 [前面的示例](#analyzing).

但是，这次您应会看到 **已标记的请求** 以及客户端IP (cli_ip)、主机、URL、操作(waf_action)和规则名称(waf_match)列中的相应值。 此信息允许您分析结果并优化规则配置。

![ELK工具功能板WAF标记请求](./assets/elk-tool-dashboard-waf-flagged.png)

请注意 **WAF标记分发** 和 **热门攻击** 面板会显示其他详细信息，可用于进一步优化规则配置。

![ELK工具仪表板WAF标记攻击请求](./assets/elk-tool-dashboard-waf-flagged-top-attacks-1.png)

![ELK工具仪表板WAF热门攻击请求](./assets/elk-tool-dashboard-waf-flagged-top-attacks-2.png)


#### 带有WAFFlags

现在，让我们添加一个包含的WAF规则 `wafFlags` 属性作为 `action` 属性和 **阻止模拟的攻击请求**.

从语法的角度来看，WAF规则与前面所看到的规则相似，但是， `action` 属性引用一个或多个 `wafFlags` 值。 要了解有关 `wafFlags`，查看 [WAF标记列表](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf.html#waf-flags-list) 部分。

- 在WKND项目的 `/config/cdn.yaml` 文件。 请注意 `block-waf-flags` 规则包括一些在受到模拟恶意流量攻击时出现在仪表板工具中的wafFlags。 事实上，随着时间的推移，分析日志以决定随着威胁演进要宣布什么新规则是很好的做法。

```yaml
kind: CDN
version: '1'
metadata:
  envTypes:
    - dev
    - stage
    - prod
data:
  trafficFilters:
    rules:
    ...
    # Enable WAF protections (only works if WAF is enabled for your environment)
      - name: block-waf-flags
        when:
          reqProperty: tier
          matches: "author|publish"
        action:
          type: block
          wafFlags:
            - SANS
            - TORNODE
            - NOUA
            - SCANNER
            - USERAGENT
            - PRIVATEFILE
            - ABNORMALPATH
            - TRAVERSAL
            - NULLBYTE
            - BACKDOOR
            - LOG4J-JNDI
            - SQLI
            - XSS
            - CODEINJECTION
            - CMDEXE
            - NO-CONTENT-TYPE
            - UTF8
```

- 如中所述，提交、推送和部署更改 [以前的示例](#logging-requests).

- 要模拟攻击，请使用相同的攻击 [Nikto](https://github.com/sullo/nikto) 命令，跟以前一样。

  ```shell
  $ ./nikto.pl -useragent "AttackSimulationAgent (Demo/1.0)" -D V -Tuning 9 -ssl -h https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html
  ```

##### 正在分析

重复中描述的相同步骤 [前面的示例](#analyzing).

此时，您应该会看到条目位于 **阻止的请求** 以及客户端IP (cli_ip)、主机、url、操作(waf_action)和规则名称(waf_match)列中的相应值。

![ELK工具功能板WAF阻止的请求](./assets/elk-tool-dashboard-waf-blocked.png)

此外， **WAF标记分发** 和 **热门攻击** 面板显示其他详细信息。

![ELK工具仪表板WAF标记攻击请求](./assets/elk-tool-dashboard-waf-blocked-top-attacks-1.png)

![ELK工具仪表板WAF热门攻击请求](./assets/elk-tool-dashboard-waf-blocked-top-attacks-2.png)

### 综合分析

在上面 _分析_ 部分，您已了解如何使用仪表板工具分析特定规则的结果。 您可以进一步探索使用其他仪表板面板来分析结果，包括：


- 已分析、已标记和已阻止的请求
- WAF标记随时间变化的分布
- 随时间触发的流量过滤器规则
- 按WAF标志ID排在前面的攻击
- 顶级触发的流量过滤器
- 按客户端IP、国家/地区和用户代理划分的前100名攻击者

![ELK工具仪表板综合分析](./assets/elk-tool-dashboard-comprehensive-analysis-1.png)

![ELK工具仪表板综合分析](./assets/elk-tool-dashboard-comprehensive-analysis-2.png)


## 下一步

熟悉推荐内容 [最佳实践](./best-practices.md) 降低安全漏洞的风险。

## 其他资源

[流量过滤器规则语法](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf.html#rules-syntax)

[CDN日志格式](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf.html#cdn-log-format)
