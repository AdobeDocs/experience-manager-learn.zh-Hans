---
title: 开发Asset compute元数据工作程序
description: 了解如何创建Asset compute元数据工作程序，该工作程序派生图像资产中最常用的颜色，并将颜色名称写回AEM中资产的元数据。
feature: Asset Compute Microservices
topics: metadata, development
version: Cloud Service
activity: develop
audience: developer
doc-type: tutorial
kt: 6448
thumbnail: 327313.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 6ece6e82-efe9-41eb-adf8-78d9deed131e
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '1434'
ht-degree: 1%

---

# 开发Asset compute元数据工作程序

自定义Asset compute工作程序可以生成XMP(XML)数据，这些数据会发送回AEM，并作为元数据存储在资产上。

常见用例包括：

+ 与第三方系统集成，例如PIM（产品信息管理系统），其中必须检索其他元数据并将其存储在资产上
+ 与Adobe服务（如Content和Commerce AI）集成，以使用其他机器学习属性来扩充资产元数据
+ 从资产的二进制文件派生资产的元数据，并将其作为资产元数据存储在AEM中作为Cloud Service

## 您将执行的操作

>[!VIDEO](https://video.tv.adobe.com/v/327313?quality=12&learn=on)

在本教程中，我们将创建一个Asset compute元数据工作程序，该工作程序派生图像资产中最常用的颜色，并将颜色名称写回AEM中资产的元数据。 虽然工作程序本身是基本的，但本教程会使用它来探索如何使用Asset compute工作程序将元数据写回AEM中的资产作为Cloud Service。

## asset compute元数据工作程序调用的逻辑流

asset compute元数据工作程序的调用与生成工作程序](../develop/worker.md)的[二进制呈现程序的调用几乎相同，主要区别在于返回类型是XMP(XML)呈现，其值也写入资产的元数据。

asset compute工作人员在`renditionCallback(...)`函数中实施Asset computeSDK工作人员API合同，从概念上讲，该合同是：

+ __输入：__ AEM资产的原始二进制文件和处理配置文件参数
+ __输出：__ XMP(XML)呈现版本作为呈现版本保留到AEM资产和资产的元数据

![asset compute元数据工作程序逻辑流](./assets/metadata/logical-flow.png)

1. AEM创作服务会调用Asset compute元数据工作程序，并提供资产的&#x200B;__(1a)__&#x200B;原始二进制文件和&#x200B;__(1b)__&#x200B;处理配置文件中定义的任何参数。
1. asset computeSDK协调自定义Asset compute元数据工作程序`renditionCallback(...)`函数的执行，该函数根据资产的二进制文件&#x200B;__(1a)__&#x200B;和任何处理配置文件参数&#x200B;__(1b)__&#x200B;导出XMP(XML)呈现。
1. asset compute工作程序将XMP(XML)表示形式保存到`rendition.path`。
1. 写入`rendition.path`的XMP(XML)数据通过Asset computeSDK传输到AEM创作服务，并将其显示为&#x200B;__(4a)__&#x200B;文本呈现，以及保留到资产元数据节点的&#x200B;__(4b)__。

## 配置manifest.yml{#manifest}

必须在[manifest.yml](../develop/manifest.md)中注册所有Asset compute工作程序。

打开项目的`manifest.yml`并添加配置新工作程序的工作程序条目，在此例中为`metadata-colors`。

_请记 `.yml` 住，空格敏感。_

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

`function` 指向在下一步中创建的 [worker实施](#metadata-worker)。从语义上命名工作程序（例如，`actions/worker/index.js`可能更好地命名为`actions/rendition-circle/index.js`），如[worker URL](#deploy)中所示，并且还确定[worker的测试包文件夹名称](#test)。

`limits`和`require-adobe-auth`分别按工作程序进行配置。 在此工作器中，内存的`512 MB`被分配，因为代码会检查（可能）大的二进制图像数据。 其他`limits`将被删除以使用默认值。

## 开发元数据工作程序{#metadata-worker}

在Asset compute项目中为新工作程序](#manifest)定义的清单.yml路径(`/actions/metadata-colors/index.js`)的[下，为新工作程序创建新的元数据工作程序JavaScript文件

### 安装npm模块

安装将在此Asset compute工作程序中使用的额外npm模块([@adobe/asset-compute-xmp](https://www.npmjs.com/package/@adobe/asset-compute-xmp?activeTab=versions)、[get-image-colors](https://www.npmjs.com/package/get-image-colors)和[color-namer](https://www.npmjs.com/package/color-namer))。

```
$ npm install @adobe/asset-compute-xmp
$ npm install get-image-colors
$ npm install color-namer
```

### 元数据工作程序代码

此工作程序与[格式副本生成工作程序](../develop/worker.md)非常相似，主要区别在于它将XMP(XML)数据写入`rendition.path`以保存回AEM。


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

## 在本地运行元数据工作程序{#development-tool}

工作代码完成后，可以使用本地Asset compute开发工具执行该代码。

由于我们的Asset compute项目包含两个工作程序（前一个[圆呈现](../develop/worker.md)和此`metadata-colors`工作程序），因此[Asset compute开发工具的](../develop/development-tool.md)配置文件定义列出了两个工作程序的执行配置文件。 第二个配置文件定义指向新的`metadata-colors`工作器。

![XML元数据呈现版本](./assets/metadata/metadata-rendition.png)

1. 从Asset compute项目的根
1. 执行`aio app run`以启动Asset compute开发工具
1. 在&#x200B;__选择文件……__&#x200B;下拉菜单，选取[要处理的示例图像](../assets/samples/sample-file.jpg)
1. 在指向`metadata-colors`工作程序的第二个配置文件定义配置中，更新`"name": "rendition.xml"` ，因为此工作程序生成XMP(XML)呈现版本。 或者，也可以添加`colorsFamily`参数（支持的值`basic`、`hex`、`html`、`ntc`、`pantone`、`roygbiv`）。

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

1. 点按&#x200B;__运行__ ，然后等待XML呈现版本生成
   + 由于配置文件定义中列出了两个工作程序，因此两个演绎版都将生成。 （可选）可以删除指向[圆呈现工作程序](../develop/worker.md)的顶部配置文件定义，以避免从开发工具中执行该定义。
1. __演绎版__&#x200B;部分预览生成的演绎版。 点按`rendition.xml`以下载它，然后在VS代码（或您最喜爱的XML/文本编辑器）中将其打开以进行审阅。

## 测试工作人员{#test}

可以使用[与二进制演绎版](../test-debug/test.md)相同的Asset compute测试框架来测试元数据工作程序。 唯一的区别在于，测试案例中的`rendition.xxx`文件必须是预期的XMP(XML)呈现版本。

1. 在Asset compute项目中创建以下结构：

   ```
   /test/asset-compute/metadata-colors/success-pantone/
   
       file.jpg
       params.json
       rendition.xml
   ```

2. 使用[样例文件](../assets/samples/sample-file.jpg)作为测试用例的`file.jpg`。
3. 将以下JSON添加到`params.json`中。

   ```
   {
       "fmt": "xml",
       "colorsFamily": "pantone"
   }
   ```

   请注意，要求`"fmt": "xml"`指示测试包生成基于文本的`.xml`呈现版本。

4. 在`rendition.xml`文件中提供预期的XML。 这可通过以下方式获取：
   + 通过开发工具运行测试输入文件并保存（已验证）XML呈现版本。

   ```
   <?xml version="1.0" encoding="UTF-8"?><rdf:RDF xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#" xmlns:wknd="https://wknd.site/assets/1.0/"><rdf:Description><wknd:colors><rdf:Seq><rdf:li>Silver</rdf:li><rdf:li>Black</rdf:li><rdf:li>Outer Space</rdf:li></rdf:Seq></wknd:colors><wknd:colorsFamily>pantone</wknd:colorsFamily></rdf:Description></rdf:RDF>
   ```

5. 从Asset compute项目的根执行`aio app test`以执行所有测试包。

### 将员工部署到Adobe I/O Runtime{#deploy}

要从AEM Assets调用此新元数据工作程序，必须使用命令将其部署到Adobe I/O Runtime:

```
$ aio app deploy
```

![aio应用程序部署](./assets/metadata/aio-app-deploy.png)

请注意，这将部署项目中的所有员工。 请查看[未删节的部署说明](../deploy/runtime.md)，了解如何部署到暂存和生产工作区。

### 与AEM处理配置文件集成{#processing-profile}

通过创建新的或修改调用此已部署工作程序的现有自定义处理配置文件服务，从AEM中调用工作程序。

![处理配置文件](./assets/metadata/processing-profile.png)

1. 以&#x200B;__AEM Administrator__&#x200B;的身份登录AEM as a Cloud Service创作服务
1. 导航到&#x200B;__工具>资产>处理配置文件__
1. ____ 创建新的或编辑的 ____ 现有处理配置文件
1. 点按&#x200B;__Custom__&#x200B;选项卡，然后点按&#x200B;__Add New__
1. 定义新服务
   + __创建元数据演绎版__:切换到活动
   + __端点：__ `https://...adobeioruntime.net/api/v1/web/wkndAemAssetCompute-0.0.1/metadata-colors`
      + 这是在[部署](#deploy)或使用命令`aio app get-url`期间获取的工作器的URL。 根据AEM as a Cloud Service环境，确保URL指向正确的工作区。
   + __服务参数__
      + 点按&#x200B;__添加参数__
         + 键: `colorFamily`
         + 值: `pantone`
            + 支持的值：`basic`、`hex`、`html`、`ntc`、`pantone`、`roygbiv`
   + __Mime 类型__
      + __包括：__ `image/jpeg`、 `image/png`、 `image/gif`、  `image/svg`
         + 这些是第三方npm模块支持的唯一用于导出颜色的MIME类型。
      + __不包括：__ `Leave blank`
1. 点按右上方的&#x200B;__Save__
1. 将处理配置文件应用到AEM Assets文件夹（如果尚未应用）

### 更新元数据架构{#metadata-schema}

要查看颜色元数据，请将图像元数据架构上的两个新字段映射到工作人员填充的新元数据数据属性。

![元数据架构](./assets/metadata/metadata-schema.png)

1. 在AEM创作服务中，导航到&#x200B;__工具> Assets >元数据架构__
1. 导航到&#x200B;__default__，然后选择并编辑&#x200B;__image__，并添加只读表单字段以显示生成的颜色元数据
1. 添加&#x200B;__单行文本__
   + __字段标签__: `Colors Family`
   + __映射到属性__: `./jcr:content/metadata/wknd:colorsFamily`
   + __规则>字段>禁用编辑__:已选中
1. 添加&#x200B;__多值文本__
   + __字段标签__: `Colors`
   + __映射到属性__: `./jcr:content/metadata/wknd:colors`
1. 点按右上方的&#x200B;__Save__

## 处理资产

![资源详细信息](./assets/metadata/asset-details.png)

1. 在AEM创作服务中，导航到&#x200B;__Assets > Files__
1. 导航到文件夹或子文件夹后，处理配置文件即会应用到
1. 将新图像（JPEG、PNG、GIF或SVG）上传到文件夹，或使用更新的[处理配置文件](#processing-profile)重新处理现有图像
1. 处理完成后，选择资产，然后点按顶部操作栏中的&#x200B;__属性__&#x200B;以显示其元数据
1. 查看从自定义Asset compute元数据工作程序写回的元数据的`Colors Family`和`Colors` [元数据字段](#metadata-schema)。

在`[dam:Asset]/jcr:content/metadata`资源上，使用写入资产元数据的颜色元数据，可以使用这些术语通过搜索将此元数据编入索引，以提高资产发现功能，如果随后调用&#x200B;__DAM元数据写回__&#x200B;工作流，则甚至可以将这些元数据写回资产的二进制文件。

### AEM Assets中的元数据呈现

![AEM Assets元数据演绎版文件](./assets/metadata/cqdam-metadata-rendition.png)

由Asset compute元数据工作程序生成的实际XMP文件也会作为离散呈现版本存储在资产上。 通常不使用此文件，而是使用对资产元数据节点应用的值，但工作程序的原始XML输出可在AEM中使用。

## Github上的元数据颜色工作代码

最终`metadata-colors/index.js`可在Github上获取，网址为：

+ [aem-guides-wknd-asset-compute/actions/metadata-colors/index.js](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/actions/metadata-colors/index.js)

最终的`test/asset-compute/metadata-colors`测试包可在Github上获取，网址为：

+ [aem-guides-wknd-asset-compute/test/asset-compute/metadata-colors](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/test/asset-compute/metadata-colors)
