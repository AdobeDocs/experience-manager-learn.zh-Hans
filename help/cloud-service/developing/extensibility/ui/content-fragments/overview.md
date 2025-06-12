---
title: AEM 内容片段扩展
description: 了解如何构建和部署 AEM as a Cloud Service 内容片段扩展。
feature: Developer Tools
version: Experience Manager as a Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay
jira: KT-11603
thumbnail: KT-11603.png
last-substantial-update: 2023-06-02T00:00:00Z
exl-id: 9164423b-a609-4bc5-9777-112d229ae748
duration: 126
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '773'
ht-degree: 100%

---

# AEM 内容片段可扩展性

AEM 内容片段 UI 是一个功能强大的可扩展 UI，可用于管理内容片段的创建、管理和编辑。为了满足您的需求，有多个扩展点可用于自定义用户界面。根据您要扩展的用户界面类型，可用的扩展点也会有所不同。

## 内容片段控制台扩展点

AEM (Adobe Experience Manager) 中的内容片段控制台是一个用户界面，它为管理和组织内容片段提供了一个集中位置。该控制台提供了一套全面的工具和功能，用于创建、编辑、发布和跟踪内容片段，使用户能够高效地管理各种渠道和接触点中的结构化内容。

![内容片段控制台](./assets/overview/cfc.png)

[AEM 内容片段控制台](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/content-fragments/content-fragments-console.html?lang=zh-Hans)是一个可扩展的用户界面，用于列出和管理内容片段。[AEM 内容片段控制台扩展是使用 `@adobe/aem-cf-admin-ui-ext-tpl` App Builder 模板创建的](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/code-generation)。

以下内容片段控制台扩展点可用：

<div class="columns is-multiline">
      <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Action bar">
        <div class="card" style="height: 100%">
          <div class="card-image">
            <figure class="image is-16by9">
              <a href="https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/api/action-bar/" title="操作栏" tabindex="-1" target="_blank" rel="referrer">
                <img class="is-bordered-r-small" src="./assets/overview/cfc-action-bar.png" alt="操作栏">
              </a>
            </figure>
          </div>
          <div class="card-content is-padded-small">
            <div class="content">
              <p class="headline is-size-6 has-text-weight-bold"><a href="https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/api/action-bar/" title="操作栏" target="_blank" rel="referrer">操作栏</a></p>
              <p class="is-size-6">自定义选择一个或多个内容片段时的操作。</p>
              <a href="https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/api/action-bar/" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" target="_blank" rel="referrer">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">查看文档</span>
              </a>
            </div>
          </div>
        </div>
      </div>
  <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Grid columns">
    <div class="card" style="height: 100%">
      <div class="card-image">
        <figure class="image is-16by9">
          <a href="https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/api/grid-columns/" title="网格列" tabindex="-1" target="_blank" rel="referrer">
            <img class="is-bordered-r-small" src="./assets/overview/cfc-grid-columns.png" alt="网格列">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/api/grid-columns/" title="网格列" target="_blank" rel="referrer">网格列</a></p>
          <p class="is-size-6">自定义内容片段列表中显示的数据。</p>
          <a href="https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/api/grid-columns/" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" target="_blank" rel="referrer">
            <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">查看文档</span>
          </a>
        </div>
      </div>
    </div>
  </div>
  <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Header menu">
    <div class="card" style="height: 100%">
      <div class="card-image">
        <figure class="image is-16by9">
          <a href="https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/api/header-menu/" title="标头菜单" tabindex="-1" target="_blank" rel="referrer">
            <img class="is-bordered-r-small" src="./assets/overview/cfc-header-menu.png" alt="标头菜单">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/api/header-menu/" title="标头菜单" target="_blank" rel="referrer">标头菜单</a></p>
          <p class="is-size-6">自定义当未选择任何内容片段时的操作。</p>
          <a href="https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/api/header-menu/" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" target="_blank" rel="referrer">
            <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">查看文档</span>
          </a>
        </div>
      </div>
    </div>
  </div>  
</div>

## 内容片段编辑器扩展点

AEM (Adobe Experience Manager) 中的内容片段编辑器是一个用户界面组件，其允许用户创建、编辑和管理内容片段。它提供了一个直观且用户友好的环境，用于处理结构化内容，使用户能够定义和组织内容元素、应用模板、管理变体，并预览内容在不同渠道中的显示方式。内容片段编辑器简化了创建可重复使用且模块化的内容的流程，这些内容可以轻松地在多个数字体验中分发和发布。

![内容片段编辑器](./assets/overview/cfe.png)

