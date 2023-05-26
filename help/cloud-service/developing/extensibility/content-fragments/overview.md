---
title: AEM内容片段控制台扩展
description: 了解如何构建和部署AEMas a Cloud Service内容片段控制台扩展
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay
kt: 11603
thumbnail: KT-11603.png
last-substantial-update: 2022-12-09T00:00:00Z
exl-id: 093c8d83-2402-4feb-8a56-267243d229dd
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '745'
ht-degree: 4%

---

# AEM内容片段控制台扩展

[AEM内容片段控制台](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/content-fragments/content-fragments-console.html) 扩展可通过两个扩展点添加：中的按钮 [内容片段控制台的](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/content-fragments/content-fragments-console.html) 标题菜单或操作栏。 这些扩展使用作为App Builder应用程序运行的JavaScript编写，可以实施自定义Web UI和无服务器Adobe I/O Runtime操作，以执行更密集、长时间运行的工作。

![AEM内容片段控制台扩展](./assets/overview/example.png){align="center"}

| 扩展类型 | 描述 | 参数 |
| :--- | :--- | :--- |
| 标题菜单 | 将按钮添加到以下情况下显示的标题： __零__ 已选择内容片段。 | 无。 |
| 操作栏 | 向操作栏添加一个按钮，该按钮在 __一个或多个__ 已选择内容片段。 | 所选内容片段的路径的数组。 |

单个AEM内容片段控制台扩展可以包含零个或一个标题菜单，以及零个或一个操作栏扩展类型。 如果需要同一类型的多个扩展类型，则必须创建多个AEM内容片段控制台扩展。

