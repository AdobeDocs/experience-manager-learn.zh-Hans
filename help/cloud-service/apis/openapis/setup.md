---
title: 设置基于 OpenAPI 的 AEM API
description: 了解如何设置您的 AEM as a Cloud Service 环境，以允许访问基于 OpenAPI 的 AEM API。
version: Experience Manager as a Cloud Service
feature: Developing
topic: Development, Architecture, Content Management
role: Developer, Leader
level: Beginner
doc-type: Article
jira: KT-17426
thumbnail: KT-17426.jpeg
last-substantial-update: 2025-02-28T00:00:00Z
duration: 0
exl-id: 1df4c816-b354-4803-bb6c-49aa7d7404c6
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '1859'
ht-degree: 9%

---

# 设置基于 OpenAPI 的 AEM API

了解如何设置您的 AEM as a Cloud Service 环境，以允许访问基于 OpenAPI 的 AEM API。

在此示例中，使用&#x200B;**服务器到服务器**&#x200B;身份验证方法的&#x200B;**AEM Assets API**&#x200B;用于演示基于OpenAPI的AEM API设置过程。 可以按照类似的步骤设置[其他基于OpenAPI的AEM API](https://developer.adobe.com/experience-cloud/experience-manager-apis/#openapi-based-apis)。

>[!VIDEO](https://video.tv.adobe.com/v/3457510?quality=12&learn=on)

高级设置过程涉及以下步骤：

1. AEM as a Cloud Service环境的现代化。
1. 启用AEM API访问。
1. 创建Adobe Developer Console (ADC)项目。
1. 配置ADC项目。
1. 配置AEM实例以启用ADC项目通信。

## 先决条件

- 对Cloud Manager和AEM as a Cloud Service环境的访问权限
- 访问Adobe Developer Console (ADC)。
- 用于在`api.yaml`文件中添加或更新API配置的AEM项目。

## AEM as a Cloud Service环境的现代化{#modernization-of-aem-as-a-cloud-service-environment}

AEM as a Cloud Service环境的现代化是每个环境一次&#x200B;**的活动**，其中涉及以下步骤。 如果您已实现AEM as a Cloud Service环境的现代化，则可以跳过此步骤。

- 更新至AEM版本&#x200B;**2024.10.18459.20241031T210302Z**&#x200B;或更高版本。
- 如果环境是在版本2024.10.18459.20241031T210302Z之前创建的，请向其中添加新的产品配置文件。

### 更新AEM实例{#update-aem-instance}

- 要更新AEM实例，请在登录到Adobe [Cloud Manager](https://my.cloudmanager.adobe.com/)后，导航到&#x200B;_环境_&#x200B;部分，选择环境名称旁边的&#x200B;_省略号_&#x200B;图标并选择&#x200B;**更新**&#x200B;选项。

![更新AEM实例](./assets/setup/update-aem-instance.png)

- 然后单击&#x200B;**提交**&#x200B;按钮并运行&#x200B;_建议_&#x200B;全栈管道。

![选择最新的AEM发行版本](./assets/setup/select-latest-aem-release.png)

在我的示例中，Fullstack管道名为&#x200B;**Dev ：： Fullstack-Deploy**，AEM环境名为&#x200B;**wknd-program-dev**。 你的名字可能不同。

### 添加新产品配置文件{#add-new-product-profiles}

- 要将新产品配置文件添加到AEM实例，请在Adobe [Cloud Manager](https://my.cloudmanager.adobe.com/)的&#x200B;_环境_&#x200B;部分中，选择环境名称旁边的&#x200B;_省略号_&#x200B;图标，然后选择&#x200B;**添加产品配置文件**&#x200B;选项。

![添加新的产品配置文件](./assets/setup/add-new-product-profiles.png)

- 通过单击环境名称旁边的&#x200B;_省略号_&#x200B;图标并选择&#x200B;**管理访问权限** > **作者配置文件**，查看新添加的产品配置文件。

- _Admin Console_&#x200B;窗口显示新添加的产品配置文件。 根据您的AEM权利(如AEM Assets、AEM Sites、AEM Forms等)，您可能会看到不同的产品配置文件。 例如，我拥有AEM Assets和Sites权限，因此我可以看到以下产品配置文件。

![查看新产品配置文件](./assets/setup/review-new-product-profiles.png)

- 以上步骤完成了AEM as a Cloud Service环境的现代化。

## 启用AEM API访问{#enable-aem-apis-access}

存在&#x200B;_新产品配置文件_&#x200B;可在[Adobe Developer Console (ADC)](https://developer.adobe.com/)中启用基于OpenAPI的AEM API访问。 如果没有这些产品配置文件，您便无法在Adobe Developer Console (ADC)中设置基于OpenAPI的AEM API。

新添加的产品配置文件与&#x200B;_服务_&#x200B;关联，这些服务代表&#x200B;_具有预定义访问控制列表(ACL)_&#x200B;的AEM用户组。 _服务_&#x200B;用于控制对AEM API的访问级别。 您还可以选择或取消选择与产品配置文件关联的&#x200B;_服务_，以减少或增加访问级别。

通过单击产品配置文件名称旁边的&#x200B;_查看详细信息_&#x200B;图标来查看关联。 在以下屏幕截图中，您可以看到&#x200B;**AEM Sites内容管理器 — 作者 — 项目XXX — 环境XXX**&#x200B;产品配置文件与&#x200B;**AEM Sites内容管理器**&#x200B;服务的关联。 查看其他产品配置文件及其与“服务”的关联。

![查看与产品配置文件关联的服务](./assets/setup/review-services-associated-with-product-profile.png)

### 启用AEM Assets API访问{#enable-aem-assets-apis-access}

在此示例中，**AEM Assets API**&#x200B;用于演示基于OpenAPI的AEM API设置过程。 但是，默认情况下，**AEM Assets API Users**&#x200B;服务不与任何产品配置文件关联。 您需要将其与所需的产品配置文件关联。

让我们将其与新添加的&#x200B;**AEM Assets Collaborator Users - author - Program XXX - Environment XXX** Product Profile或任何其他要用于AEM Assets API访问的产品配置文件关联。

![将AEM Assets API Users服务与产品配置文件关联](./assets/setup/associate-aem-assets-api-users-service-with-product-profile.png)

### 启用服务器到服务器身份验证

要为所需的基于OpenAPI的AEM API启用服务器到服务器身份验证，使用Adobe Developer Console (ADC)设置集成的用户必须作为开发人员添加到与&#x200B;_服务_&#x200B;关联的&#x200B;_产品配置文件_。

例如，要为AEM Assets API启用服务器到服务器身份验证，必须将用户作为开发人员添加到&#x200B;**AEM Assets Collaborator Users - author - Program XXX - Environment XXX** _产品配置文件_。

![将开发人员关联到产品配置文件](./assets/setup/associate-developer-to-product-profile.png)

在此关联后，ADC项目的&#x200B;_资产创作API_&#x200B;可以设置所需的服务器到服务器身份验证，并将ADC项目（在下一步中创建）中的身份验证帐户与产品配置文件关联。

>[!IMPORTANT]
>
>要为所需的AEM API启用服务器到服务器身份验证，上述步骤至关重要。 如果没有此关联，AEM API将无法与服务器到服务器身份验证方法一起使用。

通过执行上述所有步骤，您已准备AEM as a Cloud Service环境以启用基于OpenAPI的AEM API访问。 接下来，您需要创建Adobe Developer Console (ADC)项目以设置基于OpenAPI的AEM API。

## 创建Adobe Developer Console (ADC)项目{#adc-project}

Adobe Developer Console (ADC)项目用于设置基于OpenAPI的AEM API。 请记住，[Adobe Developer Console (ADC)](./overview.md#accessing-adobe-apis-and-related-concepts)是用于访问Adobe API、SDK、实时事件、无服务器函数等的开发人员中心。

ADC项目用于添加所需的API、设置其身份验证并将身份验证帐户与产品配置文件关联。

创建ADC项目：

1. 使用您的Adobe ID登录[Adobe Developer Console](https://developer.adobe.com/console)。

   ![Adobe Developer Console](./assets/setup/adobe-developer-console.png)

1. 在&#x200B;_快速入门_&#x200B;部分中，单击&#x200B;**新建项目**&#x200B;按钮。

   ![创建新项目](./assets/setup/create-new-project.png)

1. 这会创建一个具有默认名称的新项目。

   ![新项目已创建](./assets/setup/new-project-created.png)

1. 通过单击右上角的&#x200B;**编辑项目**&#x200B;按钮编辑项目名称。 提供一个有意义的名称，然后单击&#x200B;**保存**。

   ![编辑项目名称](./assets/setup/edit-project-name.png)

## 配置ADC项目{#configure-adc-project}

创建ADC项目后，您必须添加所需的AEM API，设置其身份验证，并将身份验证帐户与产品配置文件关联。

在本例中，**AEM Assets API**&#x200B;用于演示基于OpenAPI的AEM API设置过程。 但是，您可以按照类似的步骤添加其他基于OpenAPI的AEM API，如&#x200B;**AEM Sites API**、**AEM Forms API**&#x200B;等。 AEM权限决定Adobe Developer Console (ADC)中可用的API。

1. 要添加AEM API，请单击&#x200B;**添加API**&#x200B;按钮。

   ![添加API](./assets/s2s/add-api.png)

1. 在&#x200B;_添加API_&#x200B;对话框中，按&#x200B;_Experience Cloud_&#x200B;进行筛选，然后选择所需的AEM API。 例如，在本例中，选择了&#x200B;_资产创作API_。

   ![添加AEM API](./assets/s2s/add-aem-api.png)

   >[!TIP]
   >
   >    如果所需的&#x200B;**AEM API卡**&#x200B;已禁用，_为什么禁用了此卡？_&#x200B;信息显示&#x200B;**需要许可证**&#x200B;消息，原因之一可能是您没有使您的AEM as a Cloud Service环境现代化，有关详细信息，请参阅[AEM as a Cloud Service环境现代化](#modernization-of-aem-as-a-cloud-service-environment)。

1. 接下来，在&#x200B;_配置API_&#x200B;对话框中，选择所需的身份验证选项。 例如，在本例中，选择了&#x200B;**服务器到服务器**&#x200B;身份验证选项。

   ![选择身份验证](./assets/s2s/select-authentication.png)

   服务器到服务器身份验证非常适用于需要API访问而不需要用户交互的后端服务。 Web应用程序和单页应用程序身份验证选项适用于需要代表用户进行API访问的应用程序。 有关详细信息，请参阅[OAuth服务器到服务器与Web应用程序与单页应用程序凭据之间的区别](./overview.md#difference-between-oauth-server-to-server-vs-web-app-vs-single-page-app-credentials)。

   >[!TIP]
   >
   >如果您看不到服务器到服务器身份验证选项，则意味着设置集成的用户不会作为开发人员添加到与服务相关联的产品配置文件中。 有关详细信息，请参阅[启用服务器到服务器身份验证](#enable-server-to-server-authentication)。


1. 如果需要，可以重命名API以便于识别。 出于演示目的，使用默认名称。

   ![重命名凭据](./assets/s2s/rename-credential.png)

1. 在这种情况下，身份验证方法是&#x200B;**OAuth服务器到服务器**，因此您需要将身份验证帐户与产品配置文件关联。 选择&#x200B;**AEM Assets Collaborator Users - author - Program XXX - Environment XXX** Product Profile，然后单击&#x200B;**保存**。

   ![选择产品配置文件](./assets/s2s/select-product-profile.png)

1. 审查AEM API和身份验证配置。

   ![AEM API配置](./assets/s2s/aem-api-configuration.png)

   ![身份验证配置](./assets/s2s/authentication-configuration.png)

如果您选择&#x200B;**OAuth Web应用程序**&#x200B;或&#x200B;**OAuth单页应用程序**&#x200B;身份验证方法，则不会提示产品配置文件关联，但需要应用程序重定向URI。 应用程序重定向URI用于在通过授权码进行身份验证后将用户重定向到应用程序。 相关用例教程概述了此类特定于身份验证的配置。

## 配置AEM实例以启用ADC项目通信{#configure-aem-instance}

接下来，您需要配置AEM实例以启用上述ADC项目通信。 使用此配置，ADC项目的ClientID无法与AEM实例通信，导致出现403禁止错误。 将此配置视为防火墙规则，以便仅允许允许的ClientID与AEM实例通信。

让我们按照配置AEM实例的步骤启用上述ADC项目通信。

1. 在本地计算机上，导航到AEM项目（或者克隆项目，如果没有），然后找到`config`文件夹。

1. 在AEM项目中，从`api.yaml`文件夹中找到或创建`config`文件。 在我的案例中，[AEM WKND Sites项目](https://github.com/adobe/aem-guides-wknd)用于演示基于OpenAPI的AEM API设置过程。

   ![查找API YAML](./assets/setup/locate-api-yaml.png)

1. 将以下配置添加到`api.yaml`文件，以允许ADC项目的ClientID与AEM实例通信。

   ```yaml
   kind: "API"
   version: "1.0"
   metadata: 
       envTypes: ["dev", "stage", "prod"]
   data:
       allowedClientIDs:
           author:
           - "<ADC Project's Credentials ClientID>"
   ```

   将`<ADC Project's Credentials ClientID>`替换为ADC项目凭据值的实际ClientID。 本教程中使用的API端点仅在创作层上可用，但对于其他API，yaml配置还可以具有&#x200B;_发布_&#x200B;或&#x200B;_预览_&#x200B;节点。

   >[!CAUTION]
   >
   > 出于演示目的，所有环境均使用相同的ClientID。 建议为每个环境（开发、暂存、生产）使用单独的ClientID，以便更好地进行安全和控制。

1. 提交配置更改并将更改推送到Cloud Manager管道所连接的远程Git存储库。

1. 在Cloud Manager中使用[配置管道](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines#config-deployment-pipeline)部署上述更改。

   ![部署YAML](./assets/setup/config-pipeline.png)

请注意，`api.yaml`文件也可以使用命令行工具[安装在](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-learn/cloud-service/developing/rde/overview)RDE[、](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-learn/cloud-service/developing/rde/how-to-use#deploy-configuration-yaml-files)中。 这有助于在将配置更改部署到生产环境之前测试这些更改。

## 后续步骤

一旦将AEM实例配置为启用ADC项目通信，您就可以开始使用基于OpenAPI的AEM API。 了解如何使用不同的 OAuth 身份验证方法，来使用基于 OpenAPI 的 AEM API：

<!-- CARDS
{target = _self}

* ./use-cases/invoke-api-using-oauth-s2s.md
  {title = Invoke API using Server-to-Server authentication}
  {description = Learn how to invoke OpenAPI-based AEM APIs from a custom NodeJS application using OAuth Server-to-Server authentication.}
  {image = ./assets/s2s/OAuth-S2S.png}
* ./use-cases/invoke-api-using-oauth-web-app.md
  {title = Invoke API using Web App authentication}
  {description = Learn how to invoke OpenAPI-based AEM APIs from a custom web application using OAuth Web App authentication.}
  {image = ./assets/web-app/OAuth-WebApp.png}
* ./use-cases/invoke-api-using-oauth-single-page-app.md
  {title = Invoke API using Single Page App authentication}
  {description = Learn how to invoke OpenAPI-based AEM APIs from a custom Single Page App (SPA) using OAuth 2.0 PKCE flow.}
  {image = ./assets/spa/OAuth-SPA.png}  
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Invoke API using Server-to-Server authentication">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./use-cases/invoke-api-using-oauth-s2s.md" title="使用服务器到服务器身份验证调用 API" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/s2s/OAuth-S2S.png" alt="使用服务器到服务器身份验证调用 API"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./use-cases/invoke-api-using-oauth-s2s.md" target="_self" rel="referrer" title="使用服务器到服务器身份验证调用 API">使用服务器到服务器身份验证调用 API</a>
                    </p>
                    <p class="is-size-6">了解如何使用 OAuth 服务器到服务器身份验证从自定义 NodeJS 应用程序调用基于 OpenAPI 的 AEM API。</p>
                </div>
                <a href="./use-cases/invoke-api-using-oauth-s2s.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">了解详情</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Invoke API using Web App authentication">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./use-cases/invoke-api-using-oauth-web-app.md" title="使用 Web 应用身份验证调用 API" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/web-app/OAuth-WebApp.png" alt="使用 Web 应用身份验证调用 API"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./use-cases/invoke-api-using-oauth-web-app.md" target="_self" rel="referrer" title="使用 Web 应用身份验证调用 API">使用 Web 应用身份验证调用 API</a>
                    </p>
                    <p class="is-size-6">了解如何使用 OAuth Web App 身份验证从自定义 Web 应用程序调用基于 OpenAPI 的 AEM API。</p>
                </div>
                <a href="./use-cases/invoke-api-using-oauth-web-app.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">了解详情</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Invoke API using Single Page App authentication">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./use-cases/invoke-api-using-oauth-single-page-app.md" title="使用单页应用程序身份验证调用 API" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/spa/OAuth-SPA.png" alt="使用单页应用程序身份验证调用 API"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./use-cases/invoke-api-using-oauth-single-page-app.md" target="_self" rel="referrer" title="使用单页应用程序身份验证调用 API">使用单页应用程序身份验证调用 API</a>
                    </p>
                    <p class="is-size-6">了解如何使用OAuth 2.0 PKCE流程从自定义单页应用程序(SPA)调用基于OpenAPI的AEM API。</p>
                </div>
                <a href="./use-cases/invoke-api-using-oauth-single-page-app.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">了解详情</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->
