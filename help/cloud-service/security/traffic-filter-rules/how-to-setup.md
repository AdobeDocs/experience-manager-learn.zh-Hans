---
title: 如何设置包括WAF规则在内的流量过滤器规则
description: 了解如何设置以创建、部署、测试和分析流量过滤器规则（包括WAF规则）的结果。
version: Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-10-26T00:00:00Z
jira: KT-13148
thumbnail: KT-13148.jpeg
exl-id: b67bf642-3341-48d0-8ea9-5f262febf414
duration: 292
source-git-commit: c7c78ca56c1d72f13d2dc80229a10704ab0f14ab
workflow-type: tm+mt
source-wordcount: '575'
ht-degree: 3%

---

# 如何设置包括WAF规则在内的流量过滤器规则

学习 **如何设置** 流量过滤器规则，包括WAF规则。 阅读有关创建、部署、测试和分析结果的信息。

>[!VIDEO](https://video.tv.adobe.com/v/3425407?quality=12&learn=on)

## 设置

设置过程涉及以下内容：

- _创建规则_ 具有适当的AEM项目结构和配置文件。
- _部署规则_ 使用AdobeCloud Manager的配置管道。
- _测试规则_ 使用各种工具生成流量。
- _分析结果_ 使用AEMCS CDN日志和功能板工具。

### 在AEM项目中创建规则

要创建规则，请执行以下步骤：

1. 在AEM项目的顶层，创建一个文件夹 `config`.

1. 在 `config` 文件夹，创建一个名为的新文件 `cdn.yaml`.

1. 将以下元数据添加到 `cdn.yaml` 文件：

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
```

查看示例 `cdn.yaml` AEM Guides WKND Sites项目中的文件：

![WKND AEM项目规则文件和文件夹](./assets/wknd-rules-file-and-folder.png){width="800" zoomable="yes"}

### 通过Cloud Manager部署规则 {#deploy-rules-through-cloud-manager}

要部署规则，请执行以下步骤：

1. 在 [my.cloudmanager.adobe.com](https://my.cloudmanager.adobe.com/) 上登录到 Cloud Manager 并选择适当的组织和项目。

1. 导航至 _管道_ 中的卡 _项目概述_ 页面，然后单击 **+添加** 按钮并选择所需的管道类型。

   ![Cloud Manager管道信息卡](./assets/cloud-manager-pipelines-card.png)

   在上述示例中，出于演示目的 _添加非生产管道_ 选择，因为使用了开发环境。

1. 在 _添加非生产管道_ 对话框，选择并输入以下详细信息：

   1. 配置步骤：

      - **类型**：部署管道
      - **管道名称**：Dev-Config

      ![Cloud Manager配置管道对话框](./assets/cloud-manager-config-pipeline-step1-dialog.png)

   2. 源代码步骤：

      - **要部署的代码**：目标部署
      - **包括**：配置
      - **部署环境**：您的环境的名称，例如wknd-program-dev。
      - **存储库**：管道应从中检索代码的Git存储库；例如， `wknd-site`
      - **Git分支**：Git存储库分支的名称。
      - **代码位置**： `/config`，对应于在上一步中创建的顶级配置文件夹。

      ![Cloud Manager配置管道对话框](./assets/cloud-manager-config-pipeline-step2-dialog.png)

### 通过生成流量测试规则

要测试规则，有多种可用的第三方工具，并且您的组织可能有首选工具。 出于演示目的，我们使用以下工具：

- [Curl](https://curl.se/) 基本测试，例如调用URL和检查响应代码。

- [韦盖塔](https://github.com/tsenart/vegeta) 执行拒绝服务(DOS)。 按照 [Vegeta GitHub](https://github.com/tsenart/vegeta#install).

- [Nikto](https://github.com/sullo/nikto/wiki) 以发现潜在的问题和安全漏洞，如XSS、SQL注入等。 请按照 [Nikto GitHub](https://github.com/sullo/nikto).

- 通过运行以下命令，验证终端中是否已安装这些工具以及这些工具是否可用：

  ```shell
  # Curl version check
  $ curl --version
  
  # Vegeta version check
  $ vegeta -version
  
  # Nikto version check
  $ cd <PATH-OF-CLONED-REPO>/program
  ./nikto.pl -Version
  ```

### 使用仪表板工具分析结果

创建、部署和测试规则后，您可以使用分析结果 **CDN** 日志和 **AEMCS-CDN-Log-Analysis-Tooling**. 该工具提供了一组仪表板，用于可视化Splunk和ELK(Elasticsearch、Logstash和Kibana)栈栈的结果。

工具可以从以下位置克隆： [AEMCS-CDN-Log-Analysis-Tooling](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling) github存储库。 然后，按照说明安装和加载 **CDN流量仪表板** 和 **WAF仪表板** 功能板作为首选可观察性工具。

在本教程中，让我们使用ELK栈栈。 请遵循 [用于AEMCS CDN日志分析的ELK Docker容器](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling/blob/main/ELK/README.md) 设置ELK栈栈的说明。

- 加载示例仪表板后，您的Elastic仪表板工具页面应如下所示：

  ![ELK流量过滤器规则仪表板](./assets/elk-dashboard.png)

>[!NOTE]
>
>    由于尚未摄取AEMCS CDN日志，因此仪表板为空。


## 下一步

了解如何在中声明流量过滤器规则，包括WAF规则 [示例和结果分析](./examples-and-analysis.md) 第章，使用AEM WKND Sites项目。
