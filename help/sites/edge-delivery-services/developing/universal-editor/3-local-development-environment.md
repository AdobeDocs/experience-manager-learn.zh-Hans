---
title: 设置本地开发环境
description: 为使用Edge Delivery Services交付并使用Universal Editor可编辑的站点设置本地开发环境。
version: Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Tutorial
jira: KT-15832
duration: 700
source-git-commit: e8ce91b0be577ec6cf8f3ab07ba9ff09c7e7a6ab
workflow-type: tm+mt
source-wordcount: '925'
ht-degree: 1%

---


# 设置本地开发环境

本地开发环境对于快速开发由Edge Delivery Services交付的网站至关重要。 该环境使用本地开发的代码，同时从Edge Delivery Services获取内容，允许开发人员即时查看代码更改。 这样的设置支持快速、迭代的开发和测试。

Edge Delivery Services网站项目的开发工具和流程旨在让Web开发人员熟悉，并提供快速高效的开发体验。

## 开发拓扑

可通过Universal Editor编辑的Edge Delivery Services网站项目的开发拓扑包括以下方面：

- **GitHub存储库**：
   - **目的**：托管网站的代码(CSS和JavaScript)。
   - **结构**： **主分支**&#x200B;包含生产就绪代码，而其他分支包含工作代码。
   - **功能**：任何分支的代码都可以针对&#x200B;**生产**&#x200B;或&#x200B;**预览**&#x200B;环境运行，而不会影响实时网站。

- **AEM创作服务**：
   - **目的**：用作编辑和管理网站内容的规范内容存储库。
   - **功能**：内容由&#x200B;**通用编辑器**&#x200B;读取和写入。 已编辑的内容已发布到&#x200B;**生产**&#x200B;或&#x200B;**预览**&#x200B;环境中的&#x200B;**Edge Delivery Services**。

- **通用编辑器**：
   - **目的**：提供用于编辑网站内容的WYSIWYG界面。
   - **功能**：读取和写入&#x200B;**AEM创作服务**。 可以配置为使用&#x200B;**GitHub存储库**&#x200B;中任何分支的代码来测试和验证更改。

- **Edge Delivery Services**：
   - **生产环境**：
      - **目的**：向最终用户提供实时网站内容和代码。
      - **功能**：使用&#x200B;**GitHub存储库**&#x200B;的&#x200B;**主分支**&#x200B;的代码，为从&#x200B;**AEM Author服务**&#x200B;发布的内容提供服务。
   - **预览环境**：
      - **目的**：提供暂存环境，以便在部署之前测试和预览内容和代码。
      - **功能**：使用&#x200B;**GitHub存储库**&#x200B;的任何分支的代码提供从&#x200B;**AEM Author服务**&#x200B;发布的内容，从而在不影响实时网站的情况下进行全面测试。

- **本地开发人员环境**：
   - **目的**：允许开发人员在本地编写和测试代码(CSS和JavaScript)。
   - **结构**：
      - **GitHub存储库**&#x200B;的本地克隆，用于基于分支的开发。
      - 作为开发服务器的&#x200B;**AEM CLI**&#x200B;将本地代码更改应用于&#x200B;**预览环境**&#x200B;以进行热重新加载体验。
   - **工作流**：开发人员在本地编写代码，将更改提交到工作分支，将分支推送到GitHub，在&#x200B;**通用编辑器**（使用指定的分支）中验证它，并在准备好进行生产部署时将其合并到&#x200B;**主分支**。

## 先决条件

在开始开发之前，请在您的计算机上安装以下内容：

