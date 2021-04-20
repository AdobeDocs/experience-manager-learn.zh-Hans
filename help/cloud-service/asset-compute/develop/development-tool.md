---
title: asset compute开发工具
description: asset compute开发工具是一个本地Web工具，它允许开发人员在AEM SDK上下文外部针对Adobe I/O Runtime中的Asset compute资源在本地配置和执行资产计算机工作程序。
feature: Asset Compute Microservices
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6283
thumbnail: 40241.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '544'
ht-degree: 0%

---


# asset compute开发工具

asset compute开发工具是一个本地Web工具，它允许开发人员在AEM SDK上下文外部针对Adobe I/O Runtime中的Asset compute资源在本地配置和执行资产计算机工作程序。

## 运行Asset compute开发工具

asset compute开发工具可以通过终端命令从Asset compute项目的根运行：

```
$ aio app run
```

这将开始位于&#x200B;__http://localhost:9000__&#x200B;的开发工具，并在浏览器窗口中自动打开它。 要运行开发工具，必须通过查询参数](#troubleshooting__devtooltoken)提供有效、自动生成的devToolToken。[

## 了解Asset compute开发工具界面{#interface}

![asset compute开发工具](./assets/development-tool/asset-compute-dev-tool.png)

1. __源文件：__ 源文件选择用于：
   + 已选择将作为传递给Asset computeWorker的`source`二进制文件的资产二进制文件
   + 上传源文件
1. __asset compute用户档案定义：定__ 义要运行的Asset computeworker，包括参数：包括工作人员的URL端点、生成的再现名称和任何参数
1. __运行：“__ 运行”按钮执行Asset compute用户档案(在Asset compute配置用户档案编辑器中定义)
1. __中止：__ 中止按钮取消通过点击运行按钮启动的执行
1. __请求/响应：__ 提供HTTP请求以及对Adobe I/O Runtime中运行的Asset computeworker的响应。这对调试很有帮助
1. __激活日__ 志：描述Asset computeWorker执行情况的日志，以及任何错误。`aio app run`标准版中也提供此信息
1. __演绎版：__ 显示由Asset computeworker执行生成的所有演绎版
1. __DevToolToken查询参数：__ Asset compute开发工具令牌需要有 `devToolToken` 效的查询参数。每次衍生新开发工具时，会自动生成此令牌

### 运行自定义Worker

>[!VIDEO](https://video.tv.adobe.com/v/40241?quality=12&learn=on)

_在开发工具中运行Asset compute作品的点进（无音频）_

1. 确保使用`aio app run`命令从您的项目根启动Asset compute开发工具。
1. 在Asset compute开发工具中，上传或选择[示例图像文件](../assets/samples/sample-file.jpg)
   + 确保在&#x200B;__源文件__&#x200B;下拉列表中选择了该文件
1. 查看&#x200B;__Asset compute用户档案定义__&#x200B;文本区域
   + `worker`键定义已部署的Asset computeWorker的URL
   + `name`键定义要生成的再现的名称
   + 此JSON对象中可以提供其他键/值，并可在`rendition.instructions`对象下的worker中提供
      + （可选）添加`size`、`contrast`和`brightness`的值：

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

1. 点按&#x200B;__运行__&#x200B;按钮
1. __演绎版部分__&#x200B;将填充演绎版占位符
1. 工作器完成后，再现占位符将显示生成的再现

在开发工具运行时对工作代码进行代码更改将“热部署”更改。 “热部署”需要几秒钟的时间，因此在从开发工具重新运行worker之前，请允许完成部署。

## 疑难解答

+ [YAML缩进不正确](../troubleshooting.md#incorrect-yaml-indentation)
+ [memorySize限制设置得太低](../troubleshooting.md#memorysize-limit-is-set-too-low)
+ [由于缺少private.key，开发工具无法开始](../troubleshooting.md#missing-private-key)
+ [源文件下拉列表不正确](../troubleshooting.md#source-files-dropdown-incorrect)
+ [缺少或无效devToolToken查询参数](../troubleshooting.md#missing-or-invalid-devtooltoken-query-parameter)
+ [无法删除源文件](../troubleshooting.md#unable-to-remove-source-files)
+ [再现返回部分绘制/损坏](../troubleshooting.md#rendition-returned-partially-drawn-or-corrupt)
