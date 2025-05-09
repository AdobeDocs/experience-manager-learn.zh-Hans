---
title: 在AEM中设置翻译规则
description: 利用翻译配置UI，用户可以管理用于在AEM Sites中翻译内容的规则。 本视频详细介绍如何为自定义组件创建新的翻译规则。
feature: Language Copy
version: Experience Manager 6.4, Experience Manager 6.5
topic: Localization
role: User
level: Beginner
doc-type: Technical Video
exl-id: 359da531-839c-4680-abf9-c880cc700159
duration: 542
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '283'
ht-degree: 0%

---

# 设置翻译规则 {#set-up-translation-rules-in-aem}

利用翻译配置UI，用户可以管理用于在AEM Sites中翻译内容的规则。 本视频详细介绍如何为自定义组件创建新的翻译规则。

>[!NOTE]
>
> 以下视频是在AEM 6.3上录制的。AEM 6.4+引入了新的存储库结构，用于存储翻译规则XML文件。 在AEM 6.4+中使用翻译配置UI时，规则将保存到位置`/conf/global/settings/translation/rules/translation_rules.xml`。 有关更多详细信息，请参阅[标识要翻译的内容](https://helpx.adobe.com/cn/experience-manager/6-5/sites/administering/using/tc-rules.html)。

>[!VIDEO](https://video.tv.adobe.com/v/38442?quality=12&learn=on&captions=chi_hans)

翻译规则标识AEM中要提取以进行翻译的内容。 现成的翻译规则涵盖了常见的用例，例如图像组件的文本组件和替换文本。 根据项目翻译要求，可能需要其他规则。 一般而言，翻译规则允许用户指定：

1. 应根据路径和/或资源类型翻译的属性
2. 用于筛选不应翻译的属性的筛选器
3. 应翻译的引用内容（即图像或内容片段）

将更新翻译xml文件的翻译规则编辑器。 利用翻译配置UI，可以更轻松地管理各种翻译规则，并在直接编辑XML时防止出现拼写错误。

访问翻译配置UI：

* **[!UICONTROL AEM开始菜单] > [!UICONTROL 工具] > [!UICONTROL 常规] > [[!UICONTROL 翻译配置]](http://localhost:4502/libs/cq/translation/translationrules/contexts.html)**

## AEM 6.3之前的版本 {#prior-to-aem}

在以前的AEM版本中，通过编辑位于翻译工作流`/etc/workflow/models/translation/translation_rules.xml`下的XML文件手动更新了翻译规则。

## 其他资源 {#additional-resources}

* [标识要翻译的内容](https://helpx.adobe.com/cn/experience-manager/6-5/sites/administering/using/tc-rules.html)
* [翻译多语言站点的内容](https://helpx.adobe.com/cn/experience-manager/6-5/sites/administering/using/translation.html)
* [https://helpx.adobe.com/cn/experience-manager/6-5/sites/administering/using/tc-manage.html](https://helpx.adobe.com/cn/experience-manager/6-5/sites/administering/using/tc-manage.html)
* [翻译最佳实践](https://helpx.adobe.com/cn/experience-manager/6-5/sites/administering/using/tc-bp.html)
