---
source-git-commit: 10032375a23ba99f674fb59a6f6e48bf93908421
workflow-type: tm+mt
source-wordcount: '174'
ht-degree: 0%

---



# AEM Headless部署

AEM无头客户端部署采用多种形式；AEM托管的SPA、外部SPA或网站、移动设备应用程序，甚至服务器到服务器进程。

根据客户端及其部署方式，AEM Headless部署有不同的注意事项。

## AEM服务架构

在探索部署注意事项之前，必须了解AEM逻辑架构以及AEMas a Cloud Service服务层的分离和角色。 AEMas a Cloud Service由两个逻辑服务组成：

+ __AEM作者__ 是团队创建、协作和发布内容片段（和其他资产）的服务
+ __AEM发布__ 是复制已发布内容片段（和其他资产）以用于一般使用情况的服务

以生产容量运行的AEM无头客户端通常与AEM发布交互，该发布包含已批准的已发布内容。 与AEM作者交互的客户端需要特别注意，因为AEM作者默认是安全的，需要对所有请求进行授权，并且可能包含不完整或未批准的内容。

## 无外设客户端部署

+ [单页应用程序](./spa.md)
+ [Web组件/JS](./web-component.md)
+ [移动设备应用程序](./mobile.md)
+ [服务器到服务器](./server-to-server.md)

