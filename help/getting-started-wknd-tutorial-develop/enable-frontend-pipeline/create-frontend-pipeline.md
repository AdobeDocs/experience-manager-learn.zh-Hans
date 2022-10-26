---
title: 使用前端管道部署
description: 了解如何创建和运行前端管道，以构建前端资源并部署到AEMas a Cloud Service中内置的CDN。
version: Cloud Service
type: Tutorial
feature: AEM Project Archetype, Cloud Manager, CI-CD Pipeline
topic: Content Management, Development, Development, Architecture
role: Developer, Architect, Admin
level: Intermediate
kt: 10689
mini-toc-levels: 1
index: y
recommendations: disable
source-git-commit: f0c6e6cd09c1a2944de667d9f14a2d87d3e2fe1d
workflow-type: tm+mt
source-wordcount: '701'
ht-degree: 0%

---


# 使用前端管道部署

在本章中，我们在Adobe云管理器中创建并运行前端管道。 它仅从构建文件 `ui.frontend` 模块并将它们部署到AEMas a Cloud Service中的内置CDN。 从  `/etc.clientlibs` 基于前端资源交付。


## 目标 {#objectives}

* 创建并运行前端管道。
* 验证前端资源是否未从 `/etc.clientlibs` 但是从以 `https://static-`

## 使用前端管道

>[!VIDEO](https://video.tv.adobe.com/v/3409420/)

## 前提条件 {#prerequisites}

这是一个多部分教程，我们假定在 [更新标准AEM项目](./update-project.md) 已完成。

确保您 [在Cloud Manager中创建和部署管道的权限](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/requirements/users-and-roles.html?lang=en#role-definitions) 和 [访问AEMas a Cloud Service环境](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/manage-environments.html).

## 重命名现有管线

从重命名现有管线 __部署到开发环境__ to  __将全栈WKND部署到开发环境__ 通过 __配置__ 选项卡 __非生产管道名称__ 字段。 这是为了通过查看管道名称来明确管道是全栈还是前端。

![重命名管道](assets/fullstack-wknd-deploy-dev-pipeline.png)


在 __源代码__ 选项卡中，确保“存储库”和“Git分支”字段值正确无误，并且分支具有您的前端管道合同更改。

![源代码配置管道](assets/fullstack-wknd-source-code-config.png)


## 创建前端管线

至 __仅__ 从 `ui.frontend` 模块中，请执行以下步骤：

1. 在Cloud Manager UI中，从 __管道__ ，单击 __添加__ 按钮，然后选择 __添加非生产管道__ (或 __添加生产管道__)。

1. 在 __添加非生产管道__ 对话框，作为 __配置__ 步骤，选择 __部署管道__ 选项，将其命名为 __将前端WKND部署到开发人员__，然后单击 __继续__

![创建前端管道配置](assets/create-frontend-pipeline-configs.png)

1. 作为 __源代码__ 步骤，选择 __前端代码__ ，并从 __符合条件的部署环境__. 在 __源代码__ 部分确保存储库和Git分支字段值正确，并且分支具有您的前端管道合同更改。
和 __最重要__ 对于 __代码位置__ 字段的值为 `/ui.frontend` 最后，单击 __保存__.

![创建前端管道源代码](assets/create-frontend-pipeline-source-code.png)


## 部署序列

* 首先运行新重命名的 __将全栈WKND部署到开发环境__ 用于从AEM存储库中删除WKND clientlib文件的管道。 最重要的是，通过添加 __Sling配置__ 文件(`SiteConfig`, `HtmlPageItemsConfig`)。

![未设置样式的WKND站点](assets/unstyled-wknd-site.png)

>[!WARNING]
>
>之后， __将全栈WKND部署到开发环境__ 管道完成 __未设置样式__ WKND站点，可能显示为已损坏。 请在奇数小时内计划中断或部署，这是在初次从使用单个全栈管道切换到前端管道时必须计划的一次性中断。


* 最后，运行 __将前端WKND部署到开发人员__ 仅构建管道 `ui.frontend` 模块，并将前端资源直接部署到CDN。

>[!IMPORTANT]
>
>您会注意到 __未设置样式__ WKND站点恢复正常，这次 __前端__ 管道执行比全栈管道快得多。

## 验证样式更改和新的投放模式

* 打开WKND站点的任意页面，您可以看到文本颜色 __Adobe红色__ 前端资源(CSS、JS)文件则从CDN交付。 资源请求主机名以开头 `https://static-pXX-eYY.p123-e456.adobeaemcloud.com/$HASH_VALUE$/theme/site.css` 以及您在 `HtmlPageItemsConfig` 文件。


![新设置的WKND站点](assets/newly-styled-wknd-site.png)



>[!TIP]
>
>的 `$HASH_VALUE$` 此处与您在 __将前端WKND部署到开发人员__  管道 __内容哈希__ 字段。 AEM会收到前端资源的CDN URL通知，该值存储在 `/conf/wknd/sling:configs/com.adobe.cq.wcm.core.components.config.HtmlPageItemsConfig/jcr:content` 在 __prefixPath__ 属性。


![哈希值关联](assets/hash-value-correlartion.png)



## 恭喜！ {#congratulations}

恭喜，您已创建、运行并验证前端管道，该管道仅构建和部署WKND Sites项目的“ui.frontend”模块。 现在，您的前端团队可以在整个AEM项目生命周期之外快速迭代网站的设计和前端行为。

## 下面的步骤 {#next-steps}

在下一章中， [注意事项](considerations.md)，您将回顾对前端和后端开发流程的影响。
