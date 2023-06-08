---
title: 部署AEM UI扩展
description: 了解如何部署AEM UI扩展。
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
kt: 11603
last-substantial-update: 2023-06-02T00:00:00Z
exl-id: 2e37165d-c003-4206-8133-54e37ca35b8e
source-git-commit: 6b5c755bd8fe6bbf497895453b95eb236f69d5f6
workflow-type: tm+mt
source-wordcount: '777'
ht-degree: 1%

---

# 部署扩展

要在AEMas a Cloud Service环境中使用，必须部署和批准扩展App Builder应用程序。

部署扩展App Builder应用程序时，需要注意以下几个注意事项：

+ 扩展将部署到Adobe Developer Console项目工作区。 默认工作区为：
   + __生产__ 工作区包含可在所有AEMas a Cloud Service中使用的扩展部署。
   + __暂存__ 工作区充当开发人员工作区。 部署到暂存工作区的扩展在AEMas a Cloud Service中不可用。
Adobe Developer Console工作区与AEMas a Cloud Service的环境类型没有任何直接关联。
+ 部署到生产工作区的扩展将显示在Adobe组织中该扩展存在的所有AEMas a Cloud Service环境中。
不能将扩展限制为通过添加来注册的环境 [检查AEMas a Cloud Service主机名的条件逻辑](https://developer.adobe.com/uix/docs/guides/publication/#enabling-extension-only-on-specific-aem-environments).
+ 可在AEMas a Cloud Service上使用多个扩展。 Adobe建议使用每个扩展App Builder应用程序来解决单个业务目标。 也就是说，一个扩展App Builder应用程序可以实施多个扩展点，以支持共同的业务目标。

## 初始部署

要使扩展在AEMas a Cloud Service的环境中可用，必须将其部署到Adobe Developer控制台。

部署过程分为两个逻辑步骤：

1. 开发人员将扩展App Builder应用程序部署到Adobe Developer控制台。
1. 由部署经理或业务负责人审批扩展。

### 部署扩展

将扩展部署到生产工作区。 部署到生产工作区的扩展会自动添加到部署该扩展的Adobe组织中的所有AEMas a Cloud Service创作服务。

1. 打开指向已更新扩展App Builder应用程序的根目录的命令行。
1. 确保生产工作区处于活动状态

   ```shell
   $ aio app use -w Production
   ```

   将任何更改合并到 `.env` 和 `.aio`.

1. 部署更新的扩展App Builder应用程序。

   ```shell
   $ aio app deploy
   ```

#### 请求部署审批

![提交扩展以供审批](./assets/deploy/submit-for-approval.png){align="center"}

1. 登录 [Adobe Developer控制台](https://developer.adobe.com)
1. 选择 __控制台__
1. 导航到 __项目__
1. 选择与扩展关联的项目
1. 选择 __生产__ 工作区
1. 选择 __提交以供审批__
1. 填写并提交表单，根据需要更新字段。

### 部署审批

![扩展审批](./assets/deploy/adobe-exchange.png){align="center"}

1. 登录 [Adobe交换](https://exchange.adobe.com/)
1. 导航到 __管理__ > __等待审阅的应用程序__
1. __审核__ 扩展App Builder应用程序
1. 如果扩展更改可以接受 __接受__ 评论。 这会立即在Adobe组织内的所有AEMas a Cloud Service创作服务上注入该扩展。

扩展请求获得批准后，该扩展将立即在AEMas a Cloud Service创作服务中处于活动状态。

## 更新扩展

更新和扩展App Builder应用程序的过程与相同 [初始部署](#initial-deployment)，但现有扩展部署必须先撤销。

### 撤销扩展

要部署扩展的新版本，必须首先撤销（或删除）该扩展。 虽然扩展已撤销，但在AEM控制台中不可用。

1. 登录 [Adobe交换](https://exchange.adobe.com/)
1. 导航到 __管理__ > __App Builder应用程序__
1. __撤销__ 要更新的扩展

### 部署扩展

将扩展部署到生产工作区。 部署到生产工作区的扩展会自动添加到部署该扩展的Adobe组织中的所有AEMas a Cloud Service创作服务。

1. 打开指向已更新扩展App Builder应用程序的根目录的命令行。
1. 确保生产工作区处于活动状态

   ```shell
   $ aio app use -w Production
   ```

   将任何更改合并到 `.env` 和 `.aio`.

1. 部署更新的扩展App Builder应用程序。

   ```shell
   $ aio app deploy
   ```

#### 请求部署审批

![提交扩展以供审批](./assets/deploy/submit-for-approval.png){align="center"}

1. 登录 [Adobe Developer控制台](https://developer.adobe.com)
1. 选择 __控制台__
1. 导航到 __项目__
1. 选择与扩展关联的项目
1. 选择 __生产__ 工作区
1. 选择 __提交以供审批__
1. 填写并提交表单，根据需要更新字段。

#### 批准部署请求

![扩展审批](./assets/deploy/adobe-exchange.png){align="center"}

1. 登录 [Adobe交换](https://exchange.adobe.com/)
1. 导航到 __管理__ > __等待审阅的应用程序__
1. __审核__ 扩展App Builder应用程序
1. 如果扩展更改可以接受 __接受__ 评论。 这会立即在Adobe组织内的所有AEMas a Cloud Service创作服务上注入该扩展。

扩展请求获得批准后，该扩展将立即在AEMas a Cloud Service创作服务中处于活动状态。

## 删除扩展

![删除扩展](./assets/deploy/revoke.png)

要删除扩展，请从Adobe交换中撤消（或删除）该扩展。 撤销扩展后，该扩展将从所有AEMas a Cloud Service创作服务中移除。

1. 登录 [Adobe交换](https://exchange.adobe.com/)
1. 导航到 __管理__ > __App Builder应用程序__
1. __撤销__ 要删除的扩展
