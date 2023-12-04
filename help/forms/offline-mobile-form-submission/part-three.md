---
title: 在HTM5表单提交时触发AEM工作流 — 审查和批准PDF
description: 在离线模式下继续填写移动表单并提交移动表单以触发AEM工作流
feature: Mobile Forms
doc-type: article
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: a767d8f8-d75e-4472-9139-c08d804ee076
duration: 50
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '170'
ht-degree: 0%

---

# 此工作流用于审阅和批准已提交的PDF

最后一个也是最后一个步骤是创建AEM工作流，该工作流将生成静态或非交互式PDF以供审阅和批准。 工作流通过节点上配置的AEM启动器触发 `/content/pdfsubmissions`.

以下屏幕截图显示了工作流中涉及的步骤。

![工作流](assets/workflow.PNG)

## 生成非交互式PDF工作流步骤

此处指定了XDP模板以及要与模板合并的数据。 要合并的数据是从PDF提交的数据。 此提交的数据存储在节点下 `/content/pdfsubmissions`.

![工作流](assets/generate-pdf1.PNG)

生成的PDF将分配给名为的工作流变量 `submittedPDF`.

![工作流](assets/generate-pdf2.PNG)

### 分配生成的PDF以供审阅和批准

分配任务工作流组件用于分配生成的PDF以供审阅和批准。 变量 `submittedPDF` 在分配任务工作流组件的Forms和文档选项卡中使用。

![工作流](assets/assign-task.PNG)
