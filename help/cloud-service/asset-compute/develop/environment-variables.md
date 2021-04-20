---
title: 配置环境变量以实现Asset compute可扩展性
description: 环境变量在.env文件中进行维护以用于本地开发，并用于提供本地开发所需的Adobe I/O凭据和云存储凭据。
feature: Asset Compute Microservices
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6270
thumbnail: KT-6270.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '596'
ht-degree: 0%

---


# 配置环境变量

![dot env文件](assets/environment-variables/dot-env-file.png)

在开始开发Asset compute工作者之前，请确保项目已配置Adobe I/O和云存储信息。 此信息存储在项目的`.env`中，该项目仅用于本地开发，而不保存在Git中。 `.env`文件提供了一种将键/值对公开到本地Asset compute本地开发环境的便捷方式。 当[将](../deploy/runtime.md)Asset computeWorker部署到Adobe I/O Runtime时，不使用`.env`文件，而是通过环境变量传递值子集。 其他自定义参数和机密也可以存储在`.env`文件中，如第三方Web服务的开发凭据。

## 引用`private.key`

![私钥](assets/environment-variables/private-key.png)

打开`.env`文件，取消`ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH`键的注释，并提供与添加到您的Adobe I/O FireFly项目的公共证书对应的`private.key`文件系统中的绝对路径。

+ 如果您的密钥对是由Adobe I/O生成的，则会自动将其下载为`config.zip`的一部分。
+ 如果您向Adobe I/O提供了公钥，则您还应拥有相应的私钥。
+ 如果您没有这些键对，则可以在以下位置的底部生成新的键对或上传新的公钥：
   [https://console.adobe.com](https://console.adobe.io) >您的Asset compute Firefly项目> Workspaces @ Development > Service Account(JWT)。

记住，不应将`private.key`文件签入Git，因为它包含机密，而应将其存储在项目外的安全位置。

例如，在macOS上，这可能如下：

```
...
ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH=/Users/example-user/credentials/aem-guides-wknd-asset-compute/private.key
...
```

## 配置云存储凭据

本地开发Asset compute工作者需要访问[云存储](../set-up/accounts-and-services.md#cloud-storage)。 用于本地开发的云存储凭据在`.env`文件中提供。

本教程倾向于使用Azure Blob存储，但可以改用Amazon S3和`.env`文件中的相应键。

### 使用Azure Blob存储

在`.env`文件中取消注释并填充以下键，然后使用Azure Portal上提供的云存储的值填充这些键。

![Azure Blob存储](./assets/environment-variables/azure-portal-credentials.png)

1. `AZURE_STORAGE_CONTAINER_NAME`键的值
1. `AZURE_STORAGE_ACCOUNT`键的值
1. `AZURE_STORAGE_KEY`键的值

例如，这可能类似（仅用于插图的值）：

```
...
AZURE_STORAGE_ACCOUNT=aemguideswkndassetcomput
AZURE_STORAGE_KEY=Va9CnisgdbdsNJEJBqXDyNbYppbGbZ2V...OUNY/eExll0vwoLsPt/OvbM+B7pkUdpEe7zJhg==
AZURE_STORAGE_CONTAINER_NAME=asset-compute
...
```

生成的`.env`文件如下所示：

![Azure Blob存储凭据](assets/environment-variables/cloud-storage-credentials.png)

如果您未使用Microsoft Azure Blob存储，请删除或保留这些注释（通过用`#`前缀）。

### 使用Amazon S3云存储{#amazon-s3}

如果您使用Amazon S3云存储取消注释，并在`.env`文件中填充以下键。

例如，这可能类似（仅用于插图的值）：

```
...
S3_BUCKET=aemguideswkndassetcompute
AWS_ACCESS_KEY_ID=KKIXZLZYNLXJLV24PLO6
AWS_SECRET_ACCESS_KEY=Ba898CnisgabdsNJEJBqCYyVrYttbGbZ2...OiNYExll0vwoLsPtOv
AWS_REGION=us-east-1
...
```

## 验证项目配置

在配置生成的Asset compute项目后，请在进行代码更改之前验证配置，以确保在`.env`文件中设置支持服务。

要开始Asset compute项目的Adobe Development Tool，请执行以下操作：

1. 在Asset compute项目根目录中打开命令行（在VS代码中，可以通过“终端”>“新建终端”在IDE中直接打开），然后执行命令：

   ```
   $ aio app run
   ```

1. 本地Asset compute开发工具将在默认的Web浏览器中打开，位于&#x200B;__http://localhost:9000__。

   ![aio app run](assets/environment-variables/aio-app-run.png)

1. 当开发工具初始化时，观察命令行输出和Web浏览器中是否有错误消息。
1. 要停止Asset compute开发工具，请点按执行`aio app run`的窗口中的`Ctrl-C`以终止该进程。

## 疑难解答

+ [由于缺少private.key，开发工具无法开始](../troubleshooting.md#missing-private-key)
