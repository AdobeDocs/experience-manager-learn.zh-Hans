---
title: 在HTM5表单提交时触发AEM工作流
seo-title: 在HTML5表单提交时触发AEM Workflow
description: 在脱机模式下继续填写移动表单并提交移动表单以触发AEM工作流
seo-description: 在脱机模式下继续填写移动表单并提交移动表单以触发AEM工作流
feature: 移动设备表单
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: 开发
role: 开发人员
level: 富有经验
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '194'
ht-degree: 4%

---


# 用于审阅和批准提交的PDF的工作流

最后也是最后一步，创建AEM工作流，该工作流将生成静态或非交互式PDF供审阅和批准。 工作流将通过在节点`/content/pdfsubmissions`上配置的AEM启动程序触发。

以下屏幕截图显示了工作流程中涉及的步骤。

![workflow](assets/workflow.PNG)

## 生成非交互式PDF工作流程步骤

此处指定XDP模板以及要与模板合并的数据。 要合并的数据是PDF中提交的数据。 此提交的数据存储在节点`/content/pdfsubmissions`下。

![工作流](assets/generate-pdf1.PNG)

生成的PDF被分配给名为`submittedPDF`的工作流变量。

![工作流](assets/generate-pdf2.PNG)

### 分配生成的pdf以供审阅和审批

分配任务工作流组件用于分配生成的PDF以供审阅和审批。 变量`submittedPDF`用于指定任务工作流组件的“Forms和文档”选项卡。

![工作流](assets/assign-task.PNG)
