---
title: AEM中的MCP服务器
description: 了解如何从首选的AI支持IDE或基于Chat的应用程序中使用AEM模型上下文协议(MCP)服务器来简化和加速AEM内容工作。
version: Experience Manager as a Cloud Service
role: Leader, User, Developer
level: Beginner
doc-type: Article
duration: 0
last-substantial-update: 2026-03-04T00:00:00Z
jira: KT-20473
exl-id: 7f2e4e37-6440-423e-9ba9-9228fe03600b
source-git-commit: ac44a73d2b63dba5292393730c712aec68ddea6c
workflow-type: tm+mt
source-wordcount: '877'
ht-degree: 0%

---

# AEM中的MCP服务器

了解如何使用您首选的AI支持IDE或基于Chat的应用程序中的AEM _模型上下文协议(MCP)服务器_&#x200B;来简化和加速AEM内容工作。 您可以使用自然语言描述所需的内容，而不是编写低级API代码或在AEM UI中导航。

## AEM MCP服务器列表

所有AEM MCP服务器在`https://mcp.adobeaemcloud.com/adobe/mcp/`下均可用。 有关详细信息，请参阅[将MCP用于AEM as a Cloud Service](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-cloud-service/content/ai-in-aem/using-mcp-with-aem-as-a-cloud-service)。

- **内容** (`/content`) — 具有创建、读取、更新和删除页面、片段和资产的完全访问权限。
- **内容（只读）** (`/content-readonly`) — 以只读方式列出和获取页面、片段和资产（无更改）。
- **Cloud Manager** (`/cloudmanager`) — 管理Adobe Cloud Manager程序、环境、存储库和管道。

>[!TIP]
>
>每台服务器公开的工具可能会随着时间的推移而改变。 要查看现在可用的内容，请让您的AI列出所有AEM MCP工具（例如，`List all AEM MCP tools available from this server and describe what they do`），或在IDE中键入`tools/list`提示符。

## MCP服务器的使用模式

在开始使用AEM MCP服务器之前，让我们先了解一下MCP服务器的两种主要使用模式：

- **以人为中心** — 您坐在驾驶席上。 您会问，AI会在IDE中为您建议或运行工具。
- **代理** — 代理应用程序（代理或子代理）自行调用服务器，选择工具并努力实现很少人为输入的目标。

以下是这两种使用模式的比较结果：

| 方面 | 以人为中心 | 无代理 |
| ------ | ------------- | ------- |
| **谁在驱动操作** | 你。 <br> AI在IDE或基于Chat的应用程序中为您建议或运行工具。 | 人工智能。 <br>它选取要使用的工具并以最小的指导持续进行。 |
| **决策机构** | 你控制一切。 您批准或触发每个步骤。 | 人工智能拥有更多自由。 高影响力的行动可能需要护栏或批准。 |
| **典型使用模式** | **每个开发人员**，您从自己的IDE或基于聊天的应用程序中使用它，每个会话一个开发人员，这有助于日常开发工作。 | **通过代理应用程序共享**，作为许多用户或代理的共享服务和网关。 |
| **最适合** | 查看内容、进行引导式更新、浏览或重复任务，同时保持循环。 | 代理工作流、批处理作业、管道以及系统应在最少干预下运行的目标。 |

### 在代理系统中使用MCP时

MCP服务器是为具有交互式UX和人为监督的&#x200B;**人工操作的MCP客户端**&#x200B;设计的。 MCP工具规范建议&#x200B;_可以批准或拒绝工具调用循环中的人_。

如果在代理或自治系统中使用MCP服务器，请将其视为单独的兼容性层。 列入允许列表在&#x200B;**提示**、_提示_&#x200B;或&#x200B;_路由逻辑_&#x200B;中不要对&#x200B;_工具名称进行硬编码_。 在MCP中，_工具名称_&#x200B;是程序化标识符，_描述_&#x200B;是LLM的面向模型的提示。 基于提示和选择的首选功能或描述。

通过`tools/list`实施运行时发现，处理工具列表更改(`notifications/tools/list_changed`)，并在需要超出协议基线的稳定性保证时与MCP服务器提供商协调上线和版本控制。

## MCP实体及其映射

