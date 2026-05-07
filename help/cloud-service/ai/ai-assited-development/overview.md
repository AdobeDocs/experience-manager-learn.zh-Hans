---
title: AI辅助开发
description: 了解AI辅助开发，该开发使用AI支持的IDE或编码代理以及AGENTS.md、Agent Skills和MCP服务器，帮助为AEM as a Cloud Service上的项目生成高质量的生产就绪代码。
version: Experience Manager as a Cloud Service
feature: Developer Tools
role: Developer
level: Beginner
doc-type: Article
duration: 0
last-substantial-update: 2026-04-24T00:00:00Z
jira: KT-20899
thumbnail: KT-20899.pngKT-20899
exl-id: 19b7ab0b-2f47-434a-a141-17701f432fac
source-git-commit: 6f303c8fbec523227716fe0bc1bff8fceffad1f9
workflow-type: tm+mt
source-wordcount: '906'
ht-degree: 0%

---

# AI辅助开发

AI辅助开发使用AI支持的IDE或编码代理以及`AGENTS.md`、代理技能和MCP服务器来帮助为AEM as a Cloud Service项目生成高质量、可随时投入生产的代码。

Visual Studio Code[&#128279;](https://code.visualstudio.com/docs/copilot/overview)、[Claude Code](https://code.claude.com/docs/en/overview)中的工具（如[Cursor](https://www.cursor.com/)、GitHub Copilot）以及类似的AI支持的IDE和编码代理在以下几个关键方面有所帮助：

- **更快的迭代**：从描述所需功能或更改的自然语言提示生成或重构代码。
- **学习辅助**：在出现提示时解释不熟悉的代码路径、配置、概念或最佳实践。

但是，这些优点在很大程度上取决于编码代理&#x200B;_可用的_&#x200B;上下文。 通用培训数据和单个存储库快照通常不足&#x200B;__&#x200B;而无法可靠地生成可用于生产环境的AEM代码。

## 为什么仅靠人工智能是不够的

如果没有正确的上下文，AI模型（通过AI支持的IDE或编码代理）可以：

- **幻觉API或生命周期**：建议不符合AEM as a Cloud Service最佳实践或最新功能的代码或配置。
- **缺少过程步骤**：忽略代码存储库或培训数据中不可见的所需步骤。
- **偏离项目标准**：忽略已建立的组件、OSGi服务、工作流或Dispatcher配置模式。

此间隙是使&#x200B;_结构化上下文_ (Agent Skills and AGENTS.md)和&#x200B;_运行时可见性_ （MCP服务器）成为使AI辅助开发&#x200B;_富有成效_&#x200B;和&#x200B;_可靠_&#x200B;的必要条件。

## Adobe如何帮助进行AI辅助开发

对于AEM as a Cloud Service项目，Adobe提供：

- 通过[AI编码代理的Adobe Skills的代理技能和AGENTS.md](https://github.com/adobe/skills)
- 通过[软件分发](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=mcp*&1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&1_group.propertyvalues.operation=equals&1_group.propertyvalues.0_values=software-type%3Atooling&orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&orderby.sort=desc&layout=list&p.offset=0&p.limit=3)门户为AEM SDK和本地Dispatcher提供本地MCP服务器
- Adobe托管的AEM MCP服务器，用于IDE或聊天应用程序中的内容和Cloud Manager工作流 — 请参阅AEM中的[MCP服务器](../mcp/overview.md)

以下部分总结了每个项目。 使用此页面末尾的&#x200B;**设置**&#x200B;和&#x200B;**用例**&#x200B;部分进行安装和演练，以进行AI辅助开发。

## 座席技能是什么

代理技能是&#x200B;_过程知识或专业知识_，可帮助编码代理&#x200B;_可靠地执行实际工作_。 有关详细信息，请参阅[代理技能](https://agentskills.io)。

对于AEM as a Cloud Service项目，[AI编码代理的Adobe技能](https://github.com/adobe/skills)存储库中提供了代理技能。

## 什么是AGENTS.md

AGENTS.md提供了&#x200B;_上下文和说明_，以帮助编码代理&#x200B;_处理您的项目_。 有关详细信息，请参阅[AGENTS.md](https://agents.md/)。

对于AEM as a Cloud Service项目，当缺少&#x200B;**时，`ensure-agents-md`引导技能会在存储库根**&#x200B;处创建&#x200B;**AGENTS.md**。 该技能会检查您的项目（例如，根`pom.xml`和模块）并生成定制的指南，而不是使用静态文件。 如果&#x200B;**AGENTS.md**&#x200B;已存在，则&#x200B;**不会**&#x200B;被覆盖。

文件存在后，您可以对其进行编辑以添加更多上下文和说明，以了解您的团队或组织的最佳实践。 该技能还可以创建引用&#x200B;**AGENTS.md**&#x200B;的&#x200B;**CLAUDE.md**，以便基于Claude的工具获得相同的指导。

## MCP服务器是什么

MCP服务器通过[模型上下文协议](https://modelcontextprotocol.io/)向编码代理公开工具和数据，该协议支持调试、检查、执行和验证更改等操作。 MCP服务器可以在您的工作站（**本地**）上运行，也可以作为托管服务（**远程**）运行。

对于针对AEM SDK和Dispatcher的&#x200B;**本地开发**，请从[软件分发](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=mcp*&1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&1_group.propertyvalues.operation=equals&1_group.propertyvalues.0_values=software-type%3Atooling&orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&orderby.sort=desc&layout=list&p.offset=0&p.limit=3)门户安装这些&#x200B;**本地MCP服务器**：

- **AEM Quickstart本地MCP服务器**：公开本地AEM SDK实例的实时运行时数据，以支持故障排除和开发。 有关详细信息，请参阅[AEM快速入门MCP服务器](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-cloud-service/content/ai-in-aem/local-development-with-ai-tools#aem-quickstart-mcp-server)。
- **Dispatcher本地MCP服务器**：启用本地Dispatcher实例的运行时验证和检查。 有关详细信息，请参阅[Dispatcher MCP服务器](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-cloud-service/content/ai-in-aem/local-development-with-ai-tools#dispatcher-mcp-server)。

对于Adobe托管的AEM MCP服务器（例如，内容、只读内容和Cloud Manager），请参阅AEM中的[MCP服务器](../mcp/overview.md)。

## 设置

<!-- 
CARDS
{target = _self}

* ./setup/agent-skills.md
    {title = Set up AEM Agent Skills}
    {description = Learn how to set up AEM Agent Skills for AI-assisted development.}
    {image = ./assets/agent-skills/select-aem-agent-skills-to-install.png}
    {cta = Install AEM Agent Skills}

-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Set up AEM Agent Skills">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./setup/agent-skills.md" title="设置AEM Agent技能" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/agent-skills/select-aem-agent-skills-to-install.png" alt="设置AEM Agent技能"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./setup/agent-skills.md" target="_self" rel="referrer" title="设置AEM Agent技能">设置AEM代理技能</a>
                    </p>
                    <p class="is-size-6">了解如何设置AEM代理技能以进行人工智能辅助开发。</p>
                </div>
                <a href="./setup/agent-skills.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">安装AEM代理技能</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->

## 用例

<!-- 
CARDS
{target = _self}

* ./use-cases/component-development.md    
    {title = Create AEM Component with AI-assisted development}
    {description = Learn how to use AI-assisted development to develop AEM components.}
    {image = ./assets/component-development/review-generated-code.png}
    {cta = Create AEM Component}
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Create AEM Component with AI-assisted development">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./use-cases/component-development.md" title="使用人工智能辅助开发创建AEM组件" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/component-development/review-generated-code.png" alt="使用人工智能辅助开发创建AEM组件"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./use-cases/component-development.md" target="_self" rel="referrer" title="使用人工智能辅助开发创建AEM组件">使用AI辅助开发创建AEM组件</a>
                    </p>
                    <p class="is-size-6">了解如何使用人工智能辅助开发来开发AEM组件。</p>
                </div>
                <a href="./use-cases/component-development.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">创建AEM组件</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->

## 其他资源

- [使用AI工具进行本地开发](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-cloud-service/content/ai-in-aem/local-development-with-ai-tools)

- [面向AI编码代理的Adobe技能](https://github.com/adobe/skills)

- [AGENTS.md](https://agents.md/)

- [座席技能](https://agentskills.io/home)
