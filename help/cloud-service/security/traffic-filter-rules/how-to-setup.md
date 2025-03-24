---
title: 如何设置流量过滤器规则，包括WAF规则
description: 了解如何设置以创建、部署、测试和分析流量过滤器规则(包括WAF规则)的结果。
version: Experience Manager as a Cloud Service
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
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '575'
ht-degree: 3%

---

# 如何设置流量过滤器规则，包括WAF规则

了解&#x200B;**如何设置**&#x200B;流量过滤器规则，包括WAF规则。 阅读有关创建、部署、测试和分析结果的信息。

>[!VIDEO](https://video.tv.adobe.com/v/3425407?quality=12&learn=on)

## 设置

设置过程涉及以下内容：

- _创建具有适当AEM项目结构和配置文件的规则_。
- 使用Adobe Cloud Manager的配置管道&#x200B;_部署规则_。
- _使用各种工具测试规则_&#x200B;以生成流量。
- _使用AEMCS CDN日志和仪表板工具分析结果_。

### 在AEM项目中创建规则

要创建规则，请执行以下步骤：

1. 在AEM项目的顶层，创建文件夹`config`。

1. 在`config`文件夹中，创建一个名为`cdn.yaml`的新文件。

1. 将以下元数据添加到`cdn.yaml`文件：

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

查看AEM Guides WKND Sites项目中的`cdn.yaml`文件示例：

![WKND AEM项目规则文件和文件夹](./assets/wknd-rules-file-and-folder.png){width="800" zoomable="yes"}

### 通过Cloud Manager部署规则 {#deploy-rules-through-cloud-manager}

要部署规则，请执行以下步骤：

1. 在 [my.cloudmanager.adobe.com](https://my.cloudmanager.adobe.com/) 上登录到 Cloud Manager 并选择适当的组织和项目。

1. 从&#x200B;_项目概述_&#x200B;页面导航到&#x200B;_管道_&#x200B;卡并单击&#x200B;**+添加**&#x200B;按钮并选择所需的管道类型。

   ![Cloud Manager管道信息卡](./assets/cloud-manager-pipelines-card.png)

   在上述示例中，出于演示目的，选择了&#x200B;_添加非生产管道_，因为使用了开发环境。

1. 在&#x200B;_添加非生产管道_&#x200B;对话框中，选择并输入以下详细信息：

   1. 配置步骤：

      - **类型**：部署管道
      - **管道名称**： Dev-Config

      ![Cloud Manager配置管道对话框](./assets/cloud-manager-config-pipeline-step1-dialog.png)

   2. Source代码步骤：

      - **要部署的代码**：目标部署
      - **包含**：配置
      - **部署环境**：环境的名称，例如wknd-program-dev。
      - **存储库**：管道应从中检索代码的Git存储库；例如，`wknd-site`
      - **Git分支**： Git存储库分支的名称。
      - **代码位置**： `/config`，对应于在上一步中创建的顶级配置文件夹。

      ![Cloud Manager配置管道对话框](./assets/cloud-manager-config-pipeline-step2-dialog.png)

### 通过生成流量测试规则

要测试规则，有多种可用的第三方工具，并且您的组织可能有首选工具。 出于演示目的，我们使用以下工具：

- [Curl](https://curl.se/)用于基本测试，如调用URL和检查响应代码。

- [Vegeta](https://github.com/tsenart/vegeta)执行拒绝服务(DOS)。 按照[Vegeta GitHub](https://github.com/tsenart/vegeta#install)中的安装说明操作。

- [Nikto](https://github.com/sullo/nikto/wiki)以查找潜在的问题和安全漏洞，如XSS、SQL注入等。 按照[Nikto GitHub](https://github.com/sullo/nikto)中的安装说明进行操作。

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

创建、部署和测试规则后，您可以使用&#x200B;**CDN**&#x200B;日志和&#x200B;**AEMCS-CDN-Log-Analysis-Tooling**&#x200B;来分析结果。 该工具提供了一组仪表板，用于可视化Splunk和ELK(Elasticsearch、Logstash和Kibana)栈栈的结果。

工具可以从[AEMCS-CDN-Log-Analysis-Tooling](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling) GitHub存储库中克隆。 然后，按照相关说明安装和加载&#x200B;**CDN流量仪表板**&#x200B;和&#x200B;**WAF仪表板**&#x200B;以用于您首选的可观察性工具。

在本教程中，让我们使用ELK栈栈。 按照用于AEMCS CDN日志分析的[ELK Docker容器](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling/blob/main/ELK/README.md)说明设置ELK栈栈。

- 加载示例仪表板后，您的Elastic仪表板工具页面应如下所示：

  ![ELK流量过滤器规则仪表板](./assets/elk-dashboard.png)

>[!NOTE]
>
>    由于尚未摄取AEMCS CDN日志，因此仪表板为空。


## 下一步

了解如何使用WAF WKND Sites项目在[示例和结果分析](./examples-and-analysis.md)章节中声明流量过滤器规则，包括AEM规则。
