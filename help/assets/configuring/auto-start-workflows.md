---
title: 自动启动工作流
description: 自动启动工作流通过在上传或重新处理时自动调用自定义工作流，来扩展资产处理。
feature: Asset Compute Microservices, Workflow
version: Cloud Service
jira: KT-4994
thumbnail: 37323.jpg
topic: Development
role: Developer
level: Intermediate
last-substantial-update: 2023-05-14T00:00:00Z
doc-type: Feature Video
exl-id: 5e423f2c-90d2-474f-8bdc-fa15ae976f18
duration: 385
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '324'
ht-degree: 0%

---

# 自动启动工作流

自动启动工作流通过在上传时自动调用自定义工作流，或者一旦资源处理完成后重新处理，来扩展AEM as a Cloud Service中的资源处理。

>[!VIDEO](https://video.tv.adobe.com/v/37323?quality=12&learn=on)

>[!NOTE]
>
>使用自动启动工作流自定义资产后处理，而不是使用工作流启动器。 自动启动工作流仅&#x200B;_在资产处理完成后调用_，而不是在资产处理期间可能多次调用启动器。

## 自定义Post处理工作流

要自定义Post处理工作流，请复制默认的Assets Post处理[工作流模型](../../foundation/workflow/use-the-workflow-editor.md)。

1. 导航到&#x200B;_工具_ > _工作流_ > _模型_，从“工作流模型”屏幕开始
2. 查找并选择&#x200B;_Assets Cloud Post-Processing_&#x200B;工作流模型<br/>
   ![选择Assets Cloud Post处理工作流模型](assets/auto-start-workflow-select-workflow.png)
3. 选择&#x200B;_复制_&#x200B;按钮以创建自定义工作流
4. 选择您现在的工作流模型(将称为&#x200B;_Assets Cloud Post-Processing1_)，然后单击&#x200B;_编辑_&#x200B;按钮以编辑工作流
5. 在“工作流属性”中，为您的自定义Post处理工作流指定一个有意义的名称<br/>
   ![更改名称](assets/auto-start-workflow-change-name.png)
6. 添加步骤以满足您的业务要求，在本例中，您可以在资产完成处理时添加任务。 确保工作流的最后一步始终是&#x200B;_工作流完成_&#x200B;步骤<br/>
   ![添加工作流步骤](assets/auto-start-workflow-customize-steps.png)

   >[!NOTE]
   >
   >自动启动工作流在每次上传或重新处理资产时都会运行，因此请仔细考虑工作流步骤的扩展影响，尤其是对于批量操作，如[批量导入](../../cloud-service/migration/bulk-import.md)或迁移。

7. 选择&#x200B;_同步_&#x200B;按钮以保存更改并同步工作流模型

## 使用自定义Post处理工作流

在文件夹上配置了自定义Post处理。 要在文件夹中配置自定义Post处理工作流，请执行以下操作：

1. 选择要配置工作流的文件夹并编辑文件夹属性
2. 切换到&#x200B;_资产处理_&#x200B;选项卡
3. 在&#x200B;_自动启动工作流_&#x200B;选择框<br/>中选择您的自定义Post处理工作流
   ![设置Post处理工作流](assets/auto-start-workflow-set-workflow.png)
4. 保存更改

现在，您的自定义Post处理工作流将针对该文件夹下上传或重新处理的所有资源运行。
