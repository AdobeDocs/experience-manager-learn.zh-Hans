---
title: 在HTM5表单提交时触发AEM工作流
seo-title: 在HTML5表单提交时触发AEM工作流
description: 在离线模式下继续填写移动表单，并提交移动表单以触发AEM工作流
seo-description: 在离线模式下继续填写移动表单，并提交移动表单以触发AEM工作流
feature: 移动设备表单
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: 开发
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '192'
ht-degree: 3%

---


# 审核和批准提交的PDF的工作流程

最后一步和最后一步是创建AEM工作流，该工作流将生成静态或非交互式PDF以供审阅和批准。 工作流将通过在节点`/content/pdfsubmissions`上配置的AEM启动器触发。

以下屏幕截图显示了工作流中涉及的步骤。

![workflow](assets/workflow.PNG)

## 生成非交互式PDF工作流步骤

此处指定了XDP模板以及要与模板合并的数据。 要合并的数据是PDF中提交的数据。 提交的数据存储在节点`/content/pdfsubmissions`下。

![工作流](assets/generate-pdf1.PNG)

生成的PDF被分配到名为`submittedPDF`的工作流变量。

![工作流](assets/generate-pdf2.PNG)

### 分配生成的PDF以供审阅和批准

分配任务工作流组件用于分配生成的PDF以供审核和批准。 变量`submittedPDF`用在“分配任务”工作流组件的“Forms和文档”选项卡中。

![工作流](assets/assign-task.PNG)
