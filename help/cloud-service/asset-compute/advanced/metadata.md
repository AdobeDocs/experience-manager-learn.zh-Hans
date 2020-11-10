---
title: 开发Asset compute元数据工作者
description: 了解如何创建Asset compute元数据工作器，该工作器派生图像资产中最常用的颜色，并将颜色名称写回AEM中资产的元数据。
feature: asset-compute
topics: metadata, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6448
thumbnail: 327313.jpg
translation-type: tm+mt
source-git-commit: c2a8e6c3ae6dcaa45816b1d3efe569126c6c1e60
workflow-type: tm+mt
source-wordcount: '1434'
ht-degree: 1%

---


# 开发Asset compute元数据工作者

自定义Asset compute工作者可以生成XMP(XML)数据，这些数据将发回AEM并作为元数据存储在资产上。

常见用例包括：

+ 与第三方系统集成，如PIM（产品信息管理系统），在该系统中必须检索其他元数据并将其存储在资产上
+ 与Adobe服务（如内容和商务AI）集成，通过附加的机器学习属性来增强资产元数据
+ 从资产的二进制文件中派生资产的元数据，并将其作为资产元数据存储在AEM中作为Cloud Service

## 您将做什么

>[!VIDEO](https://video.tv.adobe.com/v/327313?quality=12&learn=on)

在本教程中，我们将创建一个Asset compute元数据工作器，它派生出图像资产中最常用的颜色，并将颜色名称写回AEM中资产的元数据。 虽然工作者本身是基本的，但本教程使用它来探索如何使用Asset compute工作者将元数据作为Cloud Service写回到AEM中的资产。

## asset compute元数据工作器调用的逻辑流

asset compute元数据Worker的调用与生成Worker的 [二进制再现的调用几乎相同](../develop/worker.md)，其主要区别是返回类型是XMP(XML)再现，其值也写入资产的元数据。

asset compute工作者实施Asset computeSDK工作者API合同，在功 `renditionCallback(...)` 能中，从概念上讲是：

+ __输入：__ AEM资产的原始二进制和处理用户档案参数
+ __输出：__ XMP(XML)再现作为再现保留到AEM资产以及资产的元数据

![asset compute元数据工作者逻辑流](./assets/metadata/logical-flow.png)

1. AEM作者服务调用Asset compute元数据工作 __器，提供资产的(1a__ )原始二进制文件 __和(1b)处理用户档案__ 中定义的任何参数。
1. asset computeSDK安排自定义用户档案元数据工作器功能的执 `renditionCallback(...)` 行，根据资产的二进制(1a)和任何处理Asset compute参数( __1b)导出XMP__ (XML)再现 __。__
1. asset compute工作者将XMP(XML)表示形式保存到 `rendition.path`。
1. 写入的XMP(XML)数据 `rendition.path` 通过Asset computeSDK传输到AEM作者服务，并以(4a)文本再现形式 __显示它，(4b)______ 保留到资产的元数据节点。

## 配置manifest.yml{#manifest}

所有Asset compute工作者都必须在manifest. [yml中注册](../develop/manifest.md)。

打开项目并添 `manifest.yml` 加配置新工作者的工作者条目，在这种情况下 `metadata-colors`。

_请记 `.yml` 住，空格很敏感。_

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

`function` 指向在下一步中创建的工 [作器实施](#metadata-worker)。 从语义上命名工作器(例 `actions/worker/index.js` 如，可能更好地命 `actions/rendition-circle/index.js`名该工作器)，这些命名在工 [作器的URL中显示](#deploy) ，还确定 [该工作器的测试套件文件夹名称](#test)。

The `limits` and `require-adobe-auth` Designed by worker. 在此工作器中， `512 MB` 当代码检查（可能）大型二进制图像数据时，将分配内存。 另一个 `limits` 将被删除以使用默认值。

## 开发元数据工作器{#metadata-worker}

在Asset compute项目中为新工作者在定义的清单。yml路径 [下为新工作者创建新的元数据工作者](#manifest)JavaScript文件，网址为 `/actions/metadata-colors/index.js`

### 安装npm模块

安装将在此[Asset compute工作器中使用的额外npm](https://www.npmjs.com/package/@adobe/asset-compute-xmp?activeTab=versions)模块(@adobe [/asset-compute-xmp](https://www.npmjs.com/package/get-image-colors)、 [get-image-colors](https://www.npmjs.com/package/color-namer)和color-namer)。

```
$ npm install @adobe/asset-compute-xmp
$ npm install get-image-colors
$ npm install color-namer
```

### 元数据工作代码

此工作器与再现生 [成工作器非常相似](../develop/worker.md)，其主要区别在于它将XMP(XML)数据写 `rendition.path` 入到AEM中以将其保存回。


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
      // These namespaces will be automatically registered in AEM if they do not yet exist
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

## 在本地运行元数据工作器{#development-tool}

工作代码完成后，可以使用本地Asset compute开发工具执行它。

由于我们的Asset compute项目包含两个工作 [者(前一个圆](../develop/worker.md)`metadata-colors` 形再现和此工作者 [)，因此](../develop/development-tool.md) Asset compute开发工具的用户档案定义将为这两个工作者执行用户档案。 第二个用户档案定义指向新工 `metadata-colors` 作者。

![XML元数据再现](./assets/metadata/metadata-rendition.png)

1. 从Asset compute项目的根
1. 执 `aio app run` 行开始Asset compute开发工具
1. 在选 __择文件……下拉__ ，选取要处 [理的示例图](../assets/samples/sample-file.jpg) 像
1. 在第二个用户档案定义配置(指向该 `metadata-colors` 工作器) `"name": "rendition.xml"` 中，当此工作器生成XMP(XML)再现时更新。 或者，也可以添 `colorsFamily` 加参数(支 `basic`持 `hex`的值、 `html`、 `ntc`、 `pantone``roygbiv`、、)。

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
1. 点 __按运行__ ，然后等待XML再现生成
   + 由于两个工作线程都列在用户档案定义中，因此两个再现都将生成。 （可选）可删除指向圆形再现工作 [器的顶部用户档案](../develop/worker.md) 定义，以避免从开发工具中执行它。
1. 演绎 __版部分__ ，将生成的演绎版预览。 点按下 `rendition.xml` 载它，然后在VS代码（或您最喜爱的XML/文本编辑器）中打开它进行审阅。

## 测试工作人员{#test}

可以使用与二进制再现相 [同的Asset compute测试框架来测试元数据工作器](../test-debug/test.md)。 唯一的区别是 `rendition.xxx` 测试用例中的文件必须是所需的XMP(XML)再现。

1. 在Asset compute项目中创建以下结构：

   ```
   /test/asset-compute/metadata-colors/success-pantone/
   
       file.jpg
       params.json
       rendition.xml
   ```

2. 将示 [例文件](../assets/samples/sample-file.jpg) 用作测试用例 `file.jpg`。
3. 将以下JSON添加到 `params.json`。

   ```
   {
       "fmt": "xml",
       "colorsFamily": "pantone"
   }
   ```

   请注 `"fmt": "xml"` 意，要求测试套件生成基于文 `.xml` 本的再现。

4. 在文件中提供所需的 `rendition.xml` XML。 可通过以下方式获取此信息：
   + 通过开发工具运行测试输入文件并保存（已验证）XML再现。

   ```
   <?xml version="1.0" encoding="UTF-8"?><rdf:RDF xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#" xmlns:wknd="https://wknd.site/assets/1.0/"><rdf:Description><wknd:colors><rdf:Seq><rdf:li>Silver</rdf:li><rdf:li>Black</rdf:li><rdf:li>Outer Space</rdf:li></rdf:Seq></wknd:colors><wknd:colorsFamily>pantone</wknd:colorsFamily></rdf:Description></rdf:RDF>
   ```

5. 从Asset compute `aio app test` 项目的根执行以执行所有测试套件。

### 将员工部署到Adobe I/O Runtime{#deploy}

要从AEM Assets调用此新的元数据工作器，必须使用以下命令将其部署到Adobe I/O Runtime:

```
$ aio app deploy
```

![aio应用程序部署](./assets/metadata/aio-app-deploy.png)

请注意，这将部署项目中的所有工作者。 查看有 [关如何部署到](../deploy/runtime.md) Stage和Production工作区的简略部署说明。

### 与AEM处理用户档案集成{#processing-profile}

通过创建新的或修改调用此已部署工作器的现有自定义处理用户档案服务，从AEM调用该工作器。

![处理用户档案](./assets/metadata/processing-profile.png)

1. 以AEM管理员身份登录AEM作为Cloud Service作者服 __务__
1. 导航到工 __具>资产>处理用户档案__
1. __新建__ 、编辑和 __现有__ 的处理用户档案
1. 点按自定 __义选__ 项卡，然后点 __按添加新__
1. 定义新服务
   + __创建元数据再现__:切换为活动
   + __端点：__ `https://...adobeioruntime.net/api/v1/web/wkndAemAssetCompute-0.0.1/metadata-colors`
      + 这是在部署或使用命令时获 [得](#deploy) 的工作者的URL `aio app get-url`。 根据AEM作为Cloud Service环境，确保URL指向正确的工作区。
   + __服务参数__
      + 点按 __添加参数__
         + 键: `colorFamily`
         + 值: `pantone`
            + 支持的值： `basic`, `hex`, `html`, `ntc`, `pantone``roygbiv`
   + __Mime 类型__
      + __包括：__`image/jpeg`, `image/png`, `image/gif``image/svg`
         + 这些是第三方npm模块支持的唯一用于导出颜色的MIME类型。
      + __不包括：__ `Leave blank`
1. 点按 __右上__ 方的“保存”
1. 将处理用户档案应用到AEM Assets文件夹（如果尚未这样做）

### 更新元数据模式{#metadata-schema}

要查看颜色元数据，请将图像元数据模式上的两个新字段映射到工作人员填充的新元数据属性。

![元数据架构](./assets/metadata/metadata-schema.png)

1. 在AEM作者服务中，导航到工 __具>资产>元数据模式__
1. 导航到 __默认__ ，选择并编 __辑图像__ ，添加只读表单字段以显示生成的颜色元数据
1. Add a __Single Line Text__
   + __字段标签__: `Colors Family`
   + __映射到属性__: `./jcr:content/metadata/wknd:colorsFamily`
   + __“规则”>“字段”>“禁用编辑__”:已检查
1. Add a __Multi Value Text__
   + __字段标签__: `Colors`
   + __映射到属性__: `./jcr:content/metadata/wknd:colors`
1. 点按 __右上__ 方的“保存”

## 处理资产

![资源详细信息](./assets/metadata/asset-details.png)

1. 在AEM作者服务中，导航到“资产”>“ __文件”。__
1. 导航到该文件夹或子文件夹，处理用户档案将应用到
1. 将新图像（JPEG、PNG、GIF或SVG）上传到文件夹，或使用更新的“处理”用户档案重新处理现 [有图像](#processing-profile)
1. 处理完成后，选择资产，然后点按顶 __部操作__ 栏中的属性以显示其元数据
1. 查看从自 `Colors Family` 定 `Colors` 义Asset compute [元数据工作](#metadata-schema) 器中回写的元数据的元数据和元数据字段。

使用写入资产元数据的颜色元数据(在资源上 `[dam:Asset]/jcr:content/metadata` )，可以通过搜索使用这些术语索引此元数据，从而提高资产发现能力，并且如果对资产调用DAM元数据写回工作流，甚至可以将这些元数据写回 __资产的二进制__ 。

### AEM Assets的元数据再现

![AEM Assets元数据再现文件](./assets/metadata/cqdam-metadata-rendition.png)

由Asset compute元数据工作器生成的实际XMP文件也会作为离散再现存储在资产上。 通常不使用此文件，而是使用对资产元数据节点应用的值，但AEM中提供了来自工作人员的原始XML输出。

## Github上的元数据颜色工作代码

最后一 `metadata-colors/index.js` 节在Github上提供，网址为：

+ [aem-guides-wknd-asset-compute/actions/metadata-colors/index.js](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/actions/metadata-colors/index.js)

Github上提 `test/asset-compute/metadata-colors` 供的最终测试套件位于：

+ [aem-guides-wknd-asset-compute/test/asset-compute/metadata-colors](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/test/asset-compute/metadata-colors)
