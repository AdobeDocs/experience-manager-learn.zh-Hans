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
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '1433'
ht-degree: 1%

---

# 开发Asset compute元数据工作程序

自定义Asset compute工作程序可以生成XMP(XML)数据，这些数据会发送回AEM，并作为元数据存储在资产上。

常见用例包括：

+ 与第三方系统集成，例如PIM（产品信息管理系统），其中必须检索其他元数据并将其存储在资产上
+ 与Adobe服务（如Content和Commerce AI）集成，以使用其他机器学习属性来扩充资产元数据
+ 从资产的二进制文件派生资产的元数据，并将其作为资产元数据存储在AEMas a Cloud Service

## 您将执行的操作

>[!VIDEO](https://video.tv.adobe.com/v/327313?quality=12&learn=on)

在本教程中，我们将创建一个Asset compute元数据工作程序，该工作程序派生图像资产中最常用的颜色，并将颜色名称写回AEM中资产的元数据。 虽然工作程序本身是基本的，但本教程会使用它来探索如何使用Asset compute工作程序将元数据写回AEMas a Cloud Service中的资产。

## asset compute元数据工作程序调用的逻辑流

asset compute元数据工作程序的调用与 [二进制演绎版生成工作程序](../develop/worker.md)，其主要区别在于返回类型是XMP(XML)演绎版，其值也会写入资产的元数据。

asset compute工作人员实施Asset computeSDK工作人员API合同(位于 `renditionCallback(...)` 函数，从概念上讲：

+ __输入：__ AEM资产的原始二进制文件和处理配置文件参数
+ __输出：__ XMP(XML)呈现版本作为呈现版本保留到AEM资产和资产的元数据

![asset compute元数据工作程序逻辑流](./assets/metadata/logical-flow.png)

1. AEM创作服务会调用Asset compute元数据工作程序，并提供资产的 __(1a)__ 原始二进制文件和 __(1b)__ 处理配置文件中定义的任何参数。
1. asset computeSDK协调自定义Asset compute元数据工作程序的执行 `renditionCallback(...)` 函数，根据资产的二进制文件导出XMP(XML)呈现版本 __(1a)__ 和任何处理配置文件参数 __(1b)__.
1. asset compute工作人员将XMP(XML)表示形式保存到 `rendition.path`.
1. 写入的XMP(XML)数据 `rendition.path` 通过Asset computeSDK传输到AEM创作服务，并将其公开为 __(4a)__ 文本呈现和 __(4b)__ 保留到资产的元数据节点。

## 配置manifest.yml{#manifest}

所有Asset compute员工都必须在 [manifest.yml](../develop/manifest.md).

打开项目的 `manifest.yml` 并添加配置新工作器的工作器条目，在这种情况下 `metadata-colors`.

_记住 `.yml` 对空格敏感。_

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

`function` 指向在 [下一步](#metadata-worker). 从语义上命名工作程序(例如， `actions/worker/index.js` 也许更好的名字 `actions/rendition-circle/index.js`)，如 [工作人员URL](#deploy) 并确定 [工作人员的测试包文件夹名称](#test).

的 `limits` 和 `require-adobe-auth` 是按工作人员分别配置的。 在这个工人里， `512 MB` 当代码检查（可能）大的二进制图像数据时分配内存。 另一个 `limits` 将删除以使用默认值。

## 开发元数据工作程序{#metadata-worker}

在路径的Asset compute项目中创建新的元数据工作程序JavaScript文件 [为新工作程序定义的manifest.yml](#manifest)，在 `/actions/metadata-colors/index.js`

### 安装npm模块

安装额外的npm模块([@adobe/asset-compute-xmp](https://www.npmjs.com/package/@adobe/asset-compute-xmp?activeTab=versions), [get-image-colors](https://www.npmjs.com/package/get-image-colors)和 [颜色名称](https://www.npmjs.com/package/color-namer))中使用的Asset compute工作程序。

```
$ npm install @adobe/asset-compute-xmp
$ npm install get-image-colors
$ npm install color-namer
```

### 元数据工作程序代码

这个工人看起来很像 [演绎版生成工作程序](../develop/worker.md)，主要区别是将XMP(XML)数据写入 `rendition.path` 以保存回AEM。


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

工作代码完成后，可以使用本地Asset compute开发工具执行该代码。

因为我们的Asset compute项目包含两个员工(前一个 [圆形演绎版](../develop/worker.md) 这个 `metadata-colors` worker), [asset compute开发工具的](../develop/development-tool.md) 配置文件定义列出了两个工作程序的执行配置文件。 第二个用户档案定义指向新 `metadata-colors` 工人。

![XML元数据呈现版本](./assets/metadata/metadata-rendition.png)

1. 从Asset compute项目的根
1. 执行 `aio app run` 启动Asset compute开发工具
1. 在 __选择文件……__ 下拉，选择 [样本图像](../assets/samples/sample-file.jpg) 处理
1. 在第二个配置文件定义配置中，该配置指向 `metadata-colors` 工作人员，更新 `"name": "rendition.xml"` 因为此工作人员生成XMP(XML)演绎版。 （可选）添加 `colorsFamily` 参数（支持的值） `basic`, `hex`, `html`, `ntc`, `pantone`, `roygbiv`)。

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

1. 点按 __运行__ 等待XML呈现版本生成
   + 由于配置文件定义中列出了两个工作程序，因此两个演绎版都将生成。 （可选）指向 [圈子演绎版工作人员](../develop/worker.md) ，以避免从开发工具中执行它。
1. 的 __演绎版__ 部分预览生成的呈现版本。 点按 `rendition.xml` 要下载它，请在VS代码（或您喜爱的XML/文本编辑器）中打开它以进行审阅。

## 测试工作人员{#test}

可以使用 [与二进制Asset compute相同的测试框架](../test-debug/test.md). 唯一的区别是 `rendition.xxx` 文件必须是预期的XMP(XML)呈现版本。

1. 在Asset compute项目中创建以下结构：

   ```
   /test/asset-compute/metadata-colors/success-pantone/
   
       file.jpg
       params.json
       rendition.xml
   ```

2. 使用 [示例文件](../assets/samples/sample-file.jpg) 作为测试案例 `file.jpg`.
3. 将以下JSON添加到 `params.json`.

   ```
   {
       "fmt": "xml",
       "colorsFamily": "pantone"
   }
   ```

   请注意 `"fmt": "xml"` 需要指示测试包生成 `.xml` 基于文本的呈现。

4. 在 `rendition.xml` 文件。 这可通过以下方式获取：
   + 通过开发工具运行测试输入文件并保存（已验证）XML呈现版本。

   ```
   <?xml version="1.0" encoding="UTF-8"?><rdf:RDF xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#" xmlns:wknd="https://wknd.site/assets/1.0/"><rdf:Description><wknd:colors><rdf:Seq><rdf:li>Silver</rdf:li><rdf:li>Black</rdf:li><rdf:li>Outer Space</rdf:li></rdf:Seq></wknd:colors><wknd:colorsFamily>pantone</wknd:colorsFamily></rdf:Description></rdf:RDF>
   ```

5. 执行 `aio app test` 从Asset compute项目的根目录执行所有测试包。

### 将员工部署到Adobe I/O Runtime{#deploy}

要从AEM Assets调用此新元数据工作程序，必须使用命令将其部署到Adobe I/O Runtime:

```
$ aio app deploy
```

![aio应用程序部署](./assets/metadata/aio-app-deploy.png)

请注意，这将部署项目中的所有员工。 查看 [未删节的部署说明](../deploy/runtime.md) 以了解如何部署到暂存和生产工作区。

### 与AEM处理配置文件集成{#processing-profile}

通过创建新的或修改调用此已部署工作程序的现有自定义处理配置文件服务，从AEM中调用工作程序。

![处理配置文件](./assets/metadata/processing-profile.png)

1. 登录AEMas a Cloud Service创作服务作为 __AEM管理员__
1. 导航到 __工具>资产>处理配置文件__
1. __创建__ 新的，或 __编辑__ 和现有的处理配置文件
1. 点按 __自定义__ 选项卡，然后点按 __新增__
1. 定义新服务
   + __创建元数据演绎版__:切换到活动
   + __端点：__ `https://...adobeioruntime.net/api/v1/web/wkndAemAssetCompute-0.0.1/metadata-colors`
      + 这是在 [部署](#deploy) 或使用命令 `aio app get-url`. 根据AEMas a Cloud Service环境，确保URL指向正确的工作区。
   + __服务参数__
      + 点按 __添加参数__
         + 键: `colorFamily`
         + 值: `pantone`
            + 支持的值： `basic`, `hex`, `html`, `ntc`, `pantone`, `roygbiv`
   + __Mime 类型__
      + __包括：__ `image/jpeg`, `image/png`, `image/gif`, `image/svg`
         + 这些是第三方npm模块支持的唯一用于导出颜色的MIME类型。
      + __不包括：__ `Leave blank`
1. 点按 __保存__ 在右上方
1. 将处理配置文件应用到AEM Assets文件夹（如果尚未应用）

### 更新元数据架构{#metadata-schema}

要查看颜色元数据，请将图像元数据架构上的两个新字段映射到工作人员填充的新元数据数据属性。

![元数据架构](./assets/metadata/metadata-schema.png)

1. 在AEM创作服务中，导航到 __工具> Assets >元数据架构__
1. 导航到 __默认__ 并选择和编辑 __图像__ 和添加只读表单字段以显示生成的颜色元数据
1. 添加 __单行文本__
   + __字段标签__: `Colors Family`
   + __映射到属性__: `./jcr:content/metadata/wknd:colorsFamily`
   + __规则>字段>禁用编辑__:已选中
1. 添加 __多值文本__
   + __字段标签__: `Colors`
   + __映射到属性__: `./jcr:content/metadata/wknd:colors`
1. 点按 __保存__ 在右上方

## 处理资产

![资源详细信息](./assets/metadata/asset-details.png)

1. 在AEM创作服务中，导航到 __资产>文件__
1. 导航到文件夹或子文件夹后，处理配置文件即会应用到
1. 将新图像(JPEG、PNG、GIF或SVG)上传到文件夹，或使用更新的 [处理配置文件](#processing-profile)
1. 处理完成后，选择资产，然后点按 __属性__ 以显示其元数据
1. 查看 `Colors Family` 和 `Colors` [元数据字段](#metadata-schema) 用于从自定义Asset compute元数据工作程序中回写的元数据。

在 `[dam:Asset]/jcr:content/metadata` 资源中，此元数据将通过搜索使用这些术语编入索引，以增强资产发现功能，这样，甚至可以将这些元数据写回资产的二进制文件 __DAM元数据写回__ 工作流将对其进行调用。

### AEM Assets中的元数据呈现

![AEM Assets元数据演绎版文件](./assets/metadata/cqdam-metadata-rendition.png)

由Asset compute元数据工作程序生成的实际XMP文件也会作为离散呈现版本存储在资产上。 通常不使用此文件，而是使用对资产元数据节点应用的值，但工作程序的原始XML输出可在AEM中使用。

## Github上的元数据颜色工作代码

最后 `metadata-colors/index.js` 可在Github上获取，网址为：

+ [aem-guides-wknd-asset-compute/actions/metadata-colors/index.js](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/actions/metadata-colors/index.js)

最后 `test/asset-compute/metadata-colors` 测试包可在Github上获取，网址为：

+ [aem-guides-wknd-asset-compute/test/asset-compute/metadata-colors](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/test/asset-compute/metadata-colors)
