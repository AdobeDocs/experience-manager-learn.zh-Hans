---
title: 在HTM5表单提交时触发AEM工作流
seo-title: 在提交HTML5表单时触发AEM工作流
description: 在脱机模式下继续填写移动表单并提交移动表单以触发AEM工作流程
seo-description: 在脱机模式下继续填写移动表单并提交移动表单以触发AEM工作流程
feature: mobile-forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
translation-type: tm+mt
source-git-commit: c56942831614b981684861ea78f1bd15f3bb1ab9
workflow-type: tm+mt
source-wordcount: '189'
ht-degree: 2%

---


# 审阅和批准提交的PDF的工作流程

最后一步和最后一步是创建AEM工作流，它将生成静态或非交互式PDF以供审阅和批准。 将通过在节点`/content/pdfsubmissions`上配置的AEM启动器触发工作流。

以下屏幕截图显示了工作流涉及的步骤。

![workflow](assets/workflow.PNG)

## 生成非交互式PDF工作流程步骤

此处指定了XDP模板以及要与模板合并的数据。 要合并的数据是PDF中提交的数据。 此提交的数据存储在节点`/content/pdfsubmissions`下。

![工作流](assets/generate-pdf1.PNG)

生成的PDF被分配给名为`submittedPDF`的工作流变量。

![工作流](assets/generate-pdf2.PNG)

### 分配生成的pdf以供审阅和批准

分配任务工作流组件用于分配生成的PDF以供审阅和审批。 变量`submittedPDF`用于“指定Forms”工作流组件的“文档”和“任务”选项卡。

![工作流](assets/assign-task.PNG)
