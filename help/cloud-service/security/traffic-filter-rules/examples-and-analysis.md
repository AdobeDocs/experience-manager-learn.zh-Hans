---
title: 包括 WAF 规则在内的流量过滤规则示例及结果分析
description: 了解各种流量过滤规则，包括 WAF 规则示例。此外，了解如何使用 AEM as a Cloud Service (AEMCS) CDN 日志分析结果。
version: Experience Manager as a Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-10-26T00:00:00Z
jira: KT-13148
thumbnail: KT-13148.jpeg
exl-id: 49becbcb-7965-4378-bb8e-b662fda716b7
duration: 532
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '1472'
ht-degree: 100%

---

# 包括 WAF 规则在内的流量过滤规则示例及结果分析

学习如何声明各种类型的流量过滤规则，并使用 Adobe Experience Manager as a Cloud Service (AEMCS) CDN 日志和仪表板工具来分析结果。

在本节中，您将会探索流量过滤规则的实际示例，其中包括 WAF 规则。您将学习如何使用 [AEM WKND Sites 项目](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project)，根据 URI（或路径）、IP 地址、请求数量和不同的攻击类型来记录、允许和阻止请求。

此外，您将了解如何使用仪表板工具。该工具可接收 AEMCS CDN 日志，并通过 Adobe 提供的示例仪表板来可视化关键量度。

为满足您的特定需求，您可以增强和创建自定义仪表板，从而获得更深入的见解，并优化 AEM 网站的规则配置。

