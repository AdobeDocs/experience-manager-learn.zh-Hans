---
title: 设置本地开发环境
description: 为通过 Edge Delivery Services 投放且可通过通用编辑器编辑的网站设置本地开发环境。
version: Experience Manager as a Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Tutorial
jira: KT-15832
duration: 700
exl-id: 187c305a-eb86-4229-9896-a74f5d9d822e
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '994'
ht-degree: 100%

---

# 设置本地开发环境

本地开发环境对于快速开发由 Edge Delivery Services 提供的网站至关重要。该环境在从 Edge Delivery Services 获取内容的同时，使用本地开发的代码，使开发人员能够即时查看代码更改。这样的设置支持快速、迭代的开发和测试。

Edge Delivery Services 网站项目的开发工具和流程旨在让 Web 开发人员感到熟悉，并提供快速高效的开发体验。

## 开发拓扑

本视频概述了 Edge Delivery Services 网站项目的开发拓扑，该项目可通过通用编辑器进行编辑。

>[!VIDEO](https://video.tv.adobe.com/v/3443988/?learn=on&enablevpops&captions=chi_hans)

+++ 查看更多有关开发拓扑的详细信息

- **GitHub 存储库**：
   - **用途**：托管网站的代码（CSS 和 JavaScript）。
   - **结构**：**主分支**&#x200B;包含已准备好用于生产的代码，其他分支则存放开发中的代码。
   - **功能**：任何分支的代码都可以在&#x200B;**生产环境**&#x200B;或&#x200B;**预览环境**&#x200B;中运行，而不会影响实时网站。

- **AEM Author 服务**：
   - **用途**：作为规范的内容存储库，用于编辑和管理网站内容。
   - **功能**：内容由&#x200B;**通用编辑器**&#x200B;进行读取和写入。编辑后的内容会被发布到&#x200B;**生产**&#x200B;或&#x200B;**预览**&#x200B;环境中的 **Edge Delivery Services**。

- **通用编辑器**：
   - **目的**：提供用于编辑网站内容的所见即所得界面。
   - **功能**：读取和写入 **AEM Author 服务**。可以配置为使用 **GitHub 存储库**&#x200B;中任何分支的代码来测试和验证更改。

- **Edge Delivery Services**：
   - **生产环境**：
      - **目的**：向最终用户提供实时网站内容和代码。
      - **功能**：使用来自 **GitHub 存储库****主分支**&#x200B;的代码，提供从 **AEM Author 服务**&#x200B;发布的内容。
   - **预览环境**：
      - **目的**：提供一个用于在部署前测试和预览内容与代码的暂存环境。
      - **功能**：使用来自 **GitHub 存储库**&#x200B;任意分支的代码，提供从 **AEM Author 服务**&#x200B;发布的内容，从而实现全面测试而不影响实时网站。

- **本地开发人员环境**：
   - **目的**：允许开发人员在本地创作和测试代码（CSS 和 JavaScript）。
   - **结构**：
      - 用于基于分支开发的 **GitHub 存储库**&#x200B;本地克隆副本。
      - **AEM CLI** 作为开发服务器使用，将本地代码变更应用到&#x200B;**预览环境**，以实现热加载体验。
   - **工作流程**：开发人员在本地创作代码，将更改提交到工作分支，将分支推送到 GitHub，在&#x200B;**通用编辑器**&#x200B;中对其进行验证（使用指定的分支），并在准备好进行生产部署时将其合并到&#x200B;**主分支**&#x200B;中。

+++

## 先决条件

在开始开发之前，请在您的机器上安装以下内容：

1. [Git](https://git-scm.com/)
1. [Node.js 和 npm](https://nodejs.org)
1. [Microsoft Visual Studio Code](https://code.visualstudio.com/)（或类似的代码编辑器）

## 克隆 GitHub 存储库

将[新建代码项目章节中创建的包含 AEM Edge Delivery Services 代码项目的 GitHub 存储库](./1-new-code-project.md)克隆到本地开发环境中。

![GitHub 存储库克隆](./assets/3-local-development-environment/github-clone.png)

```bash
$ cd ~/Code
$ git clone git@github.com:<YOUR_ORG>/aem-wknd-eds-ue.git
```

在 `Code` 目录中会创建一个新的 `aem-wknd-eds-ue` 文件夹，该文件夹会作为项目的根目录。虽然项目可以克隆到计算机上的任何位置，但本教程使用 `~/Code` 作为根目录。

## 安装项目依赖项

导航到项目文件夹并使用 `npm install` 安装所需的依赖项。虽然 Edge Delivery Services 项目不使用 Webpack 或 Vite 等传统 Node.js 构建系统，但它们仍需要几个依赖项以进行本地开发。

```bash
# ~/Code/aem-wknd-eds-ue

$ npm install
```

## 安装 AEM CLI

AEM CLI 是一个 Node.js 命令行工具，其旨在简化基于 Edge Delivery Services 的 AEM 网站的开发过程，同时提供本地开发服务器，以便快速开发和测试您的网站。

要安装 AEM CLI，请运行：

```bash
# ~/Code/aem-wknd-eds-ue

$ npm install @adobe/aem-cli
```

AEM CLI 还可以使用 `npm install --global @adobe/aem-cli` 进行全局安装。

## 启动本地 AEM 开发服务器

`aem up` 命令可启动本地开发服务器，并自动打开一个浏览器窗口，以显示服务器的 URL。该服务器充当 Edge Delivery Services 环境的反向代理，可在使用本地代码库进行开发的同时，从该环境提供内容。

```bash
$ cd ~/Code/aem-wknd-eds-ue 
$ aem up

    ___    ________  ___                          __      __ 
   /   |  / ____/  |/  /  _____(_)___ ___  __  __/ /___ _/ /_____  _____
  / /| | / __/ / /|_/ /  / ___/ / __ `__ \/ / / / / __ `/ __/ __ \/ ___/
 / ___ |/ /___/ /  / /  (__  ) / / / / / / /_/ / / /_/ / /_/ /_/ / /
/_/  |_/_____/_/  /_/  /____/_/_/ /_/ /_/\__,_/_/\__,_/\__/\____/_/

info: Starting AEM dev server version x.x.x
info: Local AEM dev server up and running: http://localhost:3000/
info: Enabled reverse proxy to https://main--aem-wknd-eds-ue--<YOUR_ORG>.aem.page
```

AEM CLI 会在您的浏览器中打开网址 `http://localhost:3000/`。项目中的更改会自动在 Web 浏览器中热加载，而内容更改则[需要发布到预览环境](./6-author-block.md)并刷新 Web 浏览器。

如果网站打开时显示 404 页面，很可能是[新代码项目](./1-new-code-project.md)中更新的 [fstab.yaml 或 paths.json](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/edge-dev-getting-started#create-github-project) 配置有误，或者更改尚未提交到 `main` 分支。

## 构建 JSON 片段

使用 [AEM Boilerplate XWalk 模板](https://github.com/adobe-rnd/aem-boilerplate-xwalk)创建的 Edge Delivery Services 项目依赖于 JSON 配置，这些配置支持在通用编辑器中进行区块级创作。

- **JSON 片段**：与相关区块一起存储，并定义区块模型、定义和过滤器。
   - **模型片段**：存储在 `/blocks/example/_example.json`。
   - **定义片段**：存储在 `/blocks/example/_example.json`。
   - **过滤器片段**：存储在 `/blocks/example/_example.json`。


[AEM Boilerplate XWalk 项目模板](https://github.com/adobe-rnd/aem-boilerplate-xwalk)包含一个 [Husky](https://typicode.github.io/husky/) 预提交钩子，该钩子可检测 JSON 片段的更改，并在 `git commit` 时将它们编译到相应的 `component-*.json` 文件中。

虽然可以通过 `npm run` 手动运行以下 NPM 脚本以构建 JSON 文件，但通常无需这样做，因为 husky 的预提交钩子会自动处理。

```bash
# ~/Code/aem-wknd-eds-ue

npm run build:json
```

| NPM 脚本 | 描述 |
|--------------------|-----------------------------------------------------------------------------|
| `build:json` | 从片段构建所有 JSON 文件并将它们添加到相应的 `component-*.json` 文件。 |
| `build:json:models` | 构建区块 JSON 片段并将其编译成 `/component-models.json`。 |
| `build:json:definitions` | 构建页面 JSON 片段并将其编译成 `/component-definitions.json`。 |
| `build:json:filters` | 构建页面 JSON 片段并将其编译成 `/component-filters.json`。 |

>[!TIP]
>
> 在对片段文件进行任何更改后，运行 `npm run build:json` 以重新生成主 JSON 文件。

## 代码检查

代码检查可确保代码质量和一致性，这是在将更改合并到 `main` 分支之前，Edge Delivery Services 项目必需进行的步骤。

可以通过 `npm run` 运行 NPM 脚本，例如：

```bash
# ~/Code/aem-wknd-eds-ue

$ npm run lint
```

| NPM 脚本 | 描述 |
|------------------|--------------------------------------------------|
| `lint:js` | 运行 JavaScript 代码检查。 |
| `lint:css` | 运行 CSS 代码检查。 |
| `lint` | 运行 JavaScript 和 CSS 代码检查。 |

### 自动修复代码检查问题

您可以通过向项目的 `package.json` 添加以下 `scripts`，自动修复代码检查问题，并可通过 `npm run` 运行：

```bash
# ~/Code/aem-wknd-eds-ue

$ npm run lint:fix
```

这些脚本未预装在 AEM Boilerplate XWalk 模板中，但可以添加到 `package.json` 文件中：

>[!BEGINTABS]

>[!TAB 附加脚本]

| NPM 脚本 | 命令 | 描述 |
|------------------|------------------------------------------------|-------------------------------------------------------|
| `lint:js:fix` | `npm run lint:js -- --fix` | 自动修复 JavaScript 代码检查问题。 |
| `lint:css:fix` | `stylelint blocks/**/*.css styles/*.css -- --fix` | 自动修复 CSS 代码检查问题。 |
| `lint:fix` | `npm run lint:js:fix && npm run lint:css:fix` | 同时运行 JS 和 CSS 修复脚本，实现快速清理。 |

>[!TAB package.json 示例]

以下脚本条目可以添加到 `package.json` 的 `scripts` 数组中。

```json
{
  ...
  "scripts": [
    ...,
    "lint:js:fix": "npm run lint:js -- --fix",
    "lint:css:fix": "npm run lint:css -- --fix",
    "lint:fix": "npm run lint:js:fix && npm run lint:css:fix",
    ...
  ]
  ...
}
```

>[!ENDTABS]
