---
title: AEM內容片段主控台擴充功能
description: 瞭解如何建置和部署AEMas a Cloud Service內容片段主控台擴充功能
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

# AEM內容片段主控台擴充功能

[AEM內容片段主控台](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/content-fragments/content-fragments-console.html) 擴充功能可透過兩個擴充功能點新增： [內容片段主控台的](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/content-fragments/content-fragments-console.html) 頁首功能表或動作列。 擴充功能以JavaScript撰寫而成，可當作App Builder應用程式執行，並可實作自訂Web UI和無伺服器Adobe I/O Runtime動作，以執行更密集、長時間執行的工作。

![AEM內容片段主控台擴充功能](./assets/overview/example.png){align="center"}

| 擴充功能型別 | 描述 | 引數 |
| :--- | :--- | :--- |
| 頁首功能表 | 將按鈕新增至下列時間顯示的標題： __零__ 已選取內容片段。 | 无。 |
| 動作列 | 新增按鈕至動作列，該動作列會在 __一或多個__ 已選取內容片段。 | 所選內容片段的路徑陣列。 |

單一AEM內容片段主控台擴充功能可包含零個或一個標題功能表，以及零個或一個動作列擴充功能型別。 如果需要相同型別的多個擴充功能型別，則必須建立多個AEM內容片段主控台擴充功能。