AEM内容片段控制台扩展，需要 [Adobe Developer控制台项目](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/extension-development/#create-a-project-in-adobe-developer-console) 和 [App Builder应用程序](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/code-generation) 使用 `@adobe/aem-cf-admin-ui-ext-tpl` 模板，与Adobe Developer Console项目关联。

在生成App Builder应用程序时，根据扩展的用途，从以下功能中进行选择。 可以在扩展中使用任何选项组合。

|  | 将按钮添加到 [标题菜单](./header-menu.md) | 将按钮添加到 [操作栏](./action-bar.md) | 显示 [模态](./modal.md) | 添加 [服务器端处理程序](./runtime-action.md) |
| ------------------------------------------ | :-----------------------: | :----------------------: | :--------: | :--------------------:  |
| 未选择内容片段时可用 | ✔ |  |  |  |
| 在选择一个或多个内容片段时可用 |  | ✔ |  |  |
| 从用户处收集自定义输入 |  |  | ✔️ |  |
| 向用户显示自定义反馈 |  |  | ✔️ |  |
| 调用AEM的HTTP请求 |  |  |  | ✔ |
| 调用HTTP请求以Adobe/第三方服务 |  |  |  | ✔ |


## Adobe Developer文档

Adobe Developer包含有关AEM内容片段控制台扩展的开发人员详细信息。 请查看 [Adobe Developer内容，了解更多技术详细信息](https://developer.adobe.com/uix/docs/).

## 开发扩展

按照下面列出的步骤了解如何为AEMas a Cloud Service生成、开发和部署AEM内容片段控制台扩展。

<div class="columns is-multiline">
    <!-- Create Adobe Developer Project -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Create Adobe Developer Project">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./adobe-developer-console-project.md" title="创建Adobe Developer项目" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/project/card.png" alt="创建Adobe Developer项目">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">1.创建项目</p>
                    <p class="is-size-6">创建一个Adobe Developer Console项目，以定义对其他Adobe服务的访问权限，并管理其部署。</p>
                    <a href="./adobe-developer-console-project.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">创建Adobe Developer项目</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
    <!-- Generate an Extension app -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Generate an Extension app">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./app-initialization.md" title="生成扩展应用程序" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/initialize-app/card.png" alt="初始化扩展应用程序">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">2.初始化扩展应用程序</p>
                    <p class="is-size-6">初始化一个AEM内容片段控制台扩展应用程序生成器应用程序，该应用程序定义扩展出现的位置及其执行的工作。</p>
                    <a href="./app-initialization.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">初始化扩展应用程序</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
    <!-- Extension registration -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Extension registration">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./extension-registration.md" title="延期注册" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/extension-registration/card.png" alt="延期注册">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">3.延期登记</p>
                    <p class="is-size-6">在AEM内容片段控制台中，将该扩展注册为标题菜单或操作栏扩展类型。</p>
                    <a href="./extension-registration.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">注册扩展</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
    <!-- Header Menu -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Header menu">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./header-menu.md" title="标题菜单" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/header-menu/card.png" alt="标题菜单">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">4a. 标题菜单</p>
                    <p class="is-size-6">了解如何创建AEM内容片段控制台标题菜单扩展。</p>
                    <a href="./header-menu.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">扩展标题菜单</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
    <!-- Action Bar -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Action Bar">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./action-bar.md" title="操作栏" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/action-bar/card.png" alt="操作栏">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">4b. 操作栏</p>
                    <p class="is-size-6">了解如何创建AEM内容片段控制台操作栏扩展。</p>
                    <a href="./action-bar.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">扩展操作栏</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
    <!-- Modal -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Modal">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./modal.md" title="模态" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/modal/card.png" alt="模态">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">5.模态</p>
                    <p class="is-size-6">向扩展中添加自定义模式窗口，以便用于为用户创建定制体验。 模型通常会收集用户的输入，并显示操作的结果。</p>
                    <a href="./modal.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">添加模态</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
    <!-- Adobe I/O Runtime action -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Adobe I/O Runtime action">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./runtime-action.md" title="Adobe I/O Runtime操作" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/runtime-action/card.png" alt="Adobe I/O Runtime操作">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">6.Adobe I/O Runtime行动</p>
                    <p class="is-size-6">添加无服务器Adobe I/O Runtime操作，扩展可以调用该操作与内容片段和AEM交互，以执行自定义业务操作。</p>
                    <a href="./runtime-action.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">添加Adobe I/O Runtime操作</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
    <!-- Test -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Test">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./test.md" title="测试" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/test/card.png" alt="测试">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">7.测试</p>
                    <p class="is-size-6">在开发过程中测试扩展，并使用特殊URL将已完成的扩展共享到QA或UAT测试者。</p>
                    <a href="./test.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">测试扩展</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
    <!-- Extension deployment -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Extension deployment">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./deploy.md" title="扩展部署" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/deploy/card.png" alt="扩展部署">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">8.生产部署</p>
                    <p class="is-size-6">将扩展部署到Adobe I/O，以使其可供AEM用户使用。 也可以更新和删除扩展。</p>
                    <a href="./deploy.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">部署到生产</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
</div>

## 扩展示例

示例AEM内容片段控制台扩展。

<div class="columns is-multiline">
    <!-- Bulk property update extension -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Bulk property update extension">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./example-extensions/bulk-property-update.md" title="批量属性更新扩展" tabindex="-1">
                        <img class="is-bordered-r-small" src="./example-extensions/assets/bulk-property-update/card.png" alt="批量属性更新扩展">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">批量属性更新扩展</p>
                    <p class="is-size-6">探索批量更新选定内容片段上属性的操作栏扩展示例。</p>
                    <a href="./example-extensions/bulk-property-update.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">探索扩展示例</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
    <!-- Image Generartion update extension -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="OpenAI-based image generation and upload to AEM extension">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./example-extensions/image-generation-and-image-upload.md" title="基于OpenAI的图像生成和上传到AEM扩展" tabindex="-1">
                        <img class="is-bordered-r-small" src="./example-extensions/assets/digital-image-generation/screenshot.png" alt="基于OpenAI的图像生成和上传到AEM扩展">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">基于OpenAI的图像生成和上传到AEM扩展</p>
                    <p class="is-size-6">探索一个示例操作栏扩展，该扩展使用OpenAI生成图像，将其上传到AEM并更新所选内容片段上的图像属性。</p>
                    <a href="./example-extensions/image-generation-and-image-upload.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">探索扩展示例</span>
                    </a>
                </div>
            </div>
        </div>
    </div>



</div>
