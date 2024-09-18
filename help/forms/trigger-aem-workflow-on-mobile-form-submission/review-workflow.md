---
title: 在HTML5表单提交时触发AEM工作流 — 审查和批准PDF
description: 此工作流用于查看已提交的PDF
feature: Mobile Forms
doc-type: article
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
jira: kt-16215
badgeVersions: label="AEM Forms 6.5" before-title="false"
source-git-commit: 5f42678502a785ead29982044d1f3f5ecf023e0f
workflow-type: tm+mt
source-wordcount: '172'
ht-degree: 1%

---

# 此工作流用于审阅和批准已提交的PDF

最后一个也是最后一个步骤是创建AEM工作流，该工作流将生成静态或非交互式PDF以供审阅和批准。 工作流通过节点`/content/formsubmissions`上配置的AEM启动器触发。

以下屏幕截图显示了工作流中涉及的步骤。

![工作流](assets/workflow.PNG)

## 生成非交互式PDF工作流步骤

此处指定了XDP模板以及要与模板合并的数据。 要合并的数据是从PDF提交的数据。 此提交的数据存储在节点```/content/formsubmissions```下

![工作流](assets/generate-pdf1.PNG)

生成的PDF已分配给名为`submittedPDF`的工作流变量。

![工作流](assets/generate-pdf2.PNG)

### 分配生成的PDF以供审阅和批准

分配任务工作流组件用于分配生成的PDF以供审阅和批准。 变量`submittedPDF`用在分配任务工作流组件的Forms和文档选项卡中。

![工作流](assets/assign-task.PNG)


## 后续步骤

[在您的环境中部署资产](./deploy-assets.md)