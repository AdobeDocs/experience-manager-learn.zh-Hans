---
title: 在AEM中设置翻译规则
description: 翻译配置UI允许用户管理用于在AEM Sites中翻译内容的规则。 此视频详细介绍如何为自定义组件创建新的翻译规则。
feature: 语言复制
topics: localization, content-architecture
audience: developer, administrator
doc-type: technical video
activity: setup
version: 6.4, 6.5
topic: 本地化
role: Business Practitioner
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '324'
ht-degree: 3%

---


# 设置翻译规则{#set-up-translation-rules-in-aem}

翻译配置UI允许用户管理用于在AEM Sites中翻译内容的规则。 此视频详细介绍如何为自定义组件创建新的翻译规则。

>[!NOTE]
>
> 以下视频在AEM 6.3中录制。 AEM 6.4+引入了用于存储翻译规则XML文件的新存储库结构。 在AEM 6.4+中使用翻译配置UI时，规则会保存到`/conf/global/settings/translation/rules/translation_rules.xml`位置。 有关更多详细信息，请参阅[标识要翻译的内容](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-rules.html) 。

>[!VIDEO](https://video.tv.adobe.com/v/18135/?quality=9&learn=on)

翻译规则识别AEM中要提取以进行翻译的内容。 现成的翻译规则涵盖常见用例，如文本组件和图像组件的替换文本。 根据项目翻译要求，可能需要使用其他规则。 通常，翻译规则允许用户指定：

1. 应根据路径和/或资源类型进行翻译的属性
2. 不应翻译的属性过滤器
3. 应翻译的引用内容（即图像或内容片段）

将更新翻译xml文件的翻译规则编辑器。 通过翻译配置UI，可以更轻松地管理各种翻译规则，并在直接编辑XML时防止拼写错误。

访问翻译配置UI:

* **[!UICONTROL AEM开始菜单]  >  [!UICONTROL 工具]  >  [!UICONTROL 常规]  >  [[!UICONTROL 翻译配置]](http://localhost:4502/libs/cq/translation/translationrules/contexts.html)**

## AEM 6.3 {#prior-to-aem}之前

在以前的AEM版本翻译规则中，通过编辑位于翻译工作流下的XML文件来手动更新：`/etc/workflow/models/translation/translation_rules.xml`。

## 其他资源 {#additional-resources}

* [识别要翻译的内容](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-rules.html)
* [翻译多语言站点的内容](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/translation.html)
* [https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-manage.html](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-manage.html)
* [翻译最佳实践](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-bp.html)
