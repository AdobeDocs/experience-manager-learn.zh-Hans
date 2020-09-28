---
title: 资产计算开发工具
description: 资产计算开发工具是本地Web工具，它允许开发人员在AEM SDK上下文之外针对Adobe I/O Runtime的资产计算资源在本地配置和执行资产计算机工作程序。
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6283
thumbnail: 40241.jpg
translation-type: tm+mt
source-git-commit: a71c61304bbc9d54490086b3313c823225fbe2e0
workflow-type: tm+mt
source-wordcount: '700'
ht-degree: 0%

---


# 资产计算开发工具

资产计算开发工具是本地Web工具，它允许开发人员在AEM SDK上下文之外针对Adobe I/O Runtime的资产计算资源在本地配置和执行资产计算机工作程序。

## 运行资产计算开发工具

资产计算开发工具可以通过终端命令从资产计算应用程序项目的根目录运行：

```
$ aio app run
```

这将在http://localhost:9000开始开发 __工具__，并在浏览器窗口中自动打开它。 要运行开发工具，必 [须通过查询参数提供有效、自动生成的devToolToken](#troubleshooting__devtooltoken)。

## 了解资产计算开发工具界面{#interface}

![资产计算开发工具](./assets/development-tool/asset-compute-dev-tool.png)

1. __源文件：__ 源文件选择用于：
   + 已选择资产二进制文件，该二进制文 `source` 件将作为传递给资产计算工作者的二进制文件
   + 上传源文件
1. __资产计算用户档案定义：__ 定义要运行的资产计算工作器，包括参数：包括工作者的URL终点、生成的再现名称和任何参数
1. __运行：__ “运行”按钮执行资产计算用户档案(在资产计算配置用户档案编辑器中定义)
1. __中止：__ “中止”按钮取消通过点击“运行”按钮启动的执行
1. __请求／响应：__ 提供HTTP请求以及对／来自在Adobe运行时中运行的资产计算应用程序的响应。 这有助于调试
1. __激活日志：__ 描述资产计算应用程序执行情况的日志以及任何错误。 此信息也可在标准 `aio app run` 版中获取
1. __再现：__ 显示由执行资产计算应用程序生成的所有演绎版
1. __devToolToken查询参数：__ 资产计算开发工具令牌需要有 `devToolToken` 效的查询参数。 每次衍生新开发工具时，都会自动生成此令牌

### 运行自定义工作器

>[!VIDEO](https://video.tv.adobe.com/v/40241?quality=12&learn=on)
_在开发工具中运行资产计算工作的点进（无音频）_

1. 确保使用命令从项目根目录启动资产计算开发 `aio app run` 工具。
1. 在资产计算开发工具中，上传或选择一个 [示例图像文件](../assets/samples/sample-file.jpg)
   + 确保在“源文件”下拉框中选 __择该文件__
1. 复查“资 __产计算用户档案__ ”定义文本区域
   + 该 `worker` 键定义已部署资产计算工作者的URL
   + 键 `name` 定义要生成的演绎版的名称
   + 此JSON对象中可提供其他键／值，该对象下的工作者中也提供这 `rendition.instructions` 些键
      + （可选）添加 `size`以 `contrast` 及 `brightness`:

         ```json
         {
             "renditions": [
                 {
                     "worker": "...",
                     "name": "rendition.png",
                     "size":"800",
                     "contrast": "0.30",
                     "brightness": "-0.15"
                 }
             ]
         }
         ```

1. Tap the __Run__ button
1. 演绎 __版部分将__ 填充演绎版占位符
1. 工作人员完成后，演绎版占位符将显示生成的演绎版

在开发工具运行时对工作代码进行代码更改将“热部署”更改。 “热部署”需要几秒钟，因此在从开发工具重新运行该工作人员之前，请允许完成部署。

## 疑难解答

### 源文件下拉框不正确{#troubleshooting__dev-tool-application-cache}

资产计算开发工具可能会进入其提取旧数据的状态，在显示错误项的源文件下拉 __列表中__ ，这一点最为明显。

+ __错误：__ 源文件下拉列表显示不正确的项目。
+ __原因：__ 过时的缓存浏览器状态导致
+ __解决方案：__ 在您的浏览器中，完全清除浏览器选项卡的应用程序状态、浏览器缓存、本地存储和服务工作者。

### 缺少devToolToken查询参数{#troubleshooting__devtooltoken}

+ __错误：__ 资产计算开发工具中的“未授权”通知
+ __原因：__`devToolToken` 缺失或无效
+ __解决方案：__ 关闭“资产计算开发工具”浏览器窗口，终止通过命令启动的任何正在运行的 `aio app run` 开发工具进程，并（使用）重新开始 `aio app run`开发工具。

### 无法删除源文件{#troubleshooting__remove-source-files}

+ __错误：__ 无法从开发工具UI中删除添加的源文件
+ __原因：__ 此功能尚未实现
+ __解决方案：__ 使用中定义的凭据登录到您的云存储提供 `.env`商。 找到开发工具使用的容器(也在中指定 `.env`)，导航到源文 __件夹__ ，并删除任何源图像。 如果删除的源文件继续显示在下 [拉列表中](#troubleshooting__dev-tool-application-cache) ，您可能需要执行源文件下拉列表中概述的步骤不正确，因为这些源文件可能在开发工具应用程序状态下进行本地缓存。

   ![Microsoft Azure Blob存储](./assets/development-tool/troubleshooting__remove-source-files.png)
