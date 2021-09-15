---
title: 配置环境变量以实现Asset compute可扩展性
description: 环境变量在.env文件中进行维护以用于本地开发，并用于提供本地开发所需的Adobe I/O凭据和云存储凭据。
feature: Asset Compute Microservices
topics: renditions, development
version: Cloud Service
activity: develop
audience: developer
doc-type: tutorial
kt: 6270
thumbnail: KT-6270.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: c63c5c75-1deb-4c16-ba33-e2c338ef6251
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '588'
ht-degree: 0%

---

# 配置环境变量

![点环境文件](assets/environment-variables/dot-env-file.png)

在开始开发Asset compute工作程序之前，请确保为项目配置了Adobe I/O和云存储信息。 此信息存储在项目的`.env`中，该变量仅用于本地开发，而不会保存在Git中。 `.env`文件为向本地Asset compute本地开发环境公开键/值对提供了一种便捷的方法。 当[将](../deploy/runtime.md)Asset compute工作程序部署到Adobe I/O Runtime时，不会使用`.env`文件，而是会通过环境变量传入值的子集。 其他自定义参数和密钥也可以存储在`.env`文件中，例如第三方Web服务的开发凭据。

## 引用`private.key`

![私钥](assets/environment-variables/private-key.png)

打开`.env`文件，取消对`ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH`键的注释，并将文件系统上的绝对路径提供到`private.key`，该路径与添加到Adobe I/OFireFly项目的公共证书配对。

+ 如果您的键对是由Adobe I/O生成的，则会将其作为`config.zip`的一部分自动下载。
+ 如果您提供了Adobe I/O的公钥，则您还应拥有匹配的私钥。
+ 如果您没有这些键对，则可以在的底部生成新的键对或上传新的公钥：
   [https://console.adobe.com](https://console.adobe.io)  >您的Asset computeFirefly项目>开发中的工作区>服务帐户(JWT)。

请记住，`private.key`文件不应签入Git，因为它包含机密，而应存储在项目外的安全位置。

例如，在macOS上，这可能如下所示：

```
...
ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH=/Users/example-user/credentials/aem-guides-wknd-asset-compute/private.key
...
```

## 配置云存储凭据

asset compute工作程序的本地开发需要访问[云存储](../set-up/accounts-and-services.md#cloud-storage)。 用于本地开发的云存储凭据在`.env`文件中提供。

本教程首选使用Azure Blob Storage，但Amazon S3和`.env`文件中的相应键可以改用。

### 使用Azure Blob Storage

在`.env`文件中取消注释并填充以下键，然后使用Azure Portal上配置的云存储的值对其进行填充。

![Azure Blob存储](./assets/environment-variables/azure-portal-credentials.png)

1. `AZURE_STORAGE_CONTAINER_NAME`键的值
1. `AZURE_STORAGE_ACCOUNT`键的值
1. `AZURE_STORAGE_KEY`键的值

例如，这可能类似于（仅供插图使用的值）：

```
...
AZURE_STORAGE_ACCOUNT=aemguideswkndassetcomput
AZURE_STORAGE_KEY=Va9CnisgdbdsNJEJBqXDyNbYppbGbZ2V...OUNY/eExll0vwoLsPt/OvbM+B7pkUdpEe7zJhg==
AZURE_STORAGE_CONTAINER_NAME=asset-compute
...
```

生成的`.env`文件如下所示：

![Azure Blob存储凭据](assets/environment-variables/cloud-storage-credentials.png)

如果您未使用Microsoft Azure Blob Storage，请删除或将它们保留为注释掉（使用`#`前缀）。

### 使用Amazon S3云存储{#amazon-s3}

如果您使用Amazon S3云存储取消注释，并在`.env`文件中填充以下键。

例如，这可能类似于（仅供插图使用的值）：

```
...
S3_BUCKET=aemguideswkndassetcompute
AWS_ACCESS_KEY_ID=KKIXZLZYNLXJLV24PLO6
AWS_SECRET_ACCESS_KEY=Ba898CnisgabdsNJEJBqCYyVrYttbGbZ2...OiNYExll0vwoLsPtOv
AWS_REGION=us-east-1
...
```

## 验证项目配置

配置生成的Asset compute项目后，在进行代码更改之前验证配置，以确保在`.env`文件中配置支持服务。

要为Asset compute项目启动Asset compute开发工具，请执行以下操作：

1. 在Asset compute项目根目录中打开命令行（在VS代码中，可以通过“终端”>“新终端”直接在IDE中打开此命令），然后执行命令：

   ```
   $ aio app run
   ```

1. 本地Asset compute开发工具将在默认Web浏览器中的&#x200B;__http://localhost:9000__&#x200B;打开。

   ![aio应用程序运行](assets/environment-variables/aio-app-run.png)

1. 当开发工具初始化时，请观看命令行输出和Web浏览器中的错误消息。
1. 要停止Asset compute开发工具，请点按执行`aio app run`的窗口中的`Ctrl-C`以终止该进程。

## 疑难解答

+ [由于缺少private.key，开发工具无法启动](../troubleshooting.md#missing-private-key)
