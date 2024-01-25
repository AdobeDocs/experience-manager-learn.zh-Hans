---
title: 为Edge Delivery Services设置本地开发环境
description: 如何为Edge Delivery Services设置本地开发环境。
version: 6.5, Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Technical Video
last-substantial-update: 2023-11-15T00:00:00Z
jira: KT-14483
thumbnail: 3425717.jpeg
duration: 181
exl-id: 0f3e50f0-88d8-46be-be8b-0f547c3633a6
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '87'
ht-degree: 0%

---

# 设置本地开发环境

如何为Edge Delivery Services设置本地开发环境。

>[!VIDEO](https://video.tv.adobe.com/v/3425717/?learn=on)


## 视频中概述的步骤

1. 安装AEM CLI

   ```
   $ sudo npm install -g @adobe/aem-cli
   ```

1. 将目录更改为项目目录，该项目目录是从中创建的Git存储库 [AEM样板](https://github.com/adobe/aem-boilerplate) 模板。

   ```
   $ git clone git@github.com:my-org/my-project.git
   $ cd my-project
   ```

1. 运行AEM CLI以启动本地AEM实例。

   ```
   $ pwd
     /Users/my-user/my-project
   
   $ aem up
       ___    ________  ___                          __      __ 
      /   |  / ____/  |/  /  _____(_)___ ___  __  __/ /___ _/ /_____  _____
     / /| | / __/ / /|_/ /  / ___/ / __ `__ \/ / / / / __ `/ __/ __ \/ ___/
    / ___ |/ /___/ /  / /  (__  ) / / / / / / /_/ / / /_/ / /_/ /_/ / /
   /_/  |_/_____/_/  /_/  /____/_/_/ /_/ /_/\__,_/_/\__,_/\__/\____/_/
   
   info: Starting AEM dev server vx.x.x
   info: Local AEM dev server up and running: http://localhost:3000/
   info: Enabled reverse proxy to https://main--my-project--my-org.hlx.page
   opening default browser: http://localhost:3000/
   ```

1. 打开http://localhost:3000/您的Web浏览器，查看您的AEM网站。
