---
title: 开发Asset Compute元数据工作程序
description: 了解如何创建一个Asset Compute元数据工作器，该工作器派生图像资源中最常用的颜色，并将这些颜色的名称写回到AEM中的资源元数据。
feature: Asset Compute Microservices
version: Experience Manager as a Cloud Service
doc-type: Tutorial
jira: KT-6448
thumbnail: 327313.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 6ece6e82-efe9-41eb-adf8-78d9deed131e
duration: 432
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1405'
ht-degree: 0%

---

# 开发Asset Compute元数据工作程序

自定义Asset Compute工作进程可以生成发送回AEM并作为元数据存储在资源上的XMP (XML)数据。

常见的用例包括：

+ 与第三方系统(如PIM （产品信息管理系统）)的集成，在这些第三方系统中，必须检索其他元数据并将其存储在资产上
+ 与Adobe服务(如内容和Commerce AI)集成，使用其他机器学习属性来增强资源元数据
+ 从资产的二进制文件获取有关资产的元数据，并将其存储为AEM as a Cloud Service中的资产元数据

## 您将要做什么

>[!VIDEO](https://video.tv.adobe.com/v/327313?quality=12&learn=on)

在本教程中，我们将创建一个Asset Compute元数据工作器，该工作器派生图像资源中最常用的颜色，并将这些颜色的名称写回到AEM中的资源元数据。 虽然工作器本身是基本的，但本教程将使用该工作器来探索如何使用Asset Compute工作器将元数据写回AEM as a Cloud Service中的资源。

## Asset Compute元数据工作进程调用的逻辑流

Asset Compute元数据工作程序的调用与[生成工作程序的二进制演绎版](../develop/worker.md)的调用几乎相同，主要区别在于返回类型是XMP (XML)演绎版，其值也会写入资产的元数据。

Asset Compute工作进程在`renditionCallback(...)`函数中实施Asset Compute SDK工作进程API合同，从概念上讲：

+ __输入：__ AEM资产的原始二进制文件和处理配置文件参数
+ __输出：__ XMP (XML)呈现版本作为呈现版本保留在AEM资源中，并保留在资源的元数据中

![Asset Compute元数据工作进程逻辑流](./assets/metadata/logical-flow.png)

1. AEM Author服务调用Asset Compute元数据工作程序，提供资产的&#x200B;__(1a)__&#x200B;原始二进制文件以及&#x200B;__(1b)__&#x200B;处理配置文件中定义的任何参数。
1. Asset Compute SDK根据资产的二进制文件&#x200B;__(1a)__&#x200B;和任何处理配置文件参数&#x200B;__(1b)__，协调自定义Asset Compute元数据工作进程的`renditionCallback(...)`函数的执行，从而生成XMP (XML)演绎版。
1. Asset Compute Worker将XMP (XML)表示形式保存到`rendition.path`。
1. 写入到`rendition.path`的XMP (XML)数据将通过Asset Compute SDK传输到AEM Author Service，并将它公开为&#x200B;__(4a)__&#x200B;文本演绎版，并且&#x200B;__(4b)__&#x200B;保留到资产的元数据节点。

## 配置manifest.yml{#manifest}

必须在[manifest.yml](../develop/manifest.md)中注册所有Asset Compute工作程序。

打开项目的`manifest.yml`，并添加用于配置新辅助进程的辅助进程条目，在本例中为`metadata-colors`。

_记住`.yml`区分空格。_

```
packages:
  __APP_PACKAGE__:
    license: Apache-2.0
    actions: 
      worker:
        function: actions/worker/index.js 
        web: 'yes' 
        runtime: 'nodejs:12'
        limits:
          timeout: 60000 # in ms
          memorySize: 512 # in MB
          concurrency: 10 
        annotations:
          require-adobe-auth: true
      metadata-colors:
        function: actions/metadata-colors/index.js 
        web: 'yes' 
        runtime: 'nodejs:12'
        limits:
          memorySize: 512 # in MB   
```

`function`指向在[下一步](#metadata-worker)中创建的工作程序实现。 从语义上命名工作程序（例如，`actions/worker/index.js`可能更适合命名为`actions/rendition-circle/index.js`），因为这些名称显示在[工作程序的URL](#deploy)中，并且还确定[工作程序的测试套件文件夹名称](#test)。

`limits`和`require-adobe-auth`是按辅助进程分开配置的。 在此工作进程中，当代码检查（可能）大型二进制图像数据时，将分配`512 MB`内存。 其他`limits`将被删除以使用默认值。

## 开发元数据工作程序{#metadata-worker}

在Asset Compute项目中创建新的元数据工作程序JavaScript文件，路径为[为新的工作程序定义了manifest.yml，路径为`/actions/metadata-colors/index.js`](#manifest)

### 安装npm模块

安装此Asset Compute工作进程中使用的额外npm模块([@adobe/asset-compute-xmp](https://www.npmjs.com/package/@adobe/asset-compute-xmp?activeTab=versions)、[get-image-colors](https://www.npmjs.com/package/get-image-colors)和[color-namer](https://www.npmjs.com/package/color-namer))。

```
$ npm install @adobe/asset-compute-xmp
$ npm install get-image-colors
$ npm install color-namer
```

### 元数据工作进程代码

此辅助进程与[生成演绎版的辅助进程](../develop/worker.md)非常相似，主要区别在于它将XMP (XML)数据写入`rendition.path`以保存回AEM。


```javascript
"use strict";

const { worker, SourceCorruptError } = require("@adobe/asset-compute-sdk");
const fs = require("fs").promises;

// Require the @adobe/asset-compute-xmp module to create XMP 
const { serializeXmp } = require("@adobe/asset-compute-xmp");

// Require supporting npm modules to derive image colors from image data
const getColors = require("get-image-colors");
// Require supporting npm modules to convert image colors to color names
const namer = require("color-namer");

exports.main = worker(async (source, rendition, params) => {
  // Perform any necessary source (input) checks
  const stats = await fs.stat(source.path);
  if (stats.size === 0) {
    // Throw appropriate errors whenever an erring condition is met
    throw new SourceCorruptError("source file is empty");
  }
  const MAX_COLORS = 10;
  const DEFAULT_COLORS_FAMILY = 'basic';

  // Read the color family parameter to use to derive the color names
  let colorsFamily = rendition.instructions.colorsFamily || DEFAULT_COLORS_FAMILY;

  if (['basic', 'hex', 'html', 'ntc', 'pantone', 'roygbiv'].indexOf(colorsFamily) === -1) { 
      colorsFamily = DEFAULT_COLORS_FAMILY;
  }
  
  // Use the `get-image-colors` module to derive the most common colors from the image
  let colors = await getColors(source.path, { options: MAX_COLORS });

  // Convert the color Chroma objects to their closest names
  let colorNames = colors.map((color) => getColorName(colorsFamily, color));

  // Serialize the data to XMP metadata
  // These properties are written to the [dam:Asset]/jcr:content/metadata resource
  // This stores
  // - The list of color names is stored in a JCR property named `wknd:colors`
  // - The colors family used to derive the color names is stored in a JCR property named `wknd:colorsFamily`
  const xmp = serializeXmp({
      // Use a Set to de-duplicate color names
      "wknd:colors": [...new Set(colorNames)],
      "wknd:colorsFamily": colorsFamily
    }, {
      // Define any property namespaces used in the above property/value definition
      // These namespaces are automatically registered in AEM if they do not yet exist
      namespaces: {
        wknd: "https://wknd.site/assets/1.0/",
      },
    }
  );

  // Save the XMP metadata to be written back to the asset's metadata node
  await fs.writeFile(rendition.path, xmp, "utf-8");
});

/**
 * Helper function that derives the closest color name for the color, based on the colors family
 * 
 * @param {*} colorsFamily the colors name family to use
 * @param {*} color the color to convert to a name
 */
function getColorName(colorsFamily, color) {
    if ('hex' === colorsFamily) {  return color; }

    let names = namer(color.rgb())[colorsFamily];

    if (names.length >= 1) { return names[0].name; }
}
```

## 在本地运行元数据工作程序{#development-tool}

在工作程序代码完成后，可以使用本地Asset Compute开发工具执行该代码。

由于我们的Asset Compute项目包含两个工作程序（之前的[circle演绎版](../develop/worker.md)和此`metadata-colors`工作程序），因此[Asset Compute开发工具的](../develop/development-tool.md)配置文件定义将列出这两个工作程序的执行配置文件。 第二个配置文件定义指向新的`metadata-colors`工作程序。

![XML元数据演绎版](./assets/metadata/metadata-rendition.png)

1. 从Asset Compute项目的根目录
1. 执行`aio app run`以启动Asset Compute开发工具
1. 在&#x200B;__选择文件……__&#x200B;下拉列表中，选取要处理的[示例图像](../assets/samples/sample-file.jpg)
1. 在指向`metadata-colors`辅助进程的第二个配置文件定义配置中，在此辅助进程生成XMP (XML)呈现时更新`"name": "rendition.xml"`。 （可选）添加`colorsFamily`参数（支持的值`basic`、`hex`、`html`、`ntc`、`pantone`、`roygbiv`）。

   ```json
   {
       "renditions": [
           {
               "worker": "...",
               "name": "rendition.xml",
               "colorsFamily": "pantone"
           }
       ]
   }
   ```

1. 点按&#x200B;__运行__&#x200B;并等待生成XML呈现版本
   + 由于这两个工作人员都在用户档案定义中列出，因此将生成这两个演绎版。 或者，可以删除指向[circle演绎版工作进程](../develop/worker.md)的顶部配置文件定义，以避免从开发工具中执行它。
1. __呈现版本__&#x200B;部分预览生成的呈现版本。 点按`rendition.xml`以下载它，然后在VS代码（或您喜爱的XML/文本编辑器）中打开它以进行查看。

## 测试工作程序{#test}

可以使用与二进制格式副本[&#128279;](../test-debug/test.md)相同的Asset Compute测试框架来测试元数据工作程序。 唯一的区别是测试用例中的`rendition.xxx`文件必须是预期的XMP (XML)演绎版。

1. 在Asset Compute项目中创建以下结构：

   ```
   /test/asset-compute/metadata-colors/success-pantone/
   
       file.jpg
       params.json
       rendition.xml
   ```

2. 使用[示例文件](../assets/samples/sample-file.jpg)作为测试用例的`file.jpg`。
3. 将以下JSON添加到`params.json`。

   ```
   {
       "fmt": "xml",
       "colorsFamily": "pantone"
   }
   ```

   请注意，需要`"fmt": "xml"`才能指示测试包生成基于`.xml`文本的演绎版。

4. 在`rendition.xml`文件中提供所需的XML。 这可以通过以下方式获得：
   + 通过开发工具运行测试输入文件并保存（已验证的）XML呈现版本。

   ```
   <?xml version="1.0" encoding="UTF-8"?><rdf:RDF xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#" xmlns:wknd="https://wknd.site/assets/1.0/"><rdf:Description><wknd:colors><rdf:Seq><rdf:li>Silver</rdf:li><rdf:li>Black</rdf:li><rdf:li>Outer Space</rdf:li></rdf:Seq></wknd:colors><wknd:colorsFamily>pantone</wknd:colorsFamily></rdf:Description></rdf:RDF>
   ```

5. 从Asset Compute项目的根执行`aio app test`以执行所有测试包。

### 将工作人员部署到Adobe I/O Runtime{#deploy}

要从AEM Assets调用此新元数据工作程序，必须使用命令将其部署到Adobe I/O Runtime：

```
$ aio app deploy
```

![aio应用部署](./assets/metadata/aio-app-deploy.png)

请注意，这将部署项目中的所有工作人员。 查看[未删节的部署说明](../deploy/runtime.md)，了解如何部署到暂存和生产工作区。

### 与AEM处理用户档案集成{#processing-profile}

通过创建新的自定义处理配置文件服务或修改现有的自定义处理配置文件服务，从AEM调用工作程序，此服务将调用已部署的工作程序。

![正在处理配置文件](./assets/metadata/processing-profile.png)

1. 以&#x200B;__AEM as a Cloud Service管理员__&#x200B;身份登录AEM创作服务
1. 导航到&#x200B;__工具> Assets >处理配置文件__
1. __创建__&#x200B;新的处理配置文件，或&#x200B;__编辑__&#x200B;现有的处理配置文件
1. 点按&#x200B;__自定义__&#x200B;选项卡，然后点按&#x200B;__新增__
1. 定义新服务
   + __创建元数据演绎版__：切换到活动状态
   + __终结点：__ `https://...adobeioruntime.net/api/v1/web/wkndAemAssetCompute-0.0.1/metadata-colors`
      + 这是在[部署](#deploy)期间或使用命令`aio app get-url`获得的辅助进程的URL。 根据AEM as a Cloud Service环境，确保URL指向正确的工作区。
   + __服务参数__
      + 点按&#x200B;__添加参数__
         + 键： `colorFamily`
         + 值： `pantone`
            + 支持的值： `basic`、`hex`、`html`、`ntc`、`pantone`、`roygbiv`
   + __Mime类型__
      + __包括：__ `image/jpeg`，`image/png`，`image/gif`，`image/svg`
         + 这些是第三方npm模块支持的唯一用于派生颜色的MIME类型。
      + __排除：__ `Leave blank`
1. 点按右上方的&#x200B;__保存__
1. 将处理配置文件应用到AEM Assets文件夹（如果尚未这样做）

### 更新元数据架构{#metadata-schema}

要查看颜色元数据，请将图像元数据架构上的两个新字段映射到工作进程填充的新元数据数据属性。

![元数据架构](./assets/metadata/metadata-schema.png)

1. 在AEM创作服务中，导航到&#x200B;__工具> Assets >元数据架构__
1. 导航到&#x200B;__默认__&#x200B;并选择并编辑&#x200B;__图像__&#x200B;并添加只读表单字段以公开生成的颜色元数据
1. 添加&#x200B;__单行文本__
   + __字段标签__： `Colors Family`
   + __映射到属性__： `./jcr:content/metadata/wknd:colorsFamily`
   + __规则>字段>禁用编辑__：已选中
1. 添加&#x200B;__多值文本__
   + __字段标签__： `Colors`
   + __映射到属性__： `./jcr:content/metadata/wknd:colors`
1. 点按右上方的&#x200B;__保存__

## 正在处理资产

![资源详细信息](./assets/metadata/asset-details.png)

1. 在AEM创作服务中，导航到&#x200B;__Assets >文件__
1. 导航到文件夹或子文件夹，处理配置文件将应用于
1. 将新图像(JPEG、PNG、GIF或SVG)上传到文件夹，或使用更新的[处理配置文件](#processing-profile)重新处理现有图像
1. 处理完成后，选择资产，然后点按顶部操作栏中的&#x200B;__属性__&#x200B;以显示其元数据
1. 查看从自定义Asset Compute元数据辅助进程回写的元数据的`Colors Family`和`Colors` [元数据字段](#metadata-schema)。

在将颜色元数据写入资产的元数据后，在`[dam:Asset]/jcr:content/metadata`资源上，此元数据被编入索引，提高了通过搜索使用这些术语发现资产的能力，甚至可以将其写回资产的二进制文件（如果在其上调用&#x200B;__DAM元数据写回__&#x200B;工作流）。

### AEM Assets中的元数据演绎版

![AEM Assets元数据演绎版文件](./assets/metadata/cqdam-metadata-rendition.png)

Asset Compute元数据工作进程生成的实际XMP文件还作为单独的演绎版存储在资源上。 通常不使用此文件，而是使用对资源的元数据节点应用的值，但工作进程的原始XML输出在AEM中可用。

## Github上的metadata-colors工作代码

Github上的最终`metadata-colors/index.js`位于：

+ [aem-guides-wknd-asset-compute/actions/metadata-colors/index.js](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/actions/metadata-colors/index.js)

Github上的最终`test/asset-compute/metadata-colors`测试套件位于：

+ [aem-guides-wknd-asset-compute/test/asset-compute/metadata-colors](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/test/asset-compute/metadata-colors)
