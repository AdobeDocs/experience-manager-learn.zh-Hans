---
title: 在AEM中设置翻译规则
description: 翻译配置UI允许用户管理用于在AEM Sites中翻译内容的规则。 此视频详细介绍如何为自定义组件创建新的翻译规则。
feature: Language Copy
topics: localization, content-architecture
audience: developer, administrator
doc-type: technical video
activity: setup
version: 6.4, 6.5
topic: Localization
role: User
level: Beginner
exl-id: 359da531-839c-4680-abf9-c880cc700159
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '321'
ht-degree: 6%

---

# 设置翻译规则 {#set-up-translation-rules-in-aem}

翻译配置UI允许用户管理用于在AEM Sites中翻译内容的规则。 此视频详细介绍如何为自定义组件创建新的翻译规则。

>[!NOTE]
>
> 以下视频在AEM 6.3中录制。 AEM 6.4+引入了用于存储翻译规则XML文件的新存储库结构。 在AEM 6.4及更高版本中使用翻译配置UI时，规则会保存到该位置 `/conf/global/settings/translation/rules/translation_rules.xml`. 请参阅 [识别要翻译的内容](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-rules.html) 以了解更多详细信息。

>[!VIDEO](https://video.tv.adobe.com/v/18135?quality=12&learn=on)

翻译规则识别AEM中要提取以进行翻译的内容。 现成的翻译规则涵盖常见用例，如文本组件和图像组件的替换文本。 根据项目翻译要求，可能需要使用其他规则。 通常，翻译规则允许用户指定：

1. 应根据路径和/或资源类型进行翻译的属性
2. 不应翻译的属性过滤器
3. 应翻译的引用内容（即图像或内容片段）

将更新翻译xml文件的翻译规则编辑器。 通过翻译配置UI，可以更轻松地管理各种翻译规则，并在直接编辑XML时防止拼写错误。

访问翻译配置UI:

* **[!UICONTROL AEM开始菜单] > [!UICONTROL 工具] > [!UICONTROL 常规] > [[!UICONTROL 翻译配置]](http://localhost:4502/libs/cq/translation/translationrules/contexts.html)**

## AEM 6.3之前 {#prior-to-aem}

在以前的AEM版本翻译规则中，通过编辑位于翻译工作流下的XML文件来手动更新： `/etc/workflow/models/translation/translation_rules.xml`.

## 其他资源 {#additional-resources}

* [标识要翻译的内容](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-rules.html)
* [翻译多语言站点的内容](https://helpx.adobe.com/cn/experience-manager/6-5/sites/administering/using/translation.html)
* [https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-manage.html](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-manage.html)
* [翻译最佳实践](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-bp.html)
