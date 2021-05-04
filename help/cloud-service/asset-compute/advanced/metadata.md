---
title: 开发Asset compute元数据工作器
description: 了解如何创建一个Asset compute元数据worker，它派生了图像资产中最常用的颜色，并将颜色名称写回AEM中资产的元数据。
feature: asset compute Microservices
topics: metadata, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6448
thumbnail: 327313.jpg
topic: 集成、开发
role: Developer
level: Intermediate, Experienced
translation-type: tm+mt
source-git-commit: 2efb7050b0b0c783c5f34c1f2e375cf21fa7a6cd
workflow-type: tm+mt
source-wordcount: '1439'
ht-degree: 1%

---


# 开发Asset compute元数据工作器

自定义Asset compute工作者可以生成XMP(XML)数据，这些数据将发送回AEM并作为元数据存储在资产上。

常见用例包括：

+ 与第三方系统集成，如PIM（产品信息管理系统），其中必须检索其他元数据并存储在资产上
+ 与Adobe服务（如内容和商务AI）集成，通过附加的机器学习属性来增强资产元数据
+ 从资产的二进制文件中派生资产的元数据，并将其作为资产元数据存储在AEM中作为Cloud Service

## 您将做什么

>[!VIDEO](https://video.tv.adobe.com/v/327313?quality=12&learn=on)

在本教程中，我们将创建一个Asset compute元数据worker，它派生出图像资产中最常用的颜色，并将颜色名称写回AEM中资产的元数据。 虽然worker本身是基本的，但本教程使用它来探索如何使用Asset compute worker将元数据作为Cloud Service写回到AEM中的资产。

## asset compute元数据工作器调用的逻辑流

对Asset compute元数据Worker的调用与生成Worker](../develop/worker.md)的[二进制格式副本的调用几乎相同，主要区别是返回类型是XMP(XML)格式副本，其值也会写入资产的元数据。

asset compute工作者在`renditionCallback(...)`函数中实施Asset compute SDK工作人员API合同，从概念上讲，该合同是：

+ __输入：__ AEM资产的原始二进制和处理用户档案参数
+ __输出：__ XMP(XML)再现会作为再现保留到AEM资产以及资产的元数据

![asset compute元数据工作流](./assets/metadata/logical-flow.png)

1. AEM作者服务调用Asset compute元数据worker，提供资产的&#x200B;__(1a)__&#x200B;原始二进制文件和&#x200B;__(1b)__&#x200B;在处理用户档案中定义的任何参数。
1. asset compute SDK安排自定义Asset compute元数据worker的`renditionCallback(...)`函数的执行，根据资产的二进制&#x200B;__(1a)__&#x200B;和任何处理用户档案参数&#x200B;__(1b)__&#x200B;导出XMP(XML)再现。
1. asset compute工作者将XMP(XML)表示形式保存到`rendition.path`。
1. 写入`rendition.path`的XMP(XML)数据通过Asset compute SDK传输到AEM作者服务，并以&#x200B;__(4a)__&#x200B;文本格式副本和&#x200B;__(4b)__&#x200B;保留到资产的元数据节点的形式显示。

## 配置manifest.yml{#manifest}

所有Asset computeWorker都必须在[manifest.yml](../develop/manifest.md)中注册。

打开项目的`manifest.yml`并添加配置新worker的worker条目（在本例中为`metadata-colors`）。

_记住 `.yml` 空格是敏感的。_

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

`function` 指向在下一步中创建的 [worker实现](#metadata-worker)。从语义上命名worker（例如，`actions/worker/index.js`可能更好地命名为`actions/rendition-circle/index.js`），如[worker的URL](#deploy)中所示，还确定[worker的测试套件文件夹名称](#test)。

`limits`和`require-adobe-auth`是按工作器分别配置的。 在此worker中，内存的`512 MB`被分配，因为代码检查（可能）大的二进制图像数据。 其他`limits`将被删除以使用默认值。

## 开发元数据worker{#metadata-worker}

在Asset compute项目中为新worker](#manifest)定义的清单.yml路径[的`/actions/metadata-colors/index.js`创建一个新的元数据worker JavaScript文件

### 安装npm模块

安装将在此Asset computeWorker中使用的额外npm模块([@adobe/asset-compute-xmp](https://www.npmjs.com/package/@adobe/asset-compute-xmp?activeTab=versions)、[get-image-colors](https://www.npmjs.com/package/get-image-colors)和[color-namer](https://www.npmjs.com/package/color-namer))。

```
$ npm install @adobe/asset-compute-xmp
$ npm install get-image-colors
$ npm install color-namer
```

### 元数据工作代码

此worker与[再现生成worker](../develop/worker.md)非常相似，主要区别是它将XMP(XML)数据写入`rendition.path`以保存回AEM。


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

## 在本地运行元数据worker{#development-tool}

工作代码完成后，可以使用本地Asset compute开发工具执行它。

由于我们的Asset compute项目包含两个工作程序（前一个[圆形再现](../develop/worker.md)和此`metadata-colors`工作程序），因此[Asset compute开发工具的](../develop/development-tool.md)用户档案定义将为这两个工作程序执行用户档案。 第二个用户档案定义指向新的`metadata-colors`worker。

![XML元数据再现](./assets/metadata/metadata-rendition.png)

1. 从Asset compute项目的根
1. 执行`aio app run`以开始Asset compute开发工具
1. 在&#x200B;__选择文件……__&#x200B;下拉框，选取[样本图像](../assets/samples/sample-file.jpg)进行处理
1. 在指向`metadata-colors` worker的第二个用户档案定义配置中，当此worker生成XMP(XML)再现时更新`"name": "rendition.xml"`。 或者，添加`colorsFamily`参数（支持的值`basic`、`hex`、`html`、`ntc`、`pantone`、`roygbiv`）。

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

1. 点按&#x200B;__运行__&#x200B;并等待XML再现生成
   + 由于两个工作线程都列在用户档案定义中，因此两个再现都将生成。 或者，可以删除指向[圆形再现worker](../develop/worker.md)的顶部用户档案定义，以避免从开发工具中执行它。
1. __演绎版__&#x200B;部分预览生成的演绎版。 点按`rendition.xml`以下载它，然后在VS代码（或您喜爱的XML/文本编辑器）中打开它进行查看。

## 测试worker{#test}

可以使用与二进制演绎版](../test-debug/test.md)相同的Asset compute测试框架来测试元数据工作器。 [唯一的区别是测试用例中的`rendition.xxx`文件必须是预期的XMP(XML)再现。

1. 在Asset compute项目中创建以下结构：

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

   请注意，要求`"fmt": "xml"`指示测试套件生成`.xml`基于文本的再现。

4. 在`rendition.xml`文件中提供所需的XML。 可通过以下方式获取此信息：
   + 通过开发工具运行测试输入文件并保存（已验证）XML再现。

   ```
   <?xml version="1.0" encoding="UTF-8"?><rdf:RDF xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#" xmlns:wknd="https://wknd.site/assets/1.0/"><rdf:Description><wknd:colors><rdf:Seq><rdf:li>Silver</rdf:li><rdf:li>Black</rdf:li><rdf:li>Outer Space</rdf:li></rdf:Seq></wknd:colors><wknd:colorsFamily>pantone</wknd:colorsFamily></rdf:Description></rdf:RDF>
   ```

5. 从Asset compute项目的根执行`aio app test`以执行所有测试套件。

### 将Worker部署到Adobe I/O Runtime{#deploy}

要从AEM Assets调用此新元数据worker，必须使用以下命令将其部署到Adobe I/O Runtime:

```
$ aio app deploy
```

![ai应用程序部署](./assets/metadata/aio-app-deploy.png)

请注意，这将部署项目中的所有工作人员。 查看[未加删节的部署说明](../deploy/runtime.md)，了解如何部署到舞台和生产工作区。

### 与AEM处理用户档案集成{#processing-profile}

通过创建新的或修改调用此已部署工作程序的现有自定义处理用户档案服务，从AEM调用该工作程序。

![处理用户档案](./assets/metadata/processing-profile.png)

1. 以&#x200B;__AEM Administrator__&#x200B;身份登录AEM作为Cloud Service作者服务
1. 导航到&#x200B;__工具>资产>处理用户档案__
1. __创__ 建新的或编辑的 ____ 现有处理用户档案
1. 点按&#x200B;__自定义__&#x200B;选项卡，然后点按&#x200B;__添加新__
1. 定义新服务
   + __创建元数据演绎版__:切换至活动
   + __端点：__ `https://...adobeioruntime.net/api/v1/web/wkndAemAssetCompute-0.0.1/metadata-colors`
      + 这是在[deploy](#deploy)期间或使用命令`aio app get-url`获取的工作器的URL。 根据AEM作为Cloud Service环境，确保URL点位于正确的工作区。
   + __服务参数__
      + 点按&#x200B;__添加参数__
         + 键: `colorFamily`
         + 值: `pantone`
            + 支持的值：`basic`、`hex`、`html`、`ntc`、`pantone`、`roygbiv`
   + __Mime 类型__
      + __包括__ `image/jpeg`: `image/png`、 `image/gif`、  `image/svg`
         + 这些是用于导出颜色的第三方npm模块支持的唯一MIME类型。
      + __不包括：__ `Leave blank`
1. 点按右上角的&#x200B;__保存__
1. 如果尚未将处理用户档案应用到AEM Assets文件夹，请将其应用到

### 更新元数据模式{#metadata-schema}

要查看颜色元数据，请将图像元数据模式上的两个新字段映射到工作人员填充的新元数据属性。

![元数据架构](./assets/metadata/metadata-schema.png)

1. 在AEM作者服务中，导航到&#x200B;__工具>资产>元数据模式__
1. 导航到&#x200B;__default__，选择并编辑&#x200B;__image__&#x200B;并添加只读表单字段以显示生成的颜色元数据
1. 添加&#x200B;__单行文本__
   + __字段标签__: `Colors Family`
   + __映射到属性__: `./jcr:content/metadata/wknd:colorsFamily`
   + __“规则”>“字段”>“禁用编辑__”：已选中
1. 添加&#x200B;__多值文本__
   + __字段标签__: `Colors`
   + __映射到属性__: `./jcr:content/metadata/wknd:colors`
1. 点按右上角的&#x200B;__保存__

## 处理资产

![资源详细信息](./assets/metadata/asset-details.png)

1. 在AEM作者服务中，导航到&#x200B;__资产>文件__
1. 导览至要应用处理用户档案的文件夹或子文件夹
1. 将新图像（JPEG、PNG、GIF或SVG）上传到文件夹，或使用更新的[处理用户档案](#processing-profile)重新处理现有图像
1. 处理完成后，选择资产，然后点按顶部操作栏中的&#x200B;__属性__&#x200B;以显示其元数据
1. 查看从自定义Asset compute元数据worker中回写的元数据的`Colors Family`和`Colors` [元数据字段](#metadata-schema)。

在`[dam:Asset]/jcr:content/metadata`资源上，将颜色元数据写入资产的元数据中，通过搜索使用这些术语索引此元数据以提高资产发现能力，并且如果对资产调用了&#x200B;__DAM元数据写回__&#x200B;工作流，则甚至可以将这些元数据写入资产的二进制文件。

### AEM Assets中的元数据再现

![AEM Assets元数据再现文件](./assets/metadata/cqdam-metadata-rendition.png)

由Asset compute元数据worker生成的实际XMP文件也会作为离散格式副本存储在资产上。 此文件通常不使用，而是使用应用到资产元数据节点的值，但工作器的原始XML输出在AEM中可用。

## Github上的元数据颜色工作代码

最后`metadata-colors/index.js`在Github上可用，网址为：

+ [aem-guides-wknd-asset-compute/actions/metadata-colors/index.js](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/actions/metadata-colors/index.js)

Github上有最终的`test/asset-compute/metadata-colors`测试套件，网址为：

+ [aem-guides-wknd-asset-compute/test/asset-compute/metadata-colors](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/test/asset-compute/metadata-colors)
