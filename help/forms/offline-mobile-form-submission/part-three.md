---
title: 在HTM5表單提交時觸發AEM工作流程 — 檢閱和核准PDF
seo-title: Trigger AEM Workflow on HTML5 Form Submission
description: 繼續以離線模式填寫行動表單並提交行動表單以觸發AEM工作流程
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
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '170'
ht-degree: 2%

---

# 用於檢閱和核准已提交PDF的工作流程

最後一個也是最後一個步驟是建立AEM工作流程，這會產生靜態或非互動式PDF以供檢閱和核准。 透過節點上設定的AEM啟動器觸發工作流程 `/content/pdfsubmissions`.

以下熒幕擷圖顯示工作流程中涉及的步驟。

![工作流](assets/workflow.PNG)

## 產生非互動式PDF工作流程步驟

此處會指定XDP範本及要與範本合併的資料。 要合併的資料是從PDF提交的資料。 此提交的資料會儲存在節點下 `/content/pdfsubmissions`.

![工作流](assets/generate-pdf1.PNG)

產生的PDF會指派給名為的工作流程變數 `submittedPDF`.

![工作流](assets/generate-pdf2.PNG)

### 指派產生的PDF以供稽核和核准

指派任務工作流程元件在此用於指派產生的PDF以供檢閱和核准。 變數 `submittedPDF` 用於「指派任務」工作流程元件的Forms和「檔案」索引標籤中。

![工作流](assets/assign-task.PNG)
