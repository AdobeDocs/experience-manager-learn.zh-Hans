---
title: 配置环境变量以实现Asset compute可扩展性
description: 环境变量在.env文件中进行维护以用于本地开发，并用于提供本地开发所需的Adobe I/O凭据和云存储凭据。
feature: Asset Compute Microservices
version: Cloud Service
doc-type: Tutorial
jira: KT-6270
thumbnail: KT-6270.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: c63c5c75-1deb-4c16-ba33-e2c338ef6251
duration: 144
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '587'
ht-degree: 0%

---

# 配置环境变量

![点环境文件](assets/environment-variables/dot-env-file.png)

在开始开发Asset compute工作人员之前，请确保为项目配置了Adobe I/O和云存储信息。 此信息存储在项目的 `.env`  仅用于本地开发，而不保存在Git中。 此 `.env` file提供了一种将键/值对公开到本地Asset compute本地开发环境的简便方法。 时间 [部署](../deploy/runtime.md) 将员工Asset compute到Adobe I/O Runtime， `.env` 文件未被使用，而是通过环境变量传入值的子集。 其他自定义参数和密钥可以存储在 `.env` 文件，例如第三方Web服务的开发凭据。

## 参考 `private.key`

![私钥](assets/environment-variables/private-key.png)

打开 `.env` 文件，取消注释 `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` 键，并提供文件系统上到 `private.key` 证书与添加到Adobe I/OApp Builder项目中的公共证书配对。

+ 如果您的密钥对是由Adobe I/O生成的，则会作为  `config.zip`.
+ 如果您向Adobe I/O提供了公钥，则您还应拥有匹配的私钥。
+ 如果您没有这些密钥对，则可以在底部生成新的密钥对或上传新的公钥：
  [https://console.adobe.com](https://console.adobe.io) >您的Asset computeApp Builder项目>工作区@开发>服务帐户(JWT)。

记住 `private.key` 不应将文件签入Git，因为它包含密钥，而是应将其存储在项目外部的安全位置。

例如，在macOS上，这可能如下所示：

```
...
ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH=/Users/example-user/credentials/aem-guides-wknd-asset-compute/private.key
...
```

## 配置云存储凭据

asset compute工人在当地的发展需要获得 [云存储](../set-up/accounts-and-services.md#cloud-storage). 用于本地开发的云存储凭据在中提供 `.env` 文件。

本教程倾向于使用Azure Blob Storage，但Amazon S3及其在中的相应密钥 `.env` 可以改用文件。

### 使用Azure Blob存储

取消注释并填充以下键 `.env` 文件，并使用Azure门户上提供的云存储的值填充这些文件。

![Azure Blob存储](./assets/environment-variables/azure-portal-credentials.png)

1. 的值 `AZURE_STORAGE_CONTAINER_NAME` 键
1. 的值 `AZURE_STORAGE_ACCOUNT` 键
1. 的值 `AZURE_STORAGE_KEY` 键

例如，它类似于（值仅供说明）：

```
...
AZURE_STORAGE_ACCOUNT=aemguideswkndassetcomput
AZURE_STORAGE_KEY=Va9CnisgdbdsNJEJBqXDyNbYppbGbZ2V...OUNY/eExll0vwoLsPt/OvbM+B7pkUdpEe7zJhg==
AZURE_STORAGE_CONTAINER_NAME=asset-compute
...
```

结果 `.env` 文件如下所示：

![Azure Blob存储凭据](assets/environment-variables/cloud-storage-credentials.png)

如果您未使用Microsoft Azure Blob Storage，请删除或保留这些注释项(使用前缀 `#`)。

### 使用Amazon S3云存储{#amazon-s3}

如果您使用的是Amazon S3云存储，请取消注释并填充 `.env` 文件。

例如，它类似于（值仅供说明）：

```
...
S3_BUCKET=aemguideswkndassetcompute
AWS_ACCESS_KEY_ID=KKIXZLZYNLXJLV24PLO6
AWS_SECRET_ACCESS_KEY=Ba898CnisgabdsNJEJBqCYyVrYttbGbZ2...OiNYExll0vwoLsPtOv
AWS_REGION=us-east-1
...
```

## 验证项目配置

在配置生成的Asset compute项目后，请在更改代码之前验证配置，以确保在中配置支持服务。 `.env` 文件。

要为Asset compute项目启动Asset compute开发工具，请执行以下操作：

1. 在Asset compute项目根中打开命令行（在VS代码中，这可以直接在IDE中通过“终端”>“新建终端”打开），然后执行命令：

   ```
   $ aio app run
   ```

1. 本地Asset compute开发工具将在您的默认Web浏览器中打开，网址为 __http://localhost:9000__.

   ![aio应用程序运行](assets/environment-variables/aio-app-run.png)

1. 在开发工具初始化时，请观察命令行输出和Web浏览器中的错误消息。
1. 要停止“Asset compute开发工具”，请点按 `Ctrl-C` 在窗口中执行 `aio app run` 以终止进程。

## 疑难解答

+ [由于缺少private.key，开发工具无法启动](../troubleshooting.md#missing-private-key)