>[!VIDEO](https://video.tv.adobe.com/v/3425404?quality=12&learn=on)

## 示例

让我们来探索各种流量过滤规则的示例，包括 WAF 规则。请确保您已完成之前[如何设置](./how-to-setup.md)章节中所述的必要设置流程，并且已克隆 [AEM WKND Sites 项目](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project)。

### 记录请求

首先，针对 AEM Publish 服务，开始&#x200B;**记录 WKND 登录和登出路径的请求**。

- 将以下规则添加到 WKND 项目的 `/config/cdn.yaml` 文件中。

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

- 将更改提交并推送到 Cloud Manager Git 存储库。

- 使用[之前创建的](how-to-setup.md#deploy-rules-through-cloud-manager) Cloud Manager `Dev-Config` 配置管道，将更改部署到 AEM Dev 环境中。

  ![Cloud Manager 配置管道](./assets/cloud-manager-config-pipeline.png)

- 通过在 Publish 服务上登录和登出您程序的 WKND 网站（例如，`https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html`）来测试该规则。您可以使用 `asmith/asmith` 作为用户名和密码。

  ![WKND 登录](./assets/wknd-login.png)

#### 分析{#analyzing}

让我们通过从 Cloud Manager 下载 AEMCS CDN 日志并使用上一章中设置的[仪表板工具](how-to-setup.md#analyze-results-using-elk-dashboard-tool)来分析 `publish-auth-requests` 规则的结果。

- 从[ Cloud Manager](https://my.cloudmanager.adobe.com/) 的&#x200B;**环境**&#x200B;卡片中，下载 AEMCS **Publish** 服务的 CDN 日志。

  ![Cloud Manager CDN 日志下载](./assets/cloud-manager-cdn-log-downloads.png)

  >[!TIP]
  >
  >    新请求可能需要长达 5 分钟才能出现在 CDN 日志中。

- 将下载的日志文件（例如下方截图中的 `publish_cdn_2023-10-24.log`）复制到 Elastic 仪表板工具项目的 `logs/dev` 文件夹中。

  ![ELK 工具日志文件夹](./assets/elk-tool-logs-folder.png){width="800" zoomable="yes"}

- 刷新 Elastic 仪表板工具页面。
   - 在顶部&#x200B;**全局过滤器**&#x200B;部分，编辑 `aem_env_name.keyword` 过滤器，并选择 `dev` 开发环境值。

     ![ELK 工具全局过滤器](./assets/elk-tool-global-filter.png)

   - 若要更改时间间隔，请点击右上角的日历图标，并选择所需的时间间隔。

     ![ELK 工具时间间隔](./assets/elk-tool-time-interval.png)

- 查看更新后的仪表板中的&#x200B;**已分析请求**、**已标记请求**&#x200B;以及&#x200B;**已标记请求详情**&#x200B;面板。对于匹配的 CDN 日志条目，它应显示每个条目的客户端 IP (cli_ip)、主机、URL、操作 (waf_action) 和规则名称 (waf_match) 的值。

  ![ELK 工具仪表板](./assets/elk-tool-dashboard.png)


### 阻止请求

在此示例中，我们将在已部署的 WKND 项目中，在路径为 `/content/wknd/internal` 的&#x200B;_内部_&#x200B;文件夹中添加一个页面。然后声明一个流量过滤规则，该规则会&#x200B;**阻止**&#x200B;来自与您的组织（例如，企业 VPN）匹配的指定 IP 地址以外的任何地方的子页面流量。

您可以创建自己的内部页面（例如，`demo-page.html`），也可以使用[附带的包](./assets/demo-internal-pages-package.zip)。

- 在 WKND 项目的 `/config/cdn.yaml` 文件中添加以下规则：

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

- 将更改提交并推送到 Cloud Manager Git 存储库。

- 使用[之前](how-to-setup.md#deploy-rules-through-cloud-manager)在 Cloud Manager 中创建的 `Dev-Config` 配置管道，将更改部署到 AEM Dev 环境中。

- 通过访问 WKND 网站的内部页面（例如 `https://publish-pXXXX-eYYYY.adobeaemcloud.com/content/wknd/internal/demo-page.html`）或使用下面的 CURL 命令来测试该规则：

  ```bash
  $ curl -I https://publish-pXXXX-eYYYY.adobeaemcloud.com/content/wknd/internal/demo-page.html
  ```

- 使用你在该规则中使用的 IP 地址和另一个不同的 IP 地址（例如，使用你的手机）重复上述步骤。

#### 分析

要分析 `block-internal-paths` 规则的结果，请按照[之前示例](#analyzing)中所述的相同步骤进行操作。

但是，这一次你应该能在客户端 IP (cli_ip)、主机、URL、操作 (waf_action) 和规则名称 (waf_match) 列中看到&#x200B;**被阻止的请求**&#x200B;及其对应的值。

![ELK 工具仪表板阻止的请求](./assets/elk-tool-dashboard-blocked.png)


### 防止 DoS 攻击

让我们通过阻止来自每秒发出 100 个请求的 IP 地址的请求，使其被阻止 5 分钟，从而&#x200B;**防止 DoS 攻击**。

- 在 WKND 项目的 `/config/cdn.yaml` 文件中添加以下[速率限制流量过滤规则](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf.html?lang=zh-Hans#ratelimit-structure)。

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
>对于生产环境，请与您的网络安全团队合作，确定 `rateLimit` 的合适取值，

- 如[前例](#logging-requests)所述，提交、推送并部署更改。

- 要模拟 DoS 攻击，请使用以下 [Vegeta](https://github.com/tsenart/vegeta) 命令。

  ```shell
  $ echo "GET https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html" | vegeta attack -rate=120 -duration=60s | vegeta report
  ```

  此命令在 5 秒内发出 120 个请求，并会输出报告。如您所见，成功率为 32.5%；其余请求均收到 406 HTTP 响应码，表明流量被阻止。

  ![Vegeta DoS 攻击](./assets/vegeta-dos-attack.png)

#### 分析

要分析 `prevent-dos-attacks` 规则的结果，请按照[之前示例](#analyzing)中所述的相同步骤进行操作。

这一次你应该能在客户端 IP (cli_ip)、主机、URL、操作 (waf_action) 和规则名称 (waf_match) 列中看到很多&#x200B;**被阻止的请求**&#x200B;及其对应的值。

![ELK 工具仪表板 DoS 请求](./assets/elk-tool-dashboard-dos.png)

此外，**按客户端 IP、国家和用户代理面板分类的前 100 个攻击**&#x200B;还展示了更多详细信息，这些信息可用于进一步优化规则配置。

![ELK 工具仪表板 DoS 前 100 个请求](./assets/elk-tool-dashboard-dos-top-100.png)

如需了解更多关于如何防范 DoS 和 DDoS 攻击的信息，请查阅[使用流量过滤规则阻止 DoS 和 DDoS 攻击](../blocking-dos-attack-using-traffic-filter-rules.md)教程。

### WAF 规则

到目前为止，流量过滤规则示例可由所有 Sites 和 Forms 客户进行配置。

接下来，让我们来了解一下购买了增强安全功能或 WAF-DDoS 防护许可证的客户的体验，该许可证允许他们配置高级规则，以保护网站免受更复杂的攻击。

在继续之前，请按照流量过滤规则文档中的[设置步骤](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf.html?lang=zh-Hans#setup)，为您的程序启用 WAF-DDoS 保护。

#### 无 WAFFlags

让我们先来看看在 WAF 规则宣布之前的情况。当您的程序启用 WAF-DDoS 时，默认情况下，您的 CDN 日志会记录所有恶意流量的匹配项，这样您就能获得准确的信息，从而制定合适的规则。

让我们在不添加 WAF 规则（或不使用 `wafFlags` 属性）的情况下，先对 WKND 网站进行攻击，并分析结果。

- 要模拟攻击，请使用下面的 [Nikto](https://github.com/sullo/nikto) 命令，该命令会在 6 分钟内发送约 700 个恶意请求。

  ```shell
  $ ./nikto.pl -useragent "AttackSimulationAgent (Demo/1.0)" -D V -Tuning 9 -ssl -h https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html
  ```

  ![Nikto 攻击模拟](./assets/nikto-attack.png)

  要了解攻击模拟，请查阅 [Nikto - 扫描调整](https://github.com/sullo/nikto/wiki/Scan-Tuning)文档，其中介绍了如何指定要包含或排除的测试攻击类型。

##### 分析

要分析模拟攻击的结果，请按照[之前示例](#analyzing)中所述的相同步骤进行操作。

但是，这一次你应该能在客户端 IP (cli_ip)、主机、URL、操作 (waf_action) 和规则名称 (waf_match) 列中看到&#x200B;**标记的请求**&#x200B;及其对应的值。这些信息有助于你分析结果并优化规则配置。

![ELK 工具仪表板 WAF 标记的请求](./assets/elk-tool-dashboard-waf-flagged.png)

请注意，**WAF 标记分布**&#x200B;和&#x200B;**热门攻击**&#x200B;面板展示了更多详细信息，这些信息可用于进一步优化规则配置。

![ELK 工具仪表板 WAF 标记攻击请求](./assets/elk-tool-dashboard-waf-flagged-top-attacks-1.png)

![ELK 工具仪表板 WAF 热门攻击请求](./assets/elk-tool-dashboard-waf-flagged-top-attacks-2.png)


#### 使用 WAFFlags

现在，让我们添加一个 WAF 规则，该规则包含作为 `action` 属性一部分的 `wafFlags` 属性，并&#x200B;**阻止模拟的攻击请求**。

从语法角度来看，WAF 规则与之前所见的规则相似，但 `action` 属性会引用一个或多个 `wafFlags` 值。要了解有关 `wafFlags` 的更多信息，请查阅 [WAF 标记列表](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf.html?lang=zh-Hans#waf-flags-list)部分。

- 在 WKND 项目的 `/config/cdn.yaml` 文件中添加以下规则。请注意，`block-waf-flags` 规则中包含了一些在模拟恶意流量攻击时出现在仪表板工具中的 wafFlags。实际上，随着威胁形势的发展，随着时间的推移，通过分析日志来确定需要声明哪些新规则是一种很好的做法。

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

- 如[前例](#logging-requests)所述，提交、推送并部署更改。

- 要模拟攻击，请使用与之前相同的 [Nikto](https://github.com/sullo/nikto) 命令。

  ```shell
  $ ./nikto.pl -useragent "AttackSimulationAgent (Demo/1.0)" -D V -Tuning 9 -ssl -h https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html
  ```

##### 分析

重复[之前示例](#analyzing)中描述的相同步骤。

这次你应该能在&#x200B;**被阻止的请求**&#x200B;下看到条目，并在客户端IP (cli_ip)、主机、URL、操作 (waf_action) 和规则名称 (waf_match) 列中看到相应的值。

![ELK 工具仪表板 WAF 被阻止的请求](./assets/elk-tool-dashboard-waf-blocked.png)

此外，**WAF 标志分布**&#x200B;和&#x200B;**热门攻击**&#x200B;面板还展示了更多详细信息。

![ELK 工具仪表板 WAF 标记攻击请求](./assets/elk-tool-dashboard-waf-blocked-top-attacks-1.png)

![ELK 工具仪表板 WAF 热门攻击请求](./assets/elk-tool-dashboard-waf-blocked-top-attacks-2.png)

### 综合分析

在上述&#x200B;_分析_&#x200B;部分中，您学习了如何使用仪表板工具分析特定规则的结果。您还可以使用其他仪表板面板进一步探索分析结果，包括：


- 已分析、已标记和已阻止的请求
- WAF 标记随时间分布
- 随时间触发的流量过滤规则
- 按 WAF 标志 ID 排序的主要攻击
- 最常触发的流量过滤器
- 按客户端 IP、国家/地区和用户代理排序的前 100 名攻击者

![ELK 工具仪表板综合分析](./assets/elk-tool-dashboard-comprehensive-analysis-1.png)

![ELK 工具仪表板综合分析](./assets/elk-tool-dashboard-comprehensive-analysis-2.png)


## 下一步

掌握推荐的[最佳实践](./best-practices.md)，以降低安全漏洞带来的风险。

## 其他资源

[流量过滤规则语法](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf.html?lang=zh-Hans#rules-syntax)

[CDN 日志格式](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf.html?lang=zh-Hans#cdn-log-format)

