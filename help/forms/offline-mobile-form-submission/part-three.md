---
title: 在HTM5表单提交时触发AEM工作流 — 审核和批准PDF
seo-title: Trigger AEM Workflow on HTML5 Form Submission
description: 在离线模式下继续填写移动表单，并提交移动表单以触发AEM工作流
seo-description: Continue filling mobile form in offline mode and submit mobile form to trigger AEM workflow
feature: Mobile Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: a767d8f8-d75e-4472-9139-c08d804ee076
source-git-commit: 012850e3fa80021317f59384c57adf56d67f0280
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---

# 审核和批准已提交PDF的工作流

最后一步和最后一步是创建AEM工作流，该工作流将生成一个静态或非交互式PDF以供审核和批准。 工作流将通过在节点上配置的AEM启动器触发 `/content/pdfsubmissions`.

以下屏幕截图显示了工作流中涉及的步骤。

![workflow](assets/workflow.PNG)

## 生成非交互式PDF工作流步骤

此处指定了XDP模板以及要与模板合并的数据。 要合并的数据是从PDF提交的数据。 提交的数据存储在节点下 `/content/pdfsubmissions`.

![工作流](assets/generate-pdf1.PNG)

生成的PDF会被分配给名为 `submittedPDF`.

![工作流](assets/generate-pdf2.PNG)

### 分配生成的PDF以供审阅和批准

分配任务工作流组件用于分配生成的PDF以供审核和批准。 变量 `submittedPDF` 在“分配任务”工作流组件的“Forms和文档”选项卡中使用。

![工作流](assets/assign-task.PNG)