1. [Git](https://git-scm.com/)
1. [Node.js和npm](https://nodejs.org)
1. [Microsoft Visual Studio Code](https://code.visualstudio.com/)（或类似的代码编辑器）

## 克隆GitHub存储库

将包含AEMEdge Delivery Services代码项目的[GitHub存储库](./1-new-code-project.md)克隆到本地开发环境。

![GitHub存储库克隆](./assets/3-local-development-environment/github-clone.png)

```bash
$ cd ~/Code
$ git clone git@github.com:<YOUR_ORG>/aem-wknd-eds-ue.git
```

在`Code`目录中创建一个新的`aem-wknd-eds-ue`文件夹，该文件夹用作项目的根目录。 虽然项目可以克隆到计算机上的任何位置，但本教程使用`~/Code`作为根目录。

## 安装项目依赖项

导航到项目文件夹并使用`npm install`安装所需的依赖项。 虽然Edge Delivery Services项目不使用传统的Node.js构建系统（如Webpack或Vite），但它们仍需要多个依赖关系才能进行本地开发。

```bash
# ~/Code/aem-wknd-eds-ue

$ npm install
```

## 安装AEM CLI

AEM CLI是一种Node.js命令行工具，旨在简化基于Edge Delivery Services的AEM网站的开发，提供本地开发服务器，用于快速开发和测试您的网站。

要安装AEM CLI，请运行：

```bash
# ~/Code/aem-wknd-eds-ue

$ npm install @adobe/aem-cli
```

也可以使用`npm install --global @adobe/aem-cli`全局安装AEM CLI。

## 启动本地AEM开发服务器

`aem up`命令启动本地开发服务器，并自动打开一个浏览器窗口以显示服务器的URL。 此服务器充当Edge Delivery Services环境的反向代理，在使用本地代码库进行开发时从中提供内容。

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

AEM CLI在浏览器中打开网站： `http://localhost:3000/`。 项目中的更改将自动在Web浏览器中热重新加载，而内容更改[需要发布到预览环境](./6-author-block.md)并刷新Web浏览器。

## 构建JSON片段

使用[AEM Boilerplate XWalk模板](https://github.com/adobe-rnd/aem-boilerplate-xwalk)创建的Edge Delivery Services项目依赖在通用编辑器中启用块创作的JSON配置。

- **JSON片段**：与其关联的块一起存储，并定义块模型、定义和过滤器。
   - **模型片段**：存储在`/blocks/example/_example.json`。
   - **定义片段**：存储在`/blocks/example/_example.json`中。
   - **筛选片段**：存储在`/blocks/example/_example.json`。

NPM脚本可编译这些JSON片段，并将其放置在项目根目录的适当位置。 要构建JSON文件，请使用提供的NPM脚本。 例如，要编译所有片段，请运行：

```bash
# ~/Code/aem-wknd-eds-ue

npm run build:json
```

| NPM脚本 | 描述 |
|--------------------|-----------------------------------------------------------------------------|
| `build:json` | 从片段生成所有JSON文件，并将它们添加到相应的`component-*.json`文件。 |
| `build:json:models` | 生成块JSON片段并将它们编译到`/component-models.json`中。 |
| `build:json:definitions` | 生成页面JSON片段并将它们编译到`/component-definitions.json`中。 |
| `build:json:filters` | 生成页面JSON片段并将它们编译到`/component-filters.json`中。 |

>[!TIP]
>
> 在对片段文件进行任何更改后运行`npm run build:json`以重新生成主JSON文件。

## 林亭

Litting确保代码质量和一致性，在将更改合并到`main`分支之前必须具备此条件，以便Edge Delivery Services项目能够做到这一点。

NPM脚本可以通过`npm run`运行，例如：

```bash
# ~/Code/aem-wknd-eds-ue

$ npm run lint
```

| NPM脚本 | 描述 |
|------------------|--------------------------------------------------|
| `lint:js` | 运行JavaScript Linting检查。 |
| `lint:css` | 运行CSS筛选检查。 |
| `lint` | 运行JavaScript和CSS Linting检查。 |

### 自动修复Linting问题

通过将以下`scripts`添加到项目的`package.json`，您可以自动解决Linting问题，并且可以通过`npm run`运行：

```bash
# ~/Code/aem-wknd-eds-ue

$ npm run lint:fix
```

这些脚本未使用AEM Boilerplate XWalk模板进行预配置，但可以添加到`package.json`文件中：

>[!BEGINTABS]

>[!TAB 其他脚本]

| NPM脚本 | 命令 | 描述 |
|------------------|------------------------------------------------|-------------------------------------------------------|
| `lint:js:fix` | `npm run lint:js --fix` | 自动修复JavaScript链接问题。 |
| `lint:css:fix` | `stylelint blocks/**/*.css styles/*.css --fix` | 自动修复CSS Linting问题。 |
| `lint:fix` | `npm run lint:js:fix && npm run lint:css:fix` | 运行JS和CSS修复脚本以进行快速清理。 |

>[!TAB package.json示例]

可以将以下脚本条目添加到`package.json` `scripts`数组。

```json
{
  ...
  "scripts": [
    ...,
    "lint:js:fix": "npm run lint:js --fix",
    "lint:css:fix": "npm run lint:css --fix",
    "lint:fix": "npm run lint:js:fix && npm run lint:css:fix",
    ...
  ]
  ...
}
```

>[!ENDTABS]

