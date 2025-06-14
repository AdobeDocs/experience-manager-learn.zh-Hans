---
title: 为 Edge Delivery Services 设置本地开发环境
description: 如何为 Edge Delivery Services 设置本地开发环境。
version: Experience Manager 6.5, Experience Manager as a Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Technical Video
last-substantial-update: 2023-11-15T00:00:00Z
jira: KT-14483
thumbnail: 3425717.jpeg
duration: 169
exl-id: 0f3e50f0-88d8-46be-be8b-0f547c3633a6
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '87'
ht-degree: 100%

---

# 设置本地开发环境

如何为 Edge Delivery Services 设置本地开发环境。

>[!VIDEO](https://video.tv.adobe.com/v/3434741/?learn=on&captions=chi_hans)


## 视频中概述的步骤

1. 安装 AEM CLI

   ```
   $ sudo npm install -g @adobe/aem-cli
   ```

1. 将目录更改为您的项目目录，该目录是由 [AEM 样板](https://github.com/adobe/aem-boilerplate)模板制作的 git 存储库。

   ```
   $ git clone git@github.com:my-org/my-project.git
   $ cd my-project
   ```

1. 运行 AEM CLI 以启动本地 AEM 实例。

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

1. 打开 http://localhost:3000/ 您的 Web 浏览器，以查看您的 AEM 网站。
