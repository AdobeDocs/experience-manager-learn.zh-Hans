---
title: 开发Asset Compute员工
description: Asset Compute工作程序是Asset Compute项目的核心，因为它提供自定义功能，该功能执行或协调为创建新演绎版而对资源执行的工作。
feature: Asset Compute Microservices
version: Experience Manager as a Cloud Service
doc-type: Tutorial
jira: KT-6282
thumbnail: KT-6282.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 7d51ec77-c785-4b89-b717-ff9060d8bda7
duration: 451
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1386'
ht-degree: 0%

---

# 开发Asset Compute员工

Asset Compute工作程序是Asset Compute项目的核心，因为它提供自定义功能，该功能执行或协调为创建新演绎版而对资源执行的工作。

Asset Compute项目会自动生成一个简单的工作程序，该程序无需进行任何转换即会将资源的原始二进制文件复制到命名演绎版中。 在本教程中，我们将修改此工作程序，以制作更有趣的演绎版，以说明Asset Compute工作程序的强大功能。

我们将创建一个Asset Compute工作程序，该工作程序会生成一个新的水平图像演绎版，通过模糊版本的资源来覆盖资源演绎版左侧的空白区域。 最终演绎版的宽度、高度和模糊已进行参数化。

## Asset Compute工作进程调用的逻辑流程

Asset Compute工作进程在`renditionCallback(...)`函数中实施Asset Compute SDK工作进程API合同，从概念上讲：

+ __输入：__ AEM资产的原始二进制文件和处理配置文件参数
+ __输出：__&#x200B;要添加到AEM资源的一个或多个演绎版

![Asset Compute工作进程逻辑流](./assets/worker/logical-flow.png)

1. AEM创作服务调用Asset Compute工作进程，提供资产的&#x200B;__(1a)__&#x200B;原始二进制文件（`source`参数）以及&#x200B;__(1b)__&#x200B;处理配置文件中定义的任何参数（`rendition.instructions`参数）。
1. Asset Compute SDK可协调自定义Asset Compute元数据工作进程的`renditionCallback(...)`函数的执行，从而根据资产的原始二进制文件&#x200B;__(1a)__&#x200B;和任何参数&#x200B;__(1b)__&#x200B;生成新的二进制文件演绎版。

   + 在本教程中，将创建演绎版“进行中”，这意味着工作进程构成演绎版，但源二进制文件也可以发送到其他Web服务API来生成演绎版。

1. Asset Compute Worker将新演绎版的二进制数据保存到`rendition.path`。
1. 写入到`rendition.path`的二进制数据将通过Asset Compute SDK传输到AEM创作服务，并公开为&#x200B;__(4a)__&#x200B;文本演绎版，__(4b)__&#x200B;保留到该资产的元数据节点。

