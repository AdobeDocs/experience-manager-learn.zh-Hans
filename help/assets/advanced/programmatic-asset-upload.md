---
title: 将程序化资产上传到AEM as a Cloud Service
description: 了解如何使用@adobe/aem-upload Node.js库将资源上传到AEM as a Cloud Service。
version: Experience Manager as a Cloud Service
topic: Development, Content Management
feature: Asset Management
role: Developer, Architect
level: Intermediate
last-substantial-update: 2025-11-14T00:00:00Z
doc-type: Tutorial
jira: KT-19571
thumbnail: KT-19571.png
source-git-commit: bf996405c360c77475d9f76d5de9bcd4fde3c163
workflow-type: tm+mt
source-wordcount: '1585'
ht-degree: 1%

---


# 将程序化资产上传到AEM as a Cloud Service

了解如何使用使用[aem-upload](https://github.com/adobe/aem-upload) Node.js库的客户端应用程序将资源上传到AEM as a Cloud Service环境。

## 你将学到的内容

在本教程中，您将学习：

+ 如何使用&#x200B;_直接二进制上传_&#x200B;方法，通过[aem-upload](https://github.com/adobe/aem-upload) Node.js库将资产上传到AEM as a Cloud Service环境(RDE、Dev、Stage、Prod)。
+ 如何配置和运行[aem-asset-upload-sample](./assets/programmatic-asset-upload/aem-asset-upload-sample.zip)应用程序以将资源上传到AEM as a Cloud Service环境。
+ 查看示例应用程序代码并了解实施详细信息。
+ 了解将程序化资产上传到AEM as a Cloud Service环境的最佳实践。

## 了解&#x200B;_直接二进制上传_&#x200B;方法

通过&#x200B;_直接二进制上传_&#x200B;方法，您可以使用&#x200B;_预签名URL_，直接将文件从源系统&#x200B;_上传到AEM as a Cloud Service环境中的云存储_。 它无需通过AEM的Java进程路由二进制数据，从而加快上传速度并减少服务器负载。

在运行示例应用程序之前，我们先了解一下直接二进制上传流程。

在直接二进制上传流中，二进制数据会使用预签名URL直接上传到云存储。 AEM as a Cloud Service负责进行轻量级的处理，如生成预签名的URL，并通知AEM Asset Compute服务上传已完成。 以下逻辑流程图说明了直接二进制上传流程。

![直接二进制上传流](./assets/programmatic-asset-upload/direct-binary-asset-upload-flow.png)

### aem-upload库

[aem-upload](https://github.com/adobe/aem-upload) Node.js库抽象了&#x200B;_直接二进制上传_&#x200B;方法的实现详细信息。 它提供两个类来编排上载过程：

+ **FileSystemUpload** — 从本地文件系统上传文件时使用它，包括对目录结构的支持
+ **DirectBinaryUpload** — 使用它可以对二进制上传过程执行更细粒度的控制，例如从流或缓冲区上传

>[!CAUTION]
>
>在Java中没有[aem-upload](https://github.com/adobe/aem-upload)库的等效项。 必须使用Node.js编写客户端应用程序才能使用&#x200B;_直接二进制上传_&#x200B;方法。 有关其他信息，请参阅[Experience Manager Assets API和操作](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/assets/admin/developer-reference-material-apis#use-cases-and-apis)页面。

## 示例应用程序

使用[aem-asset-upload-sample](./assets/programmatic-asset-upload/aem-asset-upload-sample.zip)应用程序了解程序化资产上传过程。 示例应用程序演示了使用`FileSystemUpload`aem-upload`DirectBinaryUpload`库中的[和](https://github.com/adobe/aem-upload)类。

### 先决条件

运行示例应用程序之前，请确保满足以下先决条件：

+ AEM as a Cloud Service创作环境，例如快速开发环境(RDE)、开发环境等。
+ Node.js（最新LTS版本）
+ 对Node.js和npm的基本了解

>[!CAUTION]
>
> 您不能使用AEM as a Cloud Service SDK(又称本地AEM实例)来测试程序化资产上传过程。 您必须使用AEM as a Cloud Service环境，例如快速开发环境(RDE)、开发环境等。

### 下载示例应用程序

1. 下载[aem-asset-upload-sample](./assets/programmatic-asset-upload/aem-asset-upload-sample.zip)应用程序zip文件并将其解压缩。

   ```bash
   $ unzip aem-asset-upload-sample.zip
   ```

1. 在您喜爱的代码编辑器中打开提取的文件夹。

   ```bash
   $ cd aem-asset-upload-sample
   $ code .
   ```

1. 使用代码编辑器终端安装依赖项。

   ```bash
   $ npm install
   ```

   ![示例应用程序](./assets/programmatic-asset-upload/install-dependencies.png)

### 配置示例应用程序

运行示例应用程序之前，必须使用必要的AEM as a Cloud Service环境详细信息(如AEM创作URL、_身份验证方法_&#x200B;和资源文件夹路径)对其进行配置。

有&#x200B;_多种身份验证方法_&#x200B;受[aem-upload](https://github.com/adobe/aem-upload) Node.js库支持。 下表总结了支持的&#x200B;_身份验证方法_&#x200B;及其用途。

| | 基本身份验证 | [本地开发令牌](https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/local-development-access-token) | [服务凭据](https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/service-credentials) | [OAuth S2S](https://developer.adobe.com/developer-console/docs/guides/authentication/ServerToServerAuthentication/) | [OAuth Web应用程序](https://developer.adobe.com/developer-console/docs/guides/authentication/UserAuthentication/implementation#oauth-web-app-credential) | [OAuth SPA](https://developer.adobe.com/developer-console/docs/guides/authentication/UserAuthentication/implementation#oauth-single-page-app-credential) |
|---|---|---|---|---|---|---|
| 是否受支持？ | &amp;amp；检查； | &amp;amp；检查； | &amp;amp；检查； | &amp;amp；交叉； | &amp;amp；交叉； | &amp;amp；交叉； |
| 用途 | 本地开发 | 本地开发 | 生产 | 不适用 | 不适用 | 不适用 |

要配置示例应用程序，请执行以下步骤：

1. 将`env.example`文件复制到`.env`文件。

   ```bash
   $ cp env.example .env
   ```

1. 打开`.env`文件并使用AEM as a Cloud Service创作URL更新`AEM_URL`环境变量。

1. 从以下选项中选择身份验证方法并更新相应的环境变量。

>[!BEGINTABS]

>[!TAB 基本身份验证]

要使用基本身份验证，您需要在AEM as a Cloud Service环境中创建用户。

1. 登录到您的AEM as a Cloud Service环境。

1. 导航到&#x200B;**工具** > **安全性** > **用户**，然后单击&#x200B;**创建**&#x200B;按钮。

   ![创建用户](./assets/programmatic-asset-upload/create-user.png)

1. 输入用户详细信息

   ![用户详细信息](./assets/programmatic-asset-upload/user-details.png)

1. 在&#x200B;**组**&#x200B;选项卡中，添加&#x200B;**DAM用户**&#x200B;组。 单击&#x200B;**保存并关闭**&#x200B;按钮。

   ![添加DAM用户组](./assets/programmatic-asset-upload/add-dam-users-group.png)

1. 使用所创建的用户的用户名和密码更新`AEM_USERNAME`和`AEM_PASSWORD`环境变量。

>[!TAB 本地开发令牌]

要获取本地开发令牌，您需要使用&#x200B;**AEM** Developer Console。 生成的令牌为JSON Web令牌(JWT)类型。

1. 登录到[Adobe Cloud Manager](https://experience.adobe.com/#/@aem/cloud-manager)，然后导航到所需的&#x200B;**环境**&#x200B;详细信息页面。 单击&#x200B;**“……”**&#x200B;并选择&#x200B;**Developer Console**。

   ![开发人员控制台](./assets/programmatic-asset-upload/developer-console.png)

1. 登录到AEM Developer Console，然后使用&#x200B;_新建控制台_&#x200B;按钮切换到更新的控制台。

1. 从&#x200B;**工具**&#x200B;部分中，选择&#x200B;**集成**，然后单击&#x200B;**获取本地令牌**&#x200B;按钮。

   ![获取本地令牌](./assets/programmatic-asset-upload/get-local-token.png)

1. 复制令牌值并用令牌值更新`AEM_BEARER_TOKEN`环境变量。

请注意，本地开发令牌的有效时间为24小时，并且是为生成令牌的用户颁发的。

>[!TAB 服务凭据]

要获取服务凭据，您需要使用&#x200B;**AEM** Developer Console。 它用于使用[jwt-auth](https://www.npmjs.com/package/@adobe/jwt-auth) npm模块生成JSON Web令牌(JWT)类型的令牌。

1. 登录到[Adobe Cloud Manager](https://experience.adobe.com/#/@aem/cloud-manager)，然后导航到所需的&#x200B;**环境**&#x200B;详细信息页面。 单击&#x200B;**“……”**&#x200B;并选择&#x200B;**Developer Console**。

   ![开发人员控制台](./assets/programmatic-asset-upload/developer-console.png)

1. 登录到AEM Developer Console，然后使用&#x200B;_新建控制台_&#x200B;按钮切换到更新的控制台。

1. 从&#x200B;**工具**&#x200B;部分中，选择&#x200B;**集成**，然后单击&#x200B;**新建技术帐户**&#x200B;按钮。

   ![获取服务凭据](./assets/programmatic-asset-upload/get-service-credentials.png)

1. 单击&#x200B;**查看**&#x200B;选项以复制服务凭据JSON。

   ![服务凭据](./assets/programmatic-asset-upload/service-credentials.png)

1. 在示例应用程序的根目录中创建一个`service-credentials.json`文件，并将服务凭据JSON粘贴到文件中。

1. 使用service-credentials.json文件的路径更新`AEM_SERVICE_CREDENTIALS_FILE`环境变量。

1. 确保服务凭据用户具有将资源上传到AEM as a Cloud Service环境的必要权限。 有关详细信息，请参阅[在AEM中配置访问权限](https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/service-credentials#configure-access-in-aem)页面。

>[!ENDTABS]

以下是配置了所有三种身份验证方法的完整示例`.env`文件。

```
# AEM Environment Configuration
# Copy this file to .env and fill in your AEM as a Cloud Service details

# AEM as a Cloud Service Author URL (without trailing slash)
# Example: https://author-p12345-e67890.adobeaemcloud.com
AEM_URL=https://author-p63947-e1733365.adobeaemcloud.com

# Upload Configuration
# Target folder in AEM DAM where assets will be uploaded
TARGET_FOLDER=/content/dam

# DirectBinaryUpload Remote URLs (required for DirectBinaryUpload example)
# URLs for remote files to upload in the DirectBinaryUpload example
# These demonstrate uploading from remote sources (URLs, CDNs, APIs)
REMOTE_FILE_URL_1=https://placehold.co/600x400/red/white?text=Adobe+Experience+Manager+Assets

################################################################
# Authentication - Choose one of the following methods:
################################################################

# Method 1: Service Credentials (RECOMMENDED for production)
# Download service credentials JSON from AEM Developer Console and save it locally
# Then provide the path to the file here
AEM_SERVICE_CREDENTIALS_FILE=./service-credentials.json

# Method 2: Bearer Token Authentication (for manual testing)
AEM_BEARER_TOKEN=eyJhbGciOiJSUzI1NiIsIng1dSI6Imltc19uYTEta2V5LWF0LTEuY2VyIiwia2lkIjoiaW1zX25hM....fsdf-Rgt5hm_8FHutTyNQnkj1x1SUs5OkqUfJaGBaKBKdqQ

# Method 3: Basic Authentication (for development/testing only)
AEM_USERNAME=asset-uploader-local-user
AEM_PASSWORD=asset-uploader-local-user

# Optional: Enable detailed logging
DEBUG=false
```

### 运行示例应用程序

示例应用程序显示了将示例资源上传到AEM as a Cloud Service环境的三种不同方法。

1. **FileSystemUpload** — 从支持目录结构和自动文件夹创建的本地文件系统上传文件
1. **DirectBinaryUpload** — 上传[远程文件](https://placehold.co/600x400/red/white?text=Adobe+Experience+Manager+Assets)。 文件二进制文件在上传到AEM as a Cloud Service环境之前在内存中进行缓冲。
1. **批量上传** — 使用自动重试逻辑和错误恢复以批量方式从本地文件系统上传多个文件。 在后台，它使用`FileSystemUpload`类从本地文件系统上传文件。

要上传的资产位于`sample-assets`文件夹中，并包含`img`、`video`和`doc`子文件夹，每个子文件夹都包含几个示例资产。

1. 要运行示例应用程序，请使用以下命令：

```bash
$ npm start
```

1. 从以下选项中输入所需的选项&#x200B;_数字_：

```
╔════════════════════════════════════════════════════════════╗
║      AEM Asset Upload Sample Application                   ║
║      Demonstrating @adobe/aem-upload library               ║
╚════════════════════════════════════════════════════════════╝

Choose an upload method:

1. FileSystemUpload - Upload files from local filesystem with auto-folder creation
2. DirectBinaryUpload - Upload from remote URLs/streams to AEM
3. Batch Upload - Upload multiple files in batches with retry logic
4. Exit
```

以下选项卡显示了AEM as a Cloud Service环境中每个上传方法的示例应用程序运行、其输出和上传的资源。

>[!BEGINTABS]

>[!TAB 文件系统上载]

1. `FileSystemUpload`选项的示例应用程序输出：

```bash
...
Upload Summary:
──────────────────────────────────────────────────
Total files: 5
Successful: 5
Failed: 0
Total time: 2.67s
──────────────────────────────────────────────────
✓ 
All files uploaded successfully!
```

1. 使用AEM as a Cloud Service环境中的`FileSystemUpload`选项上传了Assets：

   ![已在AEM as a Cloud Service环境中使用FileSystemUpload类上载资源](./assets/programmatic-asset-upload/uploaded-assets-aem-file-system-upload.png)

>[!TAB DirectBinaryUpload]

1. `DirectBinaryUpload`选项的示例应用程序输出：

```bash
...
Upload Summary:
──────────────────────────────────────────────────
Total files: 1
Successful: 1
Total time: 561ms
──────────────────────────────────────────────────

✅ Successfully uploaded to AEM: https://author-p63947-e1733365.adobeaemcloud.com/ui#/aem/assets.html/content/dam?appId=aemshell
  → remote-file-1.png
    Source: https://placehold.co/600x400/red/white?text=Adobe+Experience+Manager+Assets
✓ 
All files uploaded successfully!
```

1. 使用AEM as a Cloud Service环境中的`DirectBinaryUpload`选项上传了Assets：

![已在AEM as a Cloud Service环境中使用DirectBinaryUpload类上传资源](./assets/programmatic-asset-upload/uploaded-assets-aem-direct-binary-upload.png)

>[!TAB 批次上传]

1. `Batch Upload`选项的示例应用程序输出：

```bash
...
ℹ Found 4 item(s) to upload in batches (directories + files)
ℹ Batch size: 2 (small for demo, use 10-50 for production)

...

✓ Batch 2 completed in 2.79s

Upload Summary:
──────────────────────────────────────────────────
Total files: 5
Successful: 5
Failed: 0
Total time: 4.50s
──────────────────────────────────────────────────
✓ 
All files uploaded successfully!
```

1. 使用AEM as a Cloud Service环境中的`Batch Upload`选项上传了Assets：

![已在AEM as a Cloud Service环境中使用BatchUpload类上传资源](./assets/programmatic-asset-upload/uploaded-assets-aem-batch-upload.png)

>[!ENDTABS]

## 查看示例应用程序代码

示例应用程序的主要入口点是`index.js`文件。 它包含`promptUser`函数，该函数提示用户选择并执行所选示例。

```javascript
/**
 * Prompts user for choice and executes the selected example
 */
function promptUser() {
  rl.question(chalk.bold('Enter your choice (1-4): '), async (answer) => {
    console.log('');

    try {
      switch (answer.trim()) {
        case '1':
          console.log(chalk.bold.green('\n▶ Running FileSystemUpload Example...\n'));
          await filesystemUpload.main();
          break;

        case '2':
          console.log(chalk.bold.green('\n▶ Running DirectBinaryUpload Example...\n'));
          await directBinaryUpload.main();
          break;

        case '3':
          console.log(chalk.bold.green('\n▶ Running Batch Upload Example...\n'));
          await batchUpload.main();
          break;

        case '4':
          rl.close();
          return;

        default:
          console.log(chalk.red('\n✗ Invalid choice. Please enter 1, 2, 3, or 4.\n'));
      }

      // After example completes, ask if user wants to continue
      rl.question(chalk.bold('\nPress Enter to return to menu or Ctrl+C to exit...'), () => {
        displayMenu();
        promptUser();
      });

    } catch (error) {
      console.error(chalk.red('\n✗ Error:'), error.message);
      rl.question(chalk.bold('\nPress Enter to return to menu...'), () => {
        displayMenu();
        promptUser();
      });
    }
  });
}
```

有关完整代码，请参阅示例应用程序中的`index.js`文件。

以下选项卡显示每种上传方法的实施详细信息。

>[!BEGINTABS]

>[!TAB 文件系统上载]

`FileSystemUpload`类用于从支持目录结构和自动文件夹创建的本地文件系统上传文件。

```javascript
...
// Initialize FileSystemUpload
const upload = new FileSystemUpload();

const startTime = Date.now();
const spinner = createSpinner('Preparing upload...');

// Upload options for this specific upload
// For FileSystemUpload, the url should include the target folder path
const fullUrl = `${options.url}${targetFolder}`;

const uploadOptions = new FileSystemUploadOptions()
  .withUrl(fullUrl)
  .withDeepUpload(true);  // Enable recursive upload of subdirectories

// Add HTTP options including headers (auth is already in headers from config)
uploadOptions.withHttpOptions({
  headers: {
    ...options.headers,
    'X-Upload-Source': 'FileSystemUpload-Example'
  }
});

spinner.stop();

// Attach progress event handlers to the upload instance
handleUploadProgress(upload);

// Perform the upload and wait for completion
// Upload the contents (subdirectories and files) not the parent folder
const uploadResult = await upload.upload(uploadOptions, uploadPaths);
const totalTime = Date.now() - startTime;

// Analyze results using shared function
const analysis = analyzeUploadResult(uploadResult);

// Display summary
displayUploadSummary(analysis, totalTime);
...
```

有关完整代码，请参阅示例应用程序中的`examples/filesystem-upload.js`文件。

>[!TAB DirectBinaryUpload]

`DirectBinaryUpload`类用于将远程文件上传到AEM as a Cloud Service环境。

```javascript
...
/**
 * Creates upload file objects for DirectBinaryUpload from remote URLs
 * @param {Array<Object>} remoteFiles - Array of objects with url, fileName, targetFolder
 * @returns {Array<Object>} Array of upload file objects
 */
async function createUploadFilesFromUrls(remoteFiles) {
  const uploadFiles = [];
  
  for (const remoteFile of remoteFiles) {
    logInfo(`Fetching: ${remoteFile.fileName} from ${remoteFile.url}`);
    try {
      const fileBuffer = await fetchRemoteFile(remoteFile.url);
      uploadFiles.push({
        fileName: remoteFile.fileName,
        fileSize: fileBuffer.length,
        blob: fileBuffer,  // DirectBinaryUpload uses 'blob' for buffers
        targetFolder: remoteFile.targetFolder,
        targetFile: `${remoteFile.targetFolder}/${remoteFile.fileName}`,
        sourceUrl: remoteFile.url  // Track source URL for display in summary
      });
      logSuccess(`Downloaded: ${remoteFile.fileName} (${formatBytes(fileBuffer.length)})`);
    } catch (error) {
      logError(`Failed to fetch ${remoteFile.fileName}: ${error.message}`);
    }
  }
  
  return uploadFiles;
}

...

    // Initialize DirectBinaryUpload
    const upload = new DirectBinaryUpload();

    // Fetch remote files and create upload objects
    const uploadFiles = await createUploadFilesFromUrls(remoteFiles);

...    

    // Upload options for each file
    const uploadOptions = new DirectBinaryUploadOptions()
        .withUrl(fullUrl)
        .withUploadFiles([uploadFile]);
    
    // Add HTTP options (auth is already in headers from config)
    uploadOptions
        .withHttpOptions({
        headers: {
            ...options.headers,
            'X-Upload-Source': 'DirectBinaryUpload-Example'
        }
        })
        .withMaxConcurrent(5);

    // Upload individual file and wait for completion
    const uploadResult = await upload.uploadFiles(uploadOptions);
```

有关完整代码，请参阅示例应用程序中的`examples/direct-binary-upload.js`文件。

>[!TAB 批次上传]

它将文件拆分为多个批次，并使用自动重试逻辑和错误恢复以批量方式上传它们。 在后台，它使用`FileSystemUpload`类从本地文件系统上传文件。

```javascript
...
async function uploadInBatches(paths, options, targetFolder, batchSize = 2) {
  const allResults = [];
  const totalPaths = paths.length;
  const totalBatches = Math.ceil(totalPaths / batchSize);

  logInfo(`Processing ${totalPaths} item(s) in ${totalBatches} batch(es)`);

  for (let i = 0; i < totalPaths; i += batchSize) {
    const batchNumber = Math.floor(i / batchSize) + 1;
    const batch = paths.slice(i, i + batchSize);
    
    console.log(`\n${'='.repeat(50)}`);
    logInfo(`Batch ${batchNumber}/${totalBatches} - Uploading ${batch.length} item(s)`);
    console.log('='.repeat(50));

    const batchStartTime = Date.now();
    let retryCount = 0;
    const maxRetries = 3;
    let batchResults = null;

    // Retry logic for failed batches
    while (retryCount <= maxRetries) {
      try {
        // Create a fresh upload instance for each retry to avoid duplicate event listeners
        const upload = new FileSystemUpload();
        
        const fullUrl = `${options.url}${targetFolder}`;
        
        const uploadOptions = new FileSystemUploadOptions()
          .withUrl(fullUrl)
          .withDeepUpload(true);  // Enable recursive upload of subdirectories
        
        // Add HTTP options including headers (auth is already in headers from config)
        uploadOptions.withHttpOptions({
          headers: {
            ...options.headers,
            'X-Upload-Source': 'Batch-Upload-Example',
            'X-Batch-Number': batchNumber
          }
        });

        // Track progress - attach listeners to upload instance
        upload.on('foldercreated', (data) => {
          logSuccess(`Created folder: ${data.folderName} at ${data.targetFolder}`);
        });
        
        let currentFile = '';
        upload.on('filestart', (data) => {
          currentFile = data.fileName;
          logInfo(`Starting: ${currentFile}`);
        });

        upload.on('fileprogress', (data) => {
          const percentage = ((data.transferred / data.fileSize) * 100).toFixed(1);
          process.stdout.write(
            `\r  Progress: ${percentage}% - ${formatBytes(data.transferred)}/${formatBytes(data.fileSize)}`
          );
        });

        upload.on('fileend', (data) => {
          process.stdout.write('\n');
          logSuccess(`Completed: ${data.fileName}`);
        });

        upload.on('fileerror', (data) => {
          // Only show in DEBUG mode (may be retries)
          if (process.env.DEBUG === 'true') {
            process.stdout.write('\n');
            const errorMsg = data.error?.message || data.message || 'Unknown error';
            logWarning(`Error (may retry): ${data.fileName} - ${errorMsg}`);
          }
        });

        // Perform upload and wait for batch completion
        const uploadResult = await upload.upload(uploadOptions, batch);
        
        const batchEndTime = Date.now();
        const batchTime = batchEndTime - batchStartTime;
        
        logSuccess(`Batch ${batchNumber} completed in ${formatTime(batchTime)}`);
        
        // Extract detailed results from the upload result
        batchResults = uploadResult.detailedResult || [];
        break; // Success, exit retry loop

      } catch (error) {
        retryCount++;
        if (retryCount <= maxRetries) {
          logWarning(`Batch ${batchNumber} failed. Retry ${retryCount}/${maxRetries}...`);
          await new Promise(resolve => setTimeout(resolve, 2000 * retryCount)); // Exponential backoff
        } else {
          logError(`Batch ${batchNumber} failed after ${maxRetries} retries: ${error.message}`);
          // Mark all files in batch as failed
          batchResults = batch.map(file => ({
            fileName: path.basename(file),
            error: error,
            success: false
          }));
        }
      }
    }

    if (batchResults) {
      allResults.push(...batchResults);
    }
  }

  return allResults;
}
```

有关完整代码，请参阅示例应用程序中的`examples/batch-upload.js`文件。

>[!ENDTABS]

此外，示例应用程序中的`README.md`文件包含示例应用程序的详细文档。

## 最佳做法

1. **选择正确的身份验证方法：**
将服务凭据用于生产环境、本地开发令牌和基本身份验证仅用于开发/测试。 确保服务凭据用户具有将资源上传到AEM as a Cloud Service环境的必要权限。

1. **选择正确的上传方法：**
使用具有自动文件夹创建的本地文件的FileSystemUpload、具有细粒度控制的流/缓冲区/远程URL的DirectBinaryUpload以及具有需要重试逻辑的1000多个文件的生产环境的批量上传模式。

1. **正确构造DirectBinaryUpload文件对象**
将blob属性（非缓冲区）与必填字段{ fileName， fileSize， blob： buffer， targetFolder }一起使用，并记住DirectBinaryUpload不会自动创建文件夹。

1. **作为引用的示例应用程序：**
示例应用程序非常适合参考程序化资产上传过程的实施详细信息。 您可以将其用作您自己的实施的起点。
