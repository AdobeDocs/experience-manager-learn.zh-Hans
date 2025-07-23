---
title: 如何设置流量过滤规则（包括 WAF 规则）
description: 了解如何进行设置，以创建、部署、测试和分析流量过滤规则（包括 WAF 规则）的结果。
version: Experience Manager as a Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2025-06-04T00:00:00Z
jira: KT-18306
thumbnail: null
exl-id: 0a738af8-666b-48dc-8187-9b7e6a8d7e1b
source-git-commit: b7f567da159865ff04cb7e9bd4dae0b140048e7d
workflow-type: ht
source-wordcount: '1125'
ht-degree: 100%

---

# 如何设置流量过滤规则（包括 WAF 规则）

了解&#x200B;**如何设置**&#x200B;流量过滤规则，包括 Web 应用程序防火墙（WAF）规则。在本教程中，我们将为后续教程奠定基础，您将配置并部署规则，随后进行测试并分析结果。

为演示设置过程，本教程使用 [AEM WKND Sites 项目](https://github.com/adobe/aem-guides-wknd)。

>[!VIDEO](https://video.tv.adobe.com/v/3469395/?quality=12&learn=on)

## 设置概述

为后续教程奠定基础的准备工作包括以下几个步骤：

- 在您的 AEM 项目中的 `config` 文件夹内&#x200B;_创建规则_
- _部署规则_：通过 Adobe Cloud Manager 配置管道进行部署。
- _测试规则_：使用 Curl、Vegeta 和 Nikto 等工具进行验证
- _分析结果_：使用 AEMCS CDN 日志分析工具进行分析

## 在 AEM 项目中创建规则

要在您的 AEM 项目中定义&#x200B;**标准**&#x200B;和 **WAF** 流量过滤规则，请按照以下步骤操作：

1. 在 AEM 项目的顶层目录下创建一个名为 `config` 的文件夹。

2. 在 `config` 文件夹内创建一个名为 `cdn.yaml` 的文件。

3. 在 `cdn.yaml` 中使用以下元数据结构：

```yaml
kind: "CDN"
version: "1"
metadata:
  envTypes: ["dev", "stage", "prod"]
data:
  trafficFilters:
    rules:
```

![WKND AEM 项目规则文件和文件夹](./assets/setup/wknd-rules-file-and-folder.png)

在[下一篇教程](#next-steps)中，您将学习如何将 Adobe **推荐的标准流量过滤规则和 WAF 规则**&#x200B;添加到该文件中，为您的实施打下坚实基础。

## 使用 Adobe Cloud Manager 部署规则

为部署规则做好准备，请按照以下步骤操作：

1. 登录 [my.cloudmanager.adobe.com](https://my.cloudmanager.adobe.com/) 并选择您的项目。

2. 在&#x200B;**项目概览**&#x200B;页面中找到&#x200B;**管道**&#x200B;卡片，点击 **+添加**&#x200B;以创建一个新的管道。

   ![Cloud Manager 管道信息卡](./assets/setup/cloud-manager-pipelines-card.png)

3. 在管道向导中：

   - **类型**：部署管道
   - **管道名称**：Dev-Config

   ![Cloud Manager 配置管道对话框](./assets/setup/cloud-manager-config-pipeline-step1-dialog.png)

4. 源代码配置：

   - **要部署的代码**：有针对性的部署
   - **包括**：配置
   - **部署环境**：例如，`wknd-program-dev`
   - **存储库**：Git 存储库（例如，`wknd-site`）
   - **Git 分支**：您的工作分支
   - **代码位置**：`/config`

   ![Cloud Manager 配置管道对话框](./assets/setup/cloud-manager-config-pipeline-step2-dialog.png)

5. 检查管道配置，然后点击&#x200B;**保存**。

在[下一篇教程](#next-steps)中，您将学习如何将该管道部署到您的 AEM 环境中。

## 使用工具测试规则

要验证标准流量过滤规则和 WAF 规则的有效性，您可以使用多种工具模拟请求，并分析规则的响应效果。

请确认您的本地机器上已安装以下工具，或按照说明进行安装：

- [Curl](https://curl.se/)：用于测试请求/响应流程。
- [Vegeta](https://github.com/tsenart/vegeta)：用于模拟高请求负载（DoS 测试）。
- [Nikto](https://github.com/sullo/nikto/wiki)：用于扫描漏洞。

您可以通过以下命令验证是否已成功安装：

```shell
# Curl version check
$ curl --version

# Vegeta version check
$ vegeta -version

# Nikto version check
$ cd <PATH-OF-CLONED-REPO>/program
$ ./nikto.pl -Version
```

在[下一篇教程](#next-steps)中，您将学习如何使用这些工具模拟高请求负载和恶意请求，以测试流量过滤规则和 WAF 规则的有效性。

## 分析结果

为结果分析做好准备，请按以下步骤操作：

1. 安装 **AEMCS CDN 日志分析工具**，通过预建的仪表板查看并分析模式。

2. 从 Cloud Manager 用户界面下载日志，完成 **CDN 日志摄入**。或者，您也可以将日志直接转发至受支持的托管日志平台，例如 Splunk 或 Elasticsearch。

### AEMCS CDN 日志分析工具

要分析流量过滤规则和 WAF 规则的执行效果，您可以使用 **AEMCS CDN 日志分析工具**。该工具使用从 AEMCS CDN 收集的日志数据，提供预建仪表板帮助您查看 CDN 流量和 WAF 活动情况。

AEMCS CDN 日志分析工具支持两种可观测性平台：**ELK**（Elasticsearch、Logstash、Kibana）和 **Splunk**。

您可以通过日志转发功能（Log Forwarding）将日志流式传输至托管的 ELK 或 Splunk 服务，您可以在这些平台上安装仪表板，以查看和分析标准流量过滤规则与 WAF 流量过滤规则的效果。不过在本教程中，您将在您的计算机本地安装的一个 ELK 实例中设置仪表板。

1. 克隆 [AEMCS-CDN-Log-Analysis-Tooling](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling) 存储库。

2. 按照 [ELK Docker 容器设置指南](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling/blob/main/ELK/README.md)在本地安装并配置 ELK 堆栈。

3. 通过 ELK 仪表板，您可以查看诸如 IP 请求量、被阻止的流量、URI 模式及安全警报等关键指标。

   ![ELK 流量过滤规则仪表板](./assets/setup/elk-dashboard.png)

>[!NOTE]
> 
> 如果尚未从 AEMCS CDN 摄入日志，仪表板将显示为空。

### CDN 日志摄入

要将 CDN 日志摄入 ELK 堆栈，请按以下步骤操作：

- 从[ Cloud Manager](https://my.cloudmanager.adobe.com/) 的&#x200B;**环境**&#x200B;卡片中，下载 AEMCS **Publish** 服务的 CDN 日志。

  ![Cloud Manager CDN 日志下载](./assets/setup/cloud-manager-cdn-log-downloads.png)

  >[!TIP]
  >
  > 新请求可能需要长达 5 分钟才能出现在 CDN 日志中。

- 将下载的日志文件（例如下方截图中的 `publish_cdn_2025-06-06.log`）复制到 Elastic 仪表板工具项目的 `logs/dev` 文件夹中。

  ![ELK 工具日志文件夹](./assets/setup/elk-tool-logs-folder.png){width="800" zoomable="yes"}

- 刷新 Elastic 仪表板工具页面。
   - 在顶部&#x200B;**全局过滤器**&#x200B;部分，编辑 `aem_env_name.keyword` 过滤器，并选择 `dev` 开发环境值。

     ![ELK 工具全局过滤器](./assets/setup/elk-tool-global-filter.png)

   - 若要更改时间间隔，请点击右上角的日历图标，并选择所需的时间间隔。

- 在[下一篇教程中](#next-steps)，您将学习如何使用 ELK 堆栈中的预建仪表板分析标准流量过滤规则和 WAF 流量过滤规则的执行结果。

  ![ELK 工具预建仪表板](./assets/setup/elk-tool-pre-built-dashboards.png)

## 摘要

您已成功完成在 AEM as a Cloud Service 中实施流量过滤规则（包括 WAF 规则）的准备工作。您已创建了一个配置文件结构和进行部署的管道，并准备了用于测试与分析结果的相关工具。

## 后续步骤

接下来，请通过以下教程学习如何实施 Adobe 推荐的规则：

<!-- CARDS
{target = _self}

* ./use-cases/using-traffic-filter-rules.md
  {title = Protecting AEM websites using standard traffic filter rules}
  {description = Learn how to protect AEM websites from DoS, DDoS and bot abuse using Adobe-recommended standard traffic filter rules in AEM as a Cloud Service.}
  {image = ./assets/use-cases/using-traffic-filter-rules.png}
  {cta = Apply Rules}

* ./use-cases/using-waf-rules.md
  {title = Protecting AEM websites using WAF traffic filter rules}
  {description = Learn how to protect AEM websites from sophisticated threats including DoS, DDoS, and bot abuse using Adobe-recommended Web Application Firewall (WAF) traffic filter rules in AEM as a Cloud Service.}
  {image = ./assets/use-cases/using-waf-rules.png}
  {cta = Activate WAF}
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Protecting AEM websites using standard traffic filter rules">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./use-cases/using-traffic-filter-rules.md" title="使用标准流量过滤规则保护 AEM 网站" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/use-cases/using-traffic-filter-rules.png" alt="使用标准流量过滤规则保护 AEM 网站"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./use-cases/using-traffic-filter-rules.md" target="_self" rel="referrer" title="使用标准流量过滤规则保护 AEM 网站">使用标准流量过滤规则保护 AEM 网站</a>
                    </p>
                    <p class="is-size-6">了解如何在 AEM as a Cloud Service 中使用 Adobe 推荐的标准流量过滤规则防护 AEM 网站抵御 DoS、DDoS 攻击以及机器人滥用。</p>
                </div>
                <a href="./use-cases/using-traffic-filter-rules.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">应用规则</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Protecting AEM websites using WAF traffic filter rules">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./use-cases/using-waf-rules.md" title="使用 WAF 流量过滤规则保护 AEM 网站" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/use-cases/using-waf-rules.png" alt="使用 WAF 流量过滤规则保护 AEM 网站"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./use-cases/using-waf-rules.md" target="_self" rel="referrer" title="使用 WAF 流量过滤规则保护 AEM 网站">使用 WAF 流量过滤规则保护 AEM 网站</a>
                    </p>
                    <p class="is-size-6">了解如何在 AEM as a Cloud Service 中使用 Adobe 推荐的 Web 应用程序防火墙（WAF）流量过滤规则，防御包括 DoS、DDoS 和机器人滥用在内的复杂安全威胁。</p>
                </div>
                <a href="./use-cases/using-waf-rules.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">启用 WAF</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->

## 高级用例

除了 Adobe 推荐的标准流量过滤规则和 WAF 规则之外，您还可以实施更高级的场景，以满足特定的业务需求。这些场景包括：

<!-- CARDS
{target = _self}

* ./how-to/request-logging.md

* ./how-to/request-blocking.md

* ./how-to/request-transformation.md
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Monitoring sensitive requests">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./how-to/request-logging.md" title="监控敏感请求" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="assets/how-to/wknd-login.png" alt="监控敏感请求"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./how-to/request-logging.md" target="_self" rel="referrer" title="监控敏感请求">监控敏感请求</a>
                    </p>
                    <p class="is-size-6">了解如何通过在 AEM as a Cloud Service 中使用流量过滤规则来记录敏感请求，以对敏感请求进行监控。</p>
                </div>
                <a href="./how-to/request-logging.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">了解详情</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Restricting access">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./how-to/request-blocking.md" title="限制访问" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="assets/how-to/elk-tool-dashboard-blocked.png" alt="限制访问"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./how-to/request-blocking.md" target="_self" rel="referrer" title="限制访问">限制访问</a>
                    </p>
                    <p class="is-size-6">了解如何在 AEM as a Cloud Service 中通过流量过滤规则阻止特定请求以限制访问。</p>
                </div>
                <a href="./how-to/request-blocking.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">了解详情</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Normalizing requests">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./how-to/request-transformation.md" title="将请求标准化" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="assets/how-to/aemrequest-log-transformation.png" alt="将请求标准化"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./how-to/request-transformation.md" target="_self" rel="referrer" title="将请求标准化">将请求标准化</a>
                    </p>
                    <p class="is-size-6">了解如何在 AEM as a Cloud Service 中通过流量过滤规则将请求转换为标准化请求。</p>
                </div>
                <a href="./how-to/request-transformation.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">了解详情</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->

## 其他资源

- [流量过滤规则（包括 WAF 规则）](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf)
