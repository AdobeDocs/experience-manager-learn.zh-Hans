---
title: CDN缓存命中率分析
description: 了解如何分析AEMas a Cloud Service提供的CDN日志。 获取缓存命中率以及MISS和PASS缓存类型的顶级URL等见解，以进行优化。
version: Cloud Service
feature: Operations, CDN Cache
topic: Administration, Performance
role: Admin, Architect, Developer
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-11-10T00:00:00Z
jira: KT-13312
thumbnail: KT-13312.jpeg
source-git-commit: be503ba477d63a566b687866289a81a0aa7d01f7
workflow-type: tm+mt
source-wordcount: '1231'
ht-degree: 2%

---


# CDN缓存命中率分析

了解如何分析提供的AEMas a Cloud Service **CDN日志** 并获得以下见解 **缓存命中率**、和 **的热门URL _小姐_ 和 _通过_ 缓存类型** 进行优化。


CDN日志以JSON格式提供，其中包含各种字段，包括 `url`， `cache`，有关更多信息，请参阅 [CDN日志格式](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/logging.html?lang=en#cdn-log:~:text=Toggle%20Text%20Wrapping-,Log%20Format,-The%20CDN%20logs). 此 `cache` 字段提供有关 _缓存的状态_ 其可能值包括HIT、MISS或PASS。 让我们查看可能值的详细信息。

| 缓存状态 </br> 可能值 | 描述 |
|------------------------------------|:-----------------------------------------------------:|
| 点击 | 请求的数据为 _在CDN缓存中找到，无需进行获取_ 向AEM服务器发出请求。 |
| 小姐 | 请求的数据为 _在CDN缓存中找不到，必须请求_ 从AEM服务器。 |
| 通过 | 请求的数据为 _明确设置为不缓存_ 并始终从AEM服务器中检索。 |

在本教程中， [AEM WKND项目](https://github.com/adobe/aem-guides-wknd) 部署到AEMas a Cloud Service环境，并通过触发小规模性能测试 [Apache JMeter](https://jmeter.apache.org/).

## 下载CDN日志

要下载CDN日志，请执行以下步骤：

1. 登录Cloud Manager，网址为 [my.cloudmanager.adobe.com](https://my.cloudmanager.adobe.com/) 并选择您的组织和项目。

1. 对于所需的AEMCS环境，选择 **下载日志** 从省略号菜单中。

   ![下载日志 — Cloud Manager](assets/cdn-logs-analysis/download-logs.png){width="500" zoomable="yes"}

1. 在 **下载日志** 对话框，选择 **Publish** 服务，然后单击旁边的下载图标 **cdn** 行。

   ![CDN日志 — Cloud Manager](assets/cdn-logs-analysis/download-cdn-logs.png){width="500" zoomable="yes"}


如果下载的日志文件来自 _今天_ 文件扩展名为 `.log` 否则，对于过去的日志文件，扩展名为 `.log.gz`.

## 分析下载的CDN日志

要获得诸如缓存命中率以及MISS和PASS缓存类型的顶级URL等见解，请分析下载的CDN日志文件。 这些见解有助于优化 [CDN缓存配置](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html) 并提升站点性能。

为了分析CDN日志，本文使用 **Elasticsearch、Logstash和Kibana (ELK)** [仪表板工具](https://github.com/adobe/AEMCS-CDN-Log-Analysis-ELK-Tool) 和 [Jupyter Notebook](https://jupyter.org/).


### 使用功能板工具

此 [麋鹿栈栈](https://www.elastic.co/elastic-stack) 是一套工具，可提供用于搜索、分析和可视化数据的可扩展解决方案。 它由Elasticsearch、Logstash和Kibana组成。

要确定关键详细信息，请使用 [AEMCS-CDN-Log-Analysis-ELK-Tool](https://github.com/adobe/AEMCS-CDN-Log-Analysis-ELK-Tool) 仪表板工具项目。 此项目提供了ELK栈栈的Docker容器和预配置的Kibana仪表板来分析CDN日志。

1. 请按照中的步骤进行操作 [如何设置ELK Docker容器](https://github.com/adobe/AEMCS-CDN-Log-Analysis-ELK-Tool#how-to-set-up-the-elk-docker-container) 并确保导入 **CDN缓存命中率** Kibana仪表板。

1. 要识别CDN缓存命中率和顶级URL，请执行以下步骤：

   1. 在特定环境的文件夹内复制下载的CDN日志文件。

   1. 打开 **CDN缓存命中率** 通过单击汉堡菜单> Analytics >功能板> CDN缓存命中率，创建功能板。

      ![CDN缓存命中率 — Kibana功能板](assets/cdn-logs-analysis/cdn-cache-hit-ratio-dashboard.png){width="500" zoomable="yes"}

   1. 从右上角选择所需的时间范围。

      ![时间范围 — Kibana仪表板](assets/cdn-logs-analysis/time-range.png){width="500" zoomable="yes"}

   1. 此 **CDN缓存命中率** 报告面板的含义一目了然。

   1. 此 _请求分析总数_ 部分显示以下详细资料：
      - 按高速缓存类型列出的高速缓存比率
      - 按缓存类型列出的缓存计数

      ![请求分析总数 — Kibana仪表板](assets/cdn-logs-analysis/total-request-analysis.png){width="500" zoomable="yes"}

   1. 此 _按请求或Mime类型分析_ 显示以下详细信息：
      - 按高速缓存类型列出的高速缓存比率
      - 按缓存类型列出的缓存计数
      - 主要缺失和通过URL

      ![按请求或Mime类型分析 — Kibana仪表板](assets/cdn-logs-analysis/analysis-by-request-or-mime-types.png){width="500" zoomable="yes"}

#### 按环境名称或项目ID筛选

要按环境名称筛选摄取的日志，请执行以下步骤：

1. 在“CDN缓存命中率”功能板中，单击 **添加筛选器** 图标。

   ![过滤器 — Kibana仪表板](assets/cdn-logs-analysis/filter.png){width="500" zoomable="yes"}

1. 在 **添加筛选器** 模式中，选择 `aem_env_name.keyword` 字段，并且 `is` 运算符和所需的环境名称，最后点击 _添加筛选器_.

   ![添加筛选器 — Kibana仪表板](assets/cdn-logs-analysis/add-filter.png){width="500" zoomable="yes"}

#### 按主机名筛选

要按主机名过滤摄取的日志，请执行以下步骤：

1. 在“CDN缓存命中率”功能板中，单击 **添加筛选器** 图标。

   ![过滤器 — Kibana仪表板](assets/cdn-logs-analysis/filter.png){width="500" zoomable="yes"}

1. 在 **添加筛选器** 模式中，选择 `host.keyword` 字段，并且 `is` 运算符，并为下一个字段提供所需的主机名，最后点击 _添加筛选器_.

   ![主机过滤器 — Kibana功能板](assets/cdn-logs-analysis/add-host-filter.png){width="500" zoomable="yes"}

同样，根据分析要求向功能板添加更多过滤器。

### 使用Jupyter Notebook

此 [Jupyter Notebook](https://jupyter.org/) 是一个开源Web应用程序，通过它，可创建包含代码、文本和可视化图表的文档。 它用于数据转换、可视化和统计建模。

要加速CDN日志分析，请下载 [AEM-as-a-CloudService - CDN日志分析 — Jupyter Notebook](./assets/cdn-logs-analysis/aemcs_cdn_logs_analysis.ipynb) 文件。

下载的 `aemcs_cdn_logs_analysis.ipynb` “交互式Python笔记本”文件含义一目了然，但每个部分的关键亮点包括：

- **安装其他库**：安装 `termcolor` 和 `tabulate` Python库。
- **加载CDN日志**：使用以下方式加载CDN日志文件 `log_file` 变量值，请确保更新其值。 它还会将此CDN日志转换为 [熊猫数据帧](https://pandas.pydata.org/docs/reference/frame.html).
- **执行分析**：第一个代码块是 _显示总计、HTML、JS/CSS和图像请求的分析结果_，它提供缓存命中率百分比、条形图和饼图。
第二个代码块是 _HTML、JS/CSS和图像的5大未命中请求和传递请求URL_，以表格式显示URL及其计数。

#### 在Experience Platform中运行Jupyter笔记本

>[!IMPORTANT]
>
>如果您使用或授予了该Experience Platform使用许可，则无需安装其他软件即可运行Jupyter Notebook。

要在Experience Platform中运行Jupyter Notebook，请执行以下步骤：

1. 登录到 [Adobe Experience Cloud](https://experience.adobe.com/)，在主页中> **快速访问** 部分>单击 **Experience Platform**

   ![Experience Platform](assets/cdn-logs-analysis/experience-platform.png){width="500" zoomable="yes"}

1. 在Adobe Experience Platform主页>数据科学部分中，单击 **Notebooks** 菜单项。 要启动Jupyter Notebooks环境，请单击 **JupyterLab** 选项卡。

   ![笔记本日志文件值更新](assets/cdn-logs-analysis/datascience-notebook.png){width="500" zoomable="yes"}

1. 在JupyterLab菜单中，使用 **上载文件** 图标，上传下载的CDN日志文件并 `aemcs_cdn_logs_analysis.ipynb` 文件。

   ![上传文件 — JupyteLab](assets/cdn-logs-analysis/jupyterlab-upload-file.png){width="500" zoomable="yes"}

1. 打开 `aemcs_cdn_logs_analysis.ipynb` 通过双击保存文件。

1. 在 **加载CDN日志文件** 笔记本部分，更新 `log_file` 值。

   ![笔记本日志文件值更新](assets/cdn-logs-analysis/notebook-update-variable.png){width="500" zoomable="yes"}

1. 要运行选定的单元格并前进，请单击 **播放** 图标。

   ![笔记本日志文件值更新](assets/cdn-logs-analysis/notebook-run-cell.png){width="500" zoomable="yes"}

1. 运行 **显示总计、HTML、JS/CSS和图像请求的分析结果** 代码单元格中，输出显示缓存命中率百分比、条形图和饼图。

   ![笔记本日志文件值更新](assets/cdn-logs-analysis/output-cache-hit-ratio.png){width="500" zoomable="yes"}

1. 运行 **HTML、JS/CSS和图像的5大未命中请求和传递请求URL** 代码单元格中，输出显示前5个未命中和通过请求URL。

   ![笔记本日志文件值更新](assets/cdn-logs-analysis/output-top-urls.png){width="500" zoomable="yes"}

您可以增强Jupyter Notebook以根据您的要求分析CDN日志。

## 优化CDN缓存配置

在分析CDN日志后，您可以优化CDN缓存配置以提高站点性能。 AEM最佳实践是使缓存命中率达到90%或更高。

有关更多信息，请参阅 [优化CDN缓存配置](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html#caching).

AEM WKND项目具有参考CDN配置，有关更多信息，请参阅 [CDN配置](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.d/available_vhosts/wknd.vhost#L137-L190) 从 `wknd.vhost` 文件。
