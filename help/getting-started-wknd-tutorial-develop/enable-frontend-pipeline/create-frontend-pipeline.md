---
title: 使用前端管道部署
description: 了解如何创建并运行构建前端资源并部署到AEM as a Cloud Service中的内置CDN的前端管道。
version: Experience Manager as a Cloud Service
feature: AEM Project Archetype, Cloud Manager, CI-CD Pipeline
topic: Content Management, Development, Development, Architecture
role: Developer, Architect, Admin
level: Intermediate
jira: KT-10689
mini-toc-levels: 1
index: y
recommendations: noDisplay, noCatalog
doc-type: Tutorial
exl-id: d6da05e4-bd65-4625-b9a4-cad8eae3c9d7
duration: 225
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '685'
ht-degree: 0%

---

# 使用前端管道部署

在本章中，我们将在Adobe Cloud Manager中创建并运行前端管道。 它仅从`ui.frontend`模块构建文件，并将它们部署到AEM as a Cloud Service中的内置CDN。 因此将离开基于`/etc.clientlibs`的前端资源投放。


## 目标 {#objectives}

* 创建并运行前端管道。
* 验证前端资源不是从`/etc.clientlibs`传递，而是从以`https://static-`开头的新主机名传递

## 使用前端管道

>[!VIDEO](https://video.tv.adobe.com/v/3409420?quality=12&learn=on)

## 先决条件 {#prerequisites}

这是一个多部分教程，并假定已完成[更新标准AEM项目](./update-project.md)中列出的步骤。

确保您有[权限在Cloud Manager中创建和部署管道](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/requirements/users-and-roles.html?lang=en#role-definitions)以及[对AEM as a Cloud Service环境](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/manage-environments.html)的访问权限。

## 重命名现有管道

通过转到&#x200B;__配置__&#x200B;选项卡的&#x200B;__非生产管道名称__&#x200B;字段，将现有管道从&#x200B;__部署到开发__&#x200B;重命名为&#x200B;__全栈栈WKND部署到开发__。 这是为了通过查看管道名称来明确说明管道是全栈管道还是前端管道。

![重命名管道](assets/fullstack-wknd-deploy-dev-pipeline.png)


此外，在&#x200B;__Source代码__&#x200B;选项卡中，确保存储库和Git分支字段值正确，并且该分支包含您前端管道合同更改。

![Source代码配置管道](assets/fullstack-wknd-source-code-config.png)


## 创建前端管道

要&#x200B;__仅__&#x200B;从`ui.frontend`模块生成并部署前端资源，请执行以下步骤：

1. 在Cloud Manager UI中，从&#x200B;__管道__&#x200B;部分中单击&#x200B;__添加__&#x200B;按钮，然后根据要部署到的AEM as a Cloud Service环境选择&#x200B;__添加非生产管道__（或&#x200B;__添加生产管道__）。

1. 在&#x200B;__添加非生产管道__&#x200B;对话框中，作为&#x200B;__配置__&#x200B;步骤的一部分，选择&#x200B;__部署管道__&#x200B;选项，将其命名为&#x200B;__前端WKND部署到开发__，然后单击&#x200B;__继续__

![创建前端管道配置](assets/create-frontend-pipeline-configs.png)

1. 作为&#x200B;__Source代码__&#x200B;步骤的一部分，选择&#x200B;__前端代码__&#x200B;选项，然后从&#x200B;__符合条件的部署环境__&#x200B;中选择环境。 在&#x200B;__Source代码__部分中，确保存储库和Git分支字段值正确，并且该分支具有您前端管道合同更改。
以及__最重要的是__ __代码位置__&#x200B;字段的值为`/ui.frontend`，最后，单击&#x200B;__保存__。

![创建前端管道Source代码](assets/create-frontend-pipeline-source-code.png)


## 部署序列

* 首先运行新重命名的&#x200B;__FullStack WKND部署到Dev__&#x200B;管道，以从AEM存储库中删除WKND clientlib文件。 最重要的是，通过添加&#x200B;__Sling配置__&#x200B;文件(`SiteConfig`、`HtmlPageItemsConfig`)为前端管道合同准备AEM。

![无样式的WKND站点](assets/unstyled-wknd-site.png)

>[!WARNING]
>
>之后，__FullStack WKND部署到开发__&#x200B;管道完成，您将拥有&#x200B;__无样式的__ WKND站点，该站点可能显示为已损坏。 请计划停机或在奇数小时进行部署，这是您必须在从使用单个全栈管道到前端管道的初始切换期间计划的一次性中断。


* 最后，运行&#x200B;__FrontEnd WKND Deploy to Dev__&#x200B;管道以仅构建`ui.frontend`模块并将前端资源直接部署到CDN。

>[!IMPORTANT]
>
>您注意到&#x200B;__未设置样式的__ WKND站点已恢复正常，此时执行&#x200B;__前端__&#x200B;管道的速度比全栈管道快得多。

## 验证样式更改和新投放模式

* 打开WKND站点的任何页面，您会看到文本颜色&#x200B;__Adobe Red__，并且前端资源(CSS、JS)文件是从CDN交付的。 资源请求主机名以`https://static-pXX-eYY.p123-e456.adobeaemcloud.com/$HASH_VALUE$/theme/site.css`开头，同样以`HtmlPageItemsConfig`文件中引用的site.js或任何其他静态资源开头。


![新样式的WKND站点](assets/newly-styled-wknd-site.png)



>[!TIP]
>
>此处的`$HASH_VALUE$`与您在&#x200B;__FrontEnd WKND Deploy to Dev__&#x200B;管道的&#x200B;__内容哈希__&#x200B;字段中看到的内容相同。 AEM将收到有关前端资源的CDN URL的通知，该值存储在&#x200B;__prefixPath__&#x200B;属性下的`/conf/wknd/sling:configs/com.adobe.cq.wcm.core.components.config.HtmlPageItemsConfig/jcr:content`中。


![哈希值关联](assets/hash-value-correlartion.png)



## 恭喜！ {#congratulations}

恭喜，您已创建、运行并验证仅构建和部署WKND Sites项目的“ui.frontend”模块的前端管道。 现在，您的前端团队可以在整个AEM项目生命周期之外快速迭代站点的设计和前端行为。

## 后续步骤 {#next-steps}

在下一章[注意事项](considerations.md)中，您将回顾对前端和后端开发过程的影响。