AEM 内容片段编辑器是用于编辑内容片段的可扩展 UI。[AEM 内容片段编辑器扩展是使用 `@adobe/aem-cf-editor-ui-ext-tpl` App Builder 模板创建的](https://developer.adobe.com/uix/docs/services/aem-cf-editor/code-generation/)。

以下内容片段编辑器扩展点可用：

<div class="columns is-multiline">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Header menu">
      <div class="card" style="height: 100%">
        <div class="card-image">
          <figure class="image is-16by9">
            <a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/header-menu" title="标头菜单" tabindex="-1" target="_blank" rel="referrer">
              <img class="is-bordered-r-small" src="./assets/overview/cfe-header-menu.png" alt="标头菜单">
            </a>
          </figure>
        </div>
        <div class="card-content is-padded-small">
          <div class="content">
            <p class="headline is-size-6 has-text-weight-bold"><a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/header-menu/" title="标头菜单" target="_blank" rel="referrer">标头菜单</a></p>
            <p class="is-size-6">自定义内容片段编辑器的标头菜单中的操作。</p>
            <a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/header-menu" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" target="_blank" rel="referrer">
            <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">查看文档</span>
          </a>
          </div>
        </div>
      </div>
    </div>
  <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Rich Text Editor toolbar">
    <div class="card" style="height: 100%">
      <div class="card-image">
        <figure class="image is-16by9">
          <a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-toolbar/" title="富文本编辑器工具栏" tabindex="-1" target="_blank" rel="referrer">
            <img class="is-bordered-r-small" src="./assets/overview/cfe-rte-toolbar.png" alt="富文本编辑器工具栏">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-toolbar/" title="富文本编辑器工具栏"  target="_blank" rel="referrer">富文本编辑器工具栏</a></p>
          <p class="is-size-6">在内容片段编辑器的富文本编辑器 (RTE) 中添加自定义按钮。</p>
          <a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-toolbar/" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" target="_blank" rel="referrer">
            <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">查看文档</span>
          </a>
        </div>
      </div>
    </div>
  </div>

<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Rich Text Editor widgets">
    <div class="card" style="height: 100%">
      <div class="card-image">
        <figure class="image is-16by9">
          <a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-widgets/" title="富文本编辑器小组件" tabindex="-1"  target="_blank" rel="referrer">
            <img class="is-bordered-r-small" src="./assets/overview/cfe-rte-widgets.png" alt="富文本编辑器小组件">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-widgets/" title="富文本编辑器小组件" target="_blank" rel="referrer">富文本编辑器小组件</a></p>
          <p class="is-size-6">自定义与快捷键绑定的 RTE 中的操作。</p>
          <a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-widgets/" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" target="_blank" rel="referrer">
            <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">查看文档</span>
          </a>
        </div>
      </div>
    </div>
  </div>
  <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Rich Text Editor badges">
    <div class="card" style="height: 100%">
      <div class="card-image">
        <figure class="image is-16by9">
          <a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-badges/" title="富文本编辑器徽章" tabindex="-1" target="_blank" rel="referrer">
            <img class="is-bordered-r-small" src="./assets/overview/cfe-rte-badges.png" alt="富文本编辑器徽章">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-badges/ " title="富文本编辑器徽章" target="_blank" rel="referrer">富文本编辑器徽章</a></p>
          <p class="is-size-6">自定义 RTE 内部不可编辑的样式区块。</p>
          <a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-badges/" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" target="_blank" rel="referrer">
            <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">查看文档</span>
          </a>
        </div>
      </div>
    </div>
  </div>
</div>

## 扩展示例

欢迎来到 AEM UI 可扩展性代码示例合集！本资源旨在为您提供扩展 Adobe Experience Manager (AEM) 用户界面的实用演示和见解。无论您是想要增强 AEM 功能的开发人员，还是其他相关人员，这些代码示例都可为您提供宝贵的参考。

<div class="columns is-multiline">
  <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Bulk property update">
    <div class="card" style="height: 100%">
      <div class="card-image">
        <figure class="image is-16by9">
          <a href="./examples/console-bulk-property-update.md" title="批量属性更新" tabindex="-1">
            <img class="is-bordered-r-small" src="./assets/../examples/assets/bulk-property-update/card.png" alt="批量属性更新">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="./examples/console-bulk-property-update.md" title="批量属性更新">批量内容片段属性更新</a></p>
          <p class="is-size-6">带有模态和 Adobe I/O Runtime 操作的内容片段控制台操作栏扩展。</p>
          <a href="./examples/console-bulk-property-update.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
            <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">查看示例</span>
          </a>
        </div>
      </div>
    </div>
  </div>
  <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="OpenAI-based image generation and upload to AEM extension">
        <div class="card" style="height: 100%">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./examples/console-image-generation-and-image-upload.md" title="基于 OpenAI 的图像生成与上传至 AEM 的扩展功能" tabindex="-1">
                        <img class="is-bordered-r-small" src="./examples/assets/digital-image-generation/card.png" alt="基于 OpenAI 的图像生成与上传至 AEM 的扩展功能">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-6 has-text-weight-bold"><a href="./examples/console-image-generation-and-image-upload.md" title="基于 OpenAI 的图像生成与上传至 AEM 的扩展功能">OpenAPI 图像生成</a></p>
                    <p class="is-size-6">探索一个示例操作栏扩展，该扩展使用 OpenAI 生成图像，将其上传至 AEM，同时更新所选内容片段的图像属性。</p>
                    <a href="./examples/console-image-generation-and-image-upload.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
            <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">查看示例</span>
          </a>
                </div>
            </div>
        </div>
    </div>    
  <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Custom columns">
    <div class="card" style="height: 100%">
      <div class="card-image">
        <figure class="image is-16by9">
          <a href="./examples/custom-grid-columns.md" title="自定义列" tabindex="-1">
            <img class="is-bordered-r-small" src="./examples/assets/custom-grid-columns/card.png" alt="自定义列">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="./examples/custom-grid-columns.md" title="自定义列">自定义列</a></p>
          <p class="is-size-6">向内容片段控制台添加自定义列。</p>
          <a href="./examples/custom-grid-columns.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
            <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">查看示例</span>
          </a>
        </div>
      </div>
    </div>
  </div>    
  <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Export to XML">
    <div class="card" style="height: 100%">
      <div class="card-image">
        <figure class="image is-16by9">
          <a href="./examples/editor-export-to-xml.md" title="导出到 XML" tabindex="-1">
            <img class="is-bordered-r-small" src="./examples/assets/export-to-xml/card.png" alt="导出到 XML">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="./examples/editor-export-to-xml.md" title="导出到 XML">导出到 XML</a></p>
          <p class="is-size-6">从内容片段编辑器中将内容片段导出为 XML 格式。</p>
          <a href="./examples/editor-export-to-xml.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
            <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">查看示例</span>
          </a>
        </div>
      </div>
    </div>
  </div>    
  <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Rich Text Editor toolbar button">
    <div class="card" style="height: 100%">
      <div class="card-image">
        <figure class="image is-16by9">
          <a href="./examples/editor-rte-toolbar.md" title="富文本编辑器工具栏按钮" tabindex="-1">
            <img class="is-bordered-r-small" src="./examples/assets/rte/rte-toolbar-card.png" alt="富文本编辑器工具栏按钮">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="./examples/editor-rte-toolbar.md" title="富文本编辑器工具栏按钮">富文本编辑器工具栏按钮</a></p>
          <p class="is-size-6">将自定义工具栏按钮添加到内容片段编辑器中的 RTE 字段。</p>
          <a href="./examples/editor-rte-toolbar.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
            <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">查看示例</span>
          </a>
        </div>
      </div>
    </div>
  </div>   
  <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Rich Text Editor Widget">
    <div class="card" style="height: 100%">
      <div class="card-image">
        <figure class="image is-16by9">
          <a href="./examples/editor-rte-widget.md" title="富文本编辑器小组件" tabindex="-1">
            <img class="is-bordered-r-small" src="./examples/assets/rte/rte-widget-card.png" alt="富文本编辑器小组件">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="./examples/editor-rte-toolbar.md" title="富文本编辑器小组件">富文本编辑器小组件</a></p>
          <p class="is-size-6">将小组件添加到内容片段编辑器中的富文本编辑器。</p>
          <a href="./examples/editor-rte-widget.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
            <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">查看示例</span>
          </a>
        </div>
      </div>
    </div>
  </div>   
  <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Rich Text Editor Badge">
    <div class="card" style="height: 100%">
      <div class="card-image">
        <figure class="image is-16by9">
          <a href="./examples/editor-rte-badges.md" title="富文本编辑器徽章" tabindex="-1">
            <img class="is-bordered-r-small" src="./examples/assets/rte/rte-badge-card.png" alt="富文本编辑器徽章">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="./examples/editor-rte-badges.md" title="富文本编辑器徽章">富文本编辑器徽章</a></p>
          <p class="is-size-6">将小组件添加到内容片段编辑器中的富文本编辑器。</p>
          <a href="./examples/editor-rte-badges.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
            <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">查看示例</span>
          </a>
        </div>
      </div>
    </div>
  </div>

<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Custom fields">
    <div class="card" style="height: 100%">
      <div class="card-image">
        <figure class="image is-16by9">
          <a href="./examples/editor-custom-field.md" title="自定义字段" tabindex="-1">
            <img class="is-bordered-r-small" src="https://video.tv.adobe.com/v/3427585?format=jpeg" alt="自定义字段">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="./examples/editor-custom-field.md" title="自定义字段">自定义字段</a></p>
          <p class="is-size-6">创建自定义内容片段字段。</p>
          <a href="./examples/editor-custom-field.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
            <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">查看示例</span>
          </a>
        </div>
      </div>
    </div>
  </div> 
</div>
