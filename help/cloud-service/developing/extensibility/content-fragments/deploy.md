---
title: 部署AEM内容片段控制台扩展
description: 了解如何部署AEM内容片段控制台扩展。
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
kt: 11603
last-substantial-update: 2022-12-01T00:00:00Z
source-git-commit: a7b32982b547eb292384d2ebde80ba745091702a
workflow-type: tm+mt
source-wordcount: '802'
ht-degree: 0%

---


# 部署扩展

要在AEMas a Cloud Service环境中使用，必须部署和批准扩展App Builder应用程序。

部署扩展App Builder应用程序时，应注意以下几个注意事项：

+ 扩展将部署到Adobe Developer控制台项目工作区。 默认工作区包括：
   + __生产__ 工作区包含可在所有AEMas a Cloud Service中使用的扩展部署。
   + __阶段__ 工作区充当开发人员工作区。 部署到Stage工作区的扩展在AEMas a Cloud Service中不可用。
Adobe Developer控制台工作区与AEMas a Cloud Service环境类型没有任何直接关联。
+ 部署到生产工作区的扩展将显示在Adobe组织中存在该扩展的所有AEMas a Cloud Service环境中。
扩展不能通过添加 [检查AEMas a Cloud Service主机名的条件逻辑](https://developer.adobe.com/uix/docs/guides/publication/#enabling-extension-only-on-specific-aem-environments).
+ 多个扩展可用于AEMas a Cloud Service。 Adobe建议使用每个扩展App Builder应用程序来解决单个业务目标。 也就是说，单个扩展App Builder应用程序可以实施多个支持常见业务目标的扩展点。

## 初始部署

要使扩展在AEMas a Cloud Service环境中可用，必须将其部署到Adobe Developer控制台。

部署过程分为两个逻辑步骤：

1. 由开发人员将扩展App Builder应用程序部署到Adobe Developer Console。
1. 部署管理员或业务所有者批准扩展。

### 部署扩展App Builder应用程序

将扩展部署到生产工作区。 部署到生产工作区的扩展会自动添加到Adobe组织中部署到该扩展的所有AEMas a Cloud Service创作服务。

1. 在更新的扩展App Builder应用程序的根目录下打开命令行。
1. 确保生产工作区处于活动状态

   ```shell
   $ aio app use -w Production
   ```

   合并对 `.env` 和 `.aio`.

1. 部署更新的扩展App Builder应用程序。

   ```shell
   $ aio app deploy
   ```

#### 请求部署批准

![提交扩展以供审批](./assets/deploy/submit-for-approval.png){align="center"}

1. 登录到 [Adobe Developer控制台](https://developer.adobe.com)
1. 选择 __控制台__
1. 导航到 __项目__
1. 选择与该扩展关联的项目
1. 选择 __生产__ 工作区
1. 选择 __提交以供审批__
1. 填写并提交表单，并根据需要更新字段。

+ 需要图标。 如果您没有图标，则可以使用 [此图标](./assets/deploy/icon.png).

### 批准部署请求

![扩展批准](./assets/deploy/adobe-exchange.png){align="center"}

1. 登录到 [Adobe交换](https://exchange.adobe.com/)
1. 导航到 __管理__ > __应用程序待审核__
1. __审阅__ 扩展App Builder应用程序
1. 如果扩展更改是可接受的 __接受__ 评论。 这会立即在Adobe组织内的所有AEMas a Cloud Service创作服务上插入扩展。

扩展请求获得批准后，该扩展会立即在AEMas a Cloud Service创作服务中变为活动状态。

## 更新扩展

更新和扩展App Builder应用程序的过程与 [初始部署](#initial-deployment)，并且必须首先撤消现有扩展部署。

### 撤消扩展

要部署扩展的新版本，必须先吊销（或删除）该扩展。 扩展处于“已吊销”状态，但在AEM控制台中不可用。

1. 登录到 [Adobe交换](https://exchange.adobe.com/)
1. 导航到 __管理__ > __应用程序生成器应用程序__
1. __撤销__ 要更新的扩展

### 部署扩展

将扩展部署到生产工作区。 部署到生产工作区的扩展会自动添加到Adobe组织中部署到该扩展的所有AEMas a Cloud Service创作服务。

1. 在更新的扩展App Builder应用程序的根目录下打开命令行。
1. 确保生产工作区处于活动状态

   ```shell
   $ aio app use -w Production
   ```

   合并对 `.env` 和 `.aio`.

1. 部署更新的扩展App Builder应用程序。

   ```shell
   $ aio app deploy
   ```

#### 请求部署批准

![提交扩展以供审批](./assets/deploy/submit-for-approval.png){align="center"}

1. 登录到 [Adobe Developer控制台](https://developer.adobe.com)
1. 选择 __控制台__
1. 导航到 __项目__
1. 选择与该扩展关联的项目
1. 选择 __生产__ 工作区
1. 选择 __提交以供审批__
1. 填写并提交表单，并根据需要更新字段。

#### 批准部署请求

![扩展批准](./assets/deploy/adobe-exchange.png){align="center"}

1. 登录到 [Adobe交换](https://exchange.adobe.com/)
1. 导航到 __管理__ > __应用程序待审核__
1. __审阅__ 扩展App Builder应用程序
1. 如果扩展更改是可接受的 __接受__ 评论。 这会立即在Adobe组织内的所有AEMas a Cloud Service创作服务上插入扩展。

扩展请求获得批准后，该扩展会立即在AEMas a Cloud Service创作服务中变为活动状态。

## 删除扩展

![删除扩展](./assets/deploy/revoke.png)

要删除某个扩展，请从Exchange中撤消（或删除）该Adobe。 该扩展被撤消后，将从所有AEMas a Cloud Service创作服务中删除。

1. 登录到 [Adobe交换](https://exchange.adobe.com/)
1. 导航到 __管理__ > __应用程序生成器应用程序__
1. __撤销__ 要删除的扩展