上图列出了Asset Compute开发人员面临的问题以及指向Asset Compute工作程序调用的逻辑流。 奇怪的是，Asset Compute执行的[内部详细信息](https://experienceleague.adobe.com/docs/asset-compute/using/extend/custom-application-internals.html?lang=zh-Hans)可用，但只能依赖公共Asset Compute SDK API合同。

## 工人的解剖学

所有Asset Compute员工遵循相同的基本结构和输入/输出合同。

```javascript
'use strict';

// Any npm module imports used by the worker
const { worker, SourceCorruptError } = require('@adobe/asset-compute-sdk');
const fs = require('fs').promises;

/**
Exports the worker implemented by a custom rendition callback function, which parametrizes the input/output contract for the worker.
 + `source` represents the asset's original binary used as the input for the worker.
 + `rendition` represents the worker's output, which is the creation of a new asset rendition.
 + `params` are optional parameters, which map to additional key/value pairs, including a sub `auth` object that contains Adobe I/O access credentials.
**/
exports.main = worker(async (source, rendition, params) => {
    // Perform any necessary source (input) checks
    const stats = await fs.stat(source.path);
    if (stats.size === 0) {
        // Throw appropriate errors whenever an erring condition is met
        throw new SourceCorruptError('source file is empty');
    }

    // Access any custom parameters provided via the Processing Profile configuration
    let param1 = rendition.instructions.exampleParam;

    /** 
    Perform all work needed to transform the source into the rendition.
    
    The source data can be accessed:
        + In the worker via a file available at `source.path`
        + Or via a presigned GET URL at `source.url`
    **/
    if (success) {
        // A successful worker must write some data back to `renditions.path`. 
        // This example performs a trivial 1:1 copy of the source binary to the rendition
        await fs.copyFile(source.path, rendition.path);
    } else {
        // Upon failure an Asset Compute Error (exported by @adobe/asset-compute-commons) should be thrown.
        throw new GenericError("An error occurred!", "example-worker");
    }
});

/**
Optionally create helper classes or functions the worker's rendition callback function invokes to help organize code.

Code shared across workers, or to complex to be managed in a single file, can be broken out across supporting JavaScript files in the project and imported normally into the worker. 
**/
function customHelperFunctions() { ... }
```

## 打开worker index.js

![自动生成的index.js](./assets/worker/autogenerated-index-js.png)

1. 确保在VS代码中打开Asset Compute项目
1. 导航到`/actions/worker`文件夹
1. 打开`index.js`文件

这是我们将在本教程中修改的工作者JavaScript文件。

## 安装和导入支持npm的模块

Asset Compute项目基于Node.js，因此受益于强大的[npm模块生态系统](https://npmjs.com)。 要利用npm模块，我们必须首先将其安装到我们的Asset Compute项目中。

在此辅助进程中，我们利用[jimp](https://www.npmjs.com/package/jimp)直接在Node.js代码中创建和处理演绎版图像。

>[!WARNING]
>
>Asset Compute并非支持所有用于资产操作的npm模块。 不支持依赖存在应用程序（如ImageMagick或其他依赖操作系统的库）的npm模块。 最好限制为仅使用仅限JavaScript的npm模块。

1. 在Asset Compute项目的根目录中打开命令行（可以通过&#x200B;__终端>新终端__&#x200B;在VS Code中完成）并执行命令：

   ```
   $ npm install jimp
   ```

1. 将`jimp`模块导入到辅助进程代码中，以便通过`Jimp` JavaScript对象使用该模块。
更新辅助进程`index.js`顶部的`require`指令以从`jimp`模块导入`Jimp`对象：

   ```javascript
   'use strict';
   
   const Jimp = require('jimp');
   const { worker, SourceCorruptError } = require('@adobe/asset-compute-sdk');
   const fs = require('fs').promises;
   
   exports.main = worker(async (source, rendition, params) => {
       // Check handle a corrupt input source
       const stats = await fs.stat(source.path);
       if (stats.size === 0) {
           throw new SourceCorruptError('source file is empty');
       }
   
       // Do work here
   });
   ```

## 读取参数

Asset Compute工作人员可以读取参数，这些参数可以通过处理在AEM as a Cloud Service Author服务中定义的用户档案来传入。 参数通过`rendition.instructions`对象传递到辅助进程。

可以通过访问辅助进程代码中的`rendition.instructions.<parameterName>`来读取这些内容。

此处我们将读入可配置演绎版的`SIZE`、`BRIGHTNESS`和`CONTRAST`，如果尚未通过处理配置文件提供默认值，则提供默认值。 请注意，从AEM as a Cloud Service处理配置文件调用时，`renditions.instructions`将作为字符串传入，因此请确保将它们转换为辅助代码中的正确数据类型。

```javascript
'use strict';

const Jimp = require('jimp');
const { worker, SourceCorruptError } = require('@adobe/asset-compute-sdk');
const fs = require('fs').promises;

exports.main = worker(async (source, rendition, params) => {
    const stats = await fs.stat(source.path);
    if (stats.size === 0) {
        throw new SourceCorruptError('source file is empty');
    }

    // Read in parameters and set defaults if parameters are provided
    // Processing Profiles pass in instructions as Strings, so make sure to parse to correct data types
    const SIZE = parseInt(rendition.instructions.size) || 800; 
    const CONTRAST = parseFloat(rendition.instructions.contrast) || 0;
    const BRIGHTNESS = parseFloat(rendition.instructions.brightness) || 0;

    // Do work here
}
```

## 抛出错误{#errors}

Asset Compute工作程序可能会遇到导致错误的情况。 Adobe Asset Compute SDK提供了[一整套预定义错误](https://github.com/adobe/asset-compute-commons#asset-compute-errors)，遇到此类情况时可能会引发这些错误。 如果未应用特定的错误类型，则可以使用`GenericError`，也可以定义特定的自定义`ClientErrors`。

在开始处理演绎版之前，请检查以确保所有参数在此工作程序的上下文中有效并受支持：

+ 请确保`SIZE`、`CONTRAST`和`BRIGHTNESS`的节目指令参数有效。 如果不存在，则引发自定义错误`RenditionInstructionsError`。
   + 此文件底部定义了扩展`ClientError`的自定义`RenditionInstructionsError`类。 在为辅助进程[编写测试](../test-debug/test.md)时使用特定的自定义错误。

```javascript
'use strict';

const Jimp = require('jimp');
// Import the Asset Compute SDK provided `ClientError` 
const { worker, SourceCorruptError, ClientError } = require('@adobe/asset-compute-sdk');
const fs = require('fs').promises;

exports.main = worker(async (source, rendition, params) => {
    const stats = await fs.stat(source.path);
    if (stats.size === 0) {
        throw new SourceCorruptError('source file is empty');
    }

    // Read in parameters and set defaults if parameters are provided
    const SIZE = parseInt(rendition.instructions.size) || 800; 
    const CONTRAST = parseFloat(rendition.instructions.contrast) || 0;
    const BRIGHTNESS = parseFloat(rendition.instructions.brightness) || 0;

    if (SIZE <= 10 || SIZE >= 10000) {
        // Ensure size is within allowable bounds
        throw new RenditionInstructionsError("'size' must be between 10 and 1,0000");
    } else if (CONTRAST <= -1 || CONTRAST >= 1) {
        // Ensure contrast is valid value
        throw new RenditionInstructionsError("'contrast' must between -1 and 1");
    } else if (BRIGHTNESS <= -1 || BRIGHTNESS >= 1) {
        // Ensure contrast is valid value
        throw new RenditionInstructionsError("'brightness' must between -1 and 1");
    }

    // Do work here
}

// Create a new ClientError to handle invalid rendition.instructions values
class RenditionInstructionsError extends ClientError {
    constructor(message) {
        // Provide a:
        // + message: describing the nature of this erring condition
        // + name: the name of the error; usually same as class name
        // + reason: a short, searchable, unique error token that identifies this error
        super(message, "RenditionInstructionsError", "rendition_instructions_error");

        // Capture the strack trace
        Error.captureStackTrace(this, RenditionInstructionsError);
    }
}
```

## 创建节目

通过读取、净化和验证参数，写入代码以生成演绎版。 用于生成演绎版的伪代码如下所示：

1. 以通过`size`参数指定的平方尺寸创建新`renditionImage`画布。
1. 从源资产的二进制文件创建`image`对象
1. 使用&#x200B;__Jimp__&#x200B;库转换图像：
   + 将原始图像裁切为居中的正方形
   + 从“平方”图像的中心剪切一个圆
   + 缩放以适合由`SIZE`参数值定义的尺寸
   + 根据`CONTRAST`参数值调整对比度
   + 根据`BRIGHTNESS`参数值调整亮度
1. 将转换后的`image`放在具有透明背景的`renditionImage`的中心
1. 将合成的内容`renditionImage`写入`rendition.path`，以便将其保存回AEM作为资源演绎版。

此代码使用[Jimp API](https://github.com/oliver-moran/jimp#jimp)执行这些图像转换。

Asset Compute工作程序必须同步完成其工作，并且在工作程序的`renditionCallback`完成之前，`rendition.path`必须完全写回。 这需要使用`await`运算符同步调用异步函数。 如果您不熟悉JavaScript异步功能以及如何使其以同步方式执行，请熟悉[JavaScript的等待运算符](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/await)。

完成的工作程序`index.js`应如下所示：

```javascript
'use strict';

const Jimp = require('jimp');
const { worker, SourceCorruptError, ClientError } = require('@adobe/asset-compute-sdk');
const fs = require('fs').promises;

exports.main = worker(async (source, rendition, params) => {
    const stats = await fs.stat(source.path);
    if (stats.size === 0) {
        throw new SourceCorruptError('source file is empty');
    }

    // Read/parse and validate parameters
    const SIZE = parseInt(rendition.instructions.size) || 800; 
    const CONTRAST = parseFloat(rendition.instructions.contrast) || 0;
    const BRIGHTNESS = parseFloat(rendition.instructions.brightness) || 0;

    if (SIZE <= 10 || SIZE >= 10000) {
        throw new RenditionInstructionsError("'size' must be between 10 and 1,0000");
    } else if (CONTRAST <= -1 || CONTRAST >= 1) {
        throw new RenditionInstructionsError("'contrast' must between -1 and 1");
    } else if (BRIGHTNESS <= -1 || BRIGHTNESS >= 1) {
        throw new RenditionInstructionsError("'brightness' must between -1 and 1");
    }

    // Create target rendition image 
    let renditionImage =  new Jimp(SIZE, SIZE, 0x0);

    // Read and perform transformations on the source binary image
    let image = await Jimp.read(source.path);

    // Crop a circle from the source asset, and then apply contrast and brightness
    image.crop(
            image.bitmap.width < image.bitmap.height ? 0 : (image.bitmap.width - image.bitmap.height) / 2,
            image.bitmap.width < image.bitmap.height ? (image.bitmap.height - image.bitmap.width) / 2 : 0,
            image.bitmap.width < image.bitmap.height ? image.bitmap.width : image.bitmap.height,
            image.bitmap.width < image.bitmap.height ? image.bitmap.width : image.bitmap.height
        )   
        .circle()
        .scaleToFit(SIZE, SIZE)
        .contrast(CONTRAST)
        .brightness(BRIGHTNESS);

    // Place the transformed image onto the transparent renditionImage to save as PNG
    renditionImage.composite(image, 0, 0)

    // Write the final transformed image to the asset's rendition
    await renditionImage.writeAsync(rendition.path);
});

// Custom error used for renditions.instructions parameter checking
class RenditionInstructionsError extends ClientError {
    constructor(message) {
        super(message, "RenditionInstructionsError", "rendition_instructions_error");
        Error.captureStackTrace(this, RenditionInstructionsError);
    }
}
```

## 运行辅助进程

现在，工作进程代码已经完成，并且之前已在[manifest.yml](./manifest.md)中注册和配置，可以使用本地Asset Compute Development Tool执行该代码以查看结果。

1. 从Asset Compute项目的根目录
1. 执行`aio app run`
1. 等待Asset Compute开发工具在新窗口中打开
1. 在&#x200B;__选择文件……__&#x200B;下拉列表中，选择要处理的示例图像
   + 选择用作源资产二进制文件的示例图像文件
   + 如果尚不存在，请点按左侧的&#x200B;__(+)__，上传[示例图像](../assets/samples/sample-file.jpg)文件，然后刷新开发工具浏览器窗口
1. 在此辅助进程上更新`"name": "rendition.png"`以生成透明PNG。
   + 请注意，此“name”参数仅用于开发工具，不应依赖。

   ```json
   {
       "renditions": [
           {
               "worker": "...",
               "name": "rendition.png"
           }
       ]
   }
   ```

1. 点按&#x200B;__运行__&#x200B;并等待生成节目
1. __呈现版本__&#x200B;部分预览生成的呈现版本。 点按演绎版预览以下载完整演绎版

   ![默认PNG演绎版](./assets/worker/default-rendition.png)

### 使用参数运行辅助进程

通过在Asset Compute开发工具中提供参数作为演绎版参数JSON上的键/值对，可以模拟通过处理配置文件配置传入的参数。

>[!WARNING]
>
>在本地开发期间，当作为Cloud Service处理配置文件字符串从AEM传入时，可以使用各种数据类型传入值，因此，需要时，请确保解析正确的数据类型。
> 例如，Jimp的`crop(width, height)`函数要求其参数为`int`的参数。如果未将`parseInt(rendition.instructions.size)`解析为int，则对`jimp.crop(SIZE, SIZE)`的调用将失败，因为参数是不兼容的“String”类型。

我们的代码接受以下参数：

+ `size`定义演绎版的大小（高度和宽度为整数）
+ `contrast`定义对比度调整，必须介于–1和1之间，作为浮点数
+ `brightness`定义明亮调整，必须介于–1和1之间，为浮点数

在辅助进程`index.js`中通过以下方式读取这些内容：

+ `const SIZE = parseInt(rendition.instructions.size) || 800`
+ `const CONTRAST = parseFloat(rendition.instructions.contrast) || 0`
+ `const BRIGHTNESS = parseFloat(rendition.instructions.brightness) || 0`

1. 更新节目参数以自定义大小、对比度和亮度。

   ```json
   {
       "renditions": [
           {
               "worker": "...",
               "name": "rendition.png",
               "size": "450",
               "contrast": "0.30",
               "brightness": "0.15"
           }
       ]
   }
   ```

1. 再次点按&#x200B;__运行__
1. 点按演绎版预览以下载并查看生成的演绎版。 请注意其尺寸以及对比度和亮度与默认演绎版相比的变化。

   ![参数化的PNG演绎版](./assets/worker/parameterized-rendition.png)

1. 将其他图像上传到&#x200B;__Source文件__&#x200B;下拉列表，然后尝试使用其他参数针对这些图像运行辅助进程！

## Github上的Worker index.js

Github上的最终`index.js`位于：

+ [aem-guides-wknd-asset-compute/actions/worker/index.js](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/actions/worker/index.js)

## 疑难解答

+ [节目返回部分绘制/损坏](../troubleshooting.md#rendition-returned-partially-drawn-or-corrupt)