AEM內容片段主控台擴充功能，需要 [Adobe Developer Console專案](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/extension-development/#create-a-project-in-adobe-developer-console) 和 [應用程式產生器應用程式](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/code-generation) 使用 `@adobe/aem-cf-admin-ui-ext-tpl` 範本，與Adobe Developer Console專案相關聯。

根據擴充功能的功能，在產生App Builder應用程式時從下列功能中選取。 擴充功能中可以使用任何選項組合。

|  | 新增按鈕至 [頁首功能表](./header-menu.md) | 新增按鈕至 [動作列](./action-bar.md) | 顯示 [強制回應視窗](./modal.md) | 新增 [伺服器端處理常式](./runtime-action.md) |
| ------------------------------------------ | :-----------------------: | :----------------------: | :--------: | :--------------------:  |
| 未選取內容片段時可用 | ✔ |  |  |  |
| 在選取一或多個內容片段時可用 |  | ✔ |  |  |
| 從使用者收集自訂輸入 |  |  | ✔️ |  |
| 顯示使用者的自訂意見回饋 |  |  | ✔️ |  |
| 對AEM叫用HTTP請求 |  |  |  | ✔ |
| 對Adobe/第三方服務叫用HTTP請求 |  |  |  | ✔ |


## Adobe Developer檔案

Adobe Developer包含AEM內容片段主控台擴充功能的開發人員詳細資訊。 請檢閱 [Adobe Developer內容，以取得進一步的技術詳細資訊](https://developer.adobe.com/uix/docs/).

## 開發擴充功能

請依照下列步驟操作，瞭解如何產生、開發和部署AEMas a Cloud Service的AEM內容片段主控台擴充功能。

<div class="columns is-multiline">
    <!-- Create Adobe Developer Project -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Create Adobe Developer Project">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./adobe-developer-console-project.md" title="建立Adobe Developer專案" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/project/card.png" alt="建立Adobe Developer專案">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">1.建立專案</p>
                    <p class="is-size-6">建立Adobe Developer Console專案，定義其存取其他Adobe服務的許可權，並管理其部署。</p>
                    <a href="./adobe-developer-console-project.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">建立Adobe Developer專案</span>
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
                    <a href="./app-initialization.md" title="產生擴充功能應用程式" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/initialize-app/card.png" alt="初始化擴充功能應用程式">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">2.初始化擴充功能應用程式</p>
                    <p class="is-size-6">初始化AEM內容片段主控台擴充功能App Builder應用程式，定義擴充功能出現的位置及其執行的工作。</p>
                    <a href="./app-initialization.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">初始化擴充功能應用程式</span>
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
                    <a href="./extension-registration.md" title="擴充功能註冊" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/extension-registration/card.png" alt="擴充功能註冊">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">3.延長註冊</p>
                    <p class="is-size-6">在AEM內容片段控制檯中註冊擴充功能，作為標題功能表或動作列擴充功能型別。</p>
                    <a href="./extension-registration.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">註冊擴充功能</span>
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
                    <a href="./header-menu.md" title="頁首功能表" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/header-menu/card.png" alt="頁首功能表">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">4a. 頁首功能表</p>
                    <p class="is-size-6">瞭解如何建立AEM內容片段主控台標題功能表擴充功能。</p>
                    <a href="./header-menu.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">延伸頁首功能表</span>
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
                    <p class="is-size-6">瞭解如何建立AEM內容片段主控台動作列擴充功能。</p>
                    <a href="./action-bar.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">擴充動作列</span>
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
                    <p class="headline is-size-5 has-text-weight-bold">5.強制回應視窗</p>
                    <p class="is-size-6">新增自訂強制回應至擴充功能，以便建立適合使用者的自訂體驗。 模組通常會收集使用者的輸入，並顯示作業的結果。</p>
                    <a href="./modal.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">新增強制回應視窗</span>
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
                    <a href="./runtime-action.md" title="Adobe I/O Runtime動作" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/runtime-action/card.png" alt="Adobe I/O Runtime動作">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">6. Adobe I/O Runtime動作</p>
                    <p class="is-size-6">新增無伺服器的Adobe I/O Runtime動作，讓擴充功能可以叫用，與內容片段和AEM互動，以執行自訂業務作業。</p>
                    <a href="./runtime-action.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">新增Adobe I/O Runtime動作</span>
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
                    <p class="headline is-size-5 has-text-weight-bold">7.測試</p>
                    <p class="is-size-6">在開發期間測試擴充功能，並使用特殊URL將完成的擴充功能共用給QA或UAT測試者。</p>
                    <a href="./test.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">測試擴充功能</span>
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
                    <a href="./deploy.md" title="擴充功能部署" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/deploy/card.png" alt="擴充功能部署">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">8.生產部署</p>
                    <p class="is-size-6">將擴充功能部署至Adobe I/O，以供AEM使用者使用。 擴充功能也可以更新和移除。</p>
                    <a href="./deploy.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">部署到生產</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
</div>

## 範例擴充功能

範例AEM內容片段主控台擴充功能。

<div class="columns is-multiline">
    <!-- Bulk property update extension -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Bulk property update extension">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./example-extensions/bulk-property-update.md" title="大量屬性更新擴充功能" tabindex="-1">
                        <img class="is-bordered-r-small" src="./example-extensions/assets/bulk-property-update/card.png" alt="大量屬性更新擴充功能">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">大量屬性更新擴充功能</p>
                    <p class="is-size-6">探索大量更新所選內容片段屬性的動作列擴充功能範例。</p>
                    <a href="./example-extensions/bulk-property-update.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">探索範例擴充功能</span>
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
                    <a href="./example-extensions/image-generation-and-image-upload.md" title="以OpenAI為基礎的影像產生和上傳至AEM擴充功能" tabindex="-1">
                        <img class="is-bordered-r-small" src="./example-extensions/assets/digital-image-generation/screenshot.png" alt="以OpenAI為基礎的影像產生和上傳至AEM擴充功能">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">以OpenAI為基礎的影像產生和上傳至AEM擴充功能</p>
                    <p class="is-size-6">探索使用OpenAI產生影像、上傳至AEM並更新所選內容片段上影像屬性的動作列擴充功能範例。</p>
                    <a href="./example-extensions/image-generation-and-image-upload.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">探索範例擴充功能</span>
                    </a>
                </div>
            </div>
        </div>
    </div>



</div>
