---
title: asset compute开发工具
description: asset compute开发工具是一个本地Web工具，允许开发人员在AEM SDK上下文之外针对Adobe I/O Runtime中的Asset compute资源在本地配置和执行资产计算机工作程序。
feature: Asset Compute Microservices
topics: renditions, development
version: Cloud Service
activity: develop
audience: developer
doc-type: tutorial
kt: 6283
thumbnail: 40241.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: cbe08570-e353-4daf-94d1-a91a8d63406d
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '536'
ht-degree: 0%

---

# asset compute开发工具

asset compute开发工具是一个本地Web工具，允许开发人员在AEM SDK上下文之外针对Adobe I/O Runtime中的Asset compute资源在本地配置和执行资产计算机工作程序。

## 运行Asset compute开发工具

asset compute开发工具可通过“终端”命令从Asset compute项目的根中运行：

```
$ aio app run
```

这将启动位于&#x200B;__http://localhost:9000__&#x200B;的开发工具，并在浏览器窗口中自动将其打开。 要运行开发工具，必须通过查询参数](#troubleshooting__devtooltoken)提供有效且自动生成的devToolToken。[

## 了解Asset compute开发工具界面{#interface}

![asset compute开发工具](./assets/development-tool/asset-compute-dev-tool.png)

1. __源文件：__ 源文件选择用于：
   + 已选择将作为传递给Asset compute工作程序的`source`二进制文件的资产二进制文件
   + 上载源文件
1. __asset compute配置文件定义：__ 定义要运行的Asset compute工作程序，包括参数：包括工作者的URL端点、生成的演绎版名称以及任何参数
1. __运行：__ “运行”按钮执行在Asset compute配置文件编辑器中定义的Asset compute配置文件
1. __中止：__ 中止按钮取消点按运行按钮时启动的执行
1. __请求/响应：__ 提供HTTP请求和对/来自在Adobe I/O Runtime中运行的Asset compute工作程序的响应。这有助于进行调试
1. __激活日志：__ 描述Asset compute工作者执行的日志以及任何错误。`aio app run`标准输出中也提供此信息
1. __演绎版：__ 显示由执行Asset compute工作程序生成的所有演绎版
1. __devToolToken查询参数：__ Asset compute开发工具令牌要求存在有 `devToolToken` 效的查询参数。每次生成新开发工具时都会自动生成此令牌

### 运行自定义工作程序

>[!VIDEO](https://video.tv.adobe.com/v/40241?quality=12&learn=on)

_在开发工具中运行Asset compute工作的点进（无音频）_

1. 确保使用`aio app run`命令从项目根目录启动Asset compute开发工具。
1. 在Asset compute开发工具中，上载或选择[示例图像文件](../assets/samples/sample-file.jpg)
   + 确保在&#x200B;__源文件__&#x200B;下拉列表中选择了文件
1. 查看&#x200B;__Asset compute配置文件定义__&#x200B;文本区域
   + `worker`键值定义到已部署的Asset compute工作程序的URL
   + `name`键定义要生成的演绎版的名称
   + 其他键/值可在此JSON对象中提供，并将在`rendition.instructions`对象下的工作程序中提供
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
1. __演绎版部分__&#x200B;将使用演绎版占位符填充
1. 工作程序完成后，演绎版占位符将显示生成的演绎版

在开发工具运行时对工作代码进行代码更改将“热部署”更改。 “热部署”需要几秒钟的时间，因此在从开发工具重新运行工作程序之前，需要先完成部署。

## 疑难解答

+ [YAML缩进不正确](../troubleshooting.md#incorrect-yaml-indentation)
+ [memorySize限制设置过低](../troubleshooting.md#memorysize-limit-is-set-too-low)
+ [由于缺少private.key，开发工具无法启动](../troubleshooting.md#missing-private-key)
+ [源文件下拉列表不正确](../troubleshooting.md#source-files-dropdown-incorrect)
+ [devToolToken查询参数缺失或无效](../troubleshooting.md#missing-or-invalid-devtooltoken-query-parameter)
+ [无法删除源文件](../troubleshooting.md#unable-to-remove-source-files)
+ [返回部分绘制/损坏的演绎版](../troubleshooting.md#rendition-returned-partially-drawn-or-corrupt)