MCP是围绕三个实体生成的： **主机**、**客户端**&#x200B;和&#x200B;**服务器**。 [MCP规范](https://modelcontextprotocol.io/docs/getting-started/intro)正式定义了它们。 但是，下表以简单的术语解释了每种协议以及使用AEM MCP服务器时的映射。

| 组件 | 标准定义 | 使用AEM MCP服务器时 |
| --------- | ------------------- | ---------------- |
| **主机** | 这个应用程序运行一切，收集上下文，与人工智能交谈，处理权限，并创建客户。 | 您的&#x200B;**IDE** (Cursor)或基于Chat的应用程序是主机。 它运行MCP客户端并决定您的会话可以使用哪些工具和服务器。 |
| **客户端** | 从主机到一台服务器的单个连接。 它来回传递消息，并将服务器的访问与其他服务器分开。 | **MCP客户端**&#x200B;位于您的IDE或基于聊天的应用程序中。 在设置中添加AEM Content MCP Server时，基于IDE或Chat的应用程序将创建一个与该服务器对话的客户端。 您的提示和工具调用通过此客户端。 |
| **服务器** | 通过MCP公开工具、数据和提示的服务。 它可以在您的计算机上运行，也可以远程运行。 | Adobe托管的&#x200B;**AEM MCP服务器**&#x200B;提供了用于创建、读取、更新和删除页面、内容片段和资产的工具，以便您的IDE或基于Chat的应用程序中的AI可以与您的AEM环境配合使用。 |

简言之，**主机**&#x200B;是您的IDE或基于Chat的应用程序，**客户端**&#x200B;是从IDE或基于Chat的应用程序到AEM的连接，**服务器**&#x200B;是Adobe托管的AEM MCP服务器，可执行此项工作。

## 设置

AEM MCP服务器设计为可与定义的一组兼容MCP的应用程序配合使用。
若要在首选的IDE或基于聊天应用程序中设置AEM MCP服务器，请参阅[支持的MCP应用程序](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-cloud-service/content/ai-in-aem/mcp-support/using-mcp-with-aem-as-a-cloud-service#supported-mcp-applications)以了解详细信息。

## 用例

<!-- CARDS
{target = _self}

* ./accelerate-content-operations-with-aem-mcp-server.md    
  {title = Accelerate Content Operations with AEM MCP Server}
  {description = Learn how to use the AEM Content MCP Server from Cursor IDE to streamline and accelerate your AEM content work.}
  {image = ../assets/content-mcp-server/update-adventure-price-prompt-response.png}
  {cta = Learn Content MCP Server}

* ./cloud-manager.md
  {title = Cloud Manager MCP Server}
  {description = Learn how to use the AEM Cloud Manager MCP Server from Cursor IDE to streamline and accelerate your AEM cloud manager work.}
  {image = ../assets/cm-mcp-server/start-pipeline.png}
  {cta = Learn Cloud Manager MCP Server}
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Accelerate Content Operations with AEM MCP Server">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./accelerate-content-operations-with-aem-mcp-server.md" title="使用AEM MCP Server加速内容操作" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="../assets/content-mcp-server/update-adventure-price-prompt-response.png" alt="使用AEM MCP Server加速内容操作"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./accelerate-content-operations-with-aem-mcp-server.md" target="_self" rel="referrer" title="使用AEM MCP Server加速内容操作">使用AEM MCP服务器加速内容操作</a>
                    </p>
                    <p class="is-size-6">了解如何使用Cursor IDE中的AEM Content MCP Server简化和加速AEM内容工作。</p>
                </div>
                <a href="./accelerate-content-operations-with-aem-mcp-server.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">了解Content MCP服务器</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Cloud Manager MCP Server">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./cloud-manager.md" title="Cloud Manager MCP服务器" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="../assets/cm-mcp-server/start-pipeline.png" alt="Cloud Manager MCP服务器"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./cloud-manager.md" target="_self" rel="referrer" title="Cloud Manager MCP服务器">Cloud Manager MCP服务器</a>
                    </p>
                    <p class="is-size-6">了解如何使用Cursor IDE中的AEM Cloud Manager MCP服务器来简化和加速AEM Cloud Manager工作。</p>
                </div>
                <a href="./cloud-manager.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">了解Cloud Manager MCP服务器</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->
