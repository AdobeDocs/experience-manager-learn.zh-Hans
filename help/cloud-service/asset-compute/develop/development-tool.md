---
title: asset compute开发工具
description: asset compute开发工具是一个本地Web工具，它允许开发人员在AEM SDK上下文之外针对Adobe I/O Runtime中的Asset compute资源在本地配置和执行资产计算机工作程序。
feature: Asset Compute Microservices
version: Cloud Service
doc-type: Tutorial
jira: KT-6283
thumbnail: 40241.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: cbe08570-e353-4daf-94d1-a91a8d63406d
duration: 171
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '534'
ht-degree: 0%

---

# asset compute开发工具

asset compute开发工具是一个本地Web工具，它允许开发人员在AEM SDK上下文之外针对Adobe I/O Runtime中的Asset compute资源在本地配置和执行资产计算机工作程序。

## 运行Asset compute开发工具

asset compute开发工具可以通过终端命令从Asset compute项目的根目录运行：

```
$ aio app run
```

这将在&#x200B;__http://localhost:9000__&#x200B;处启动开发工具，并在浏览器窗口中自动打开该工具。 为了运行开发工具，[必须通过查询参数](#troubleshooting__devtooltoken)提供自动生成的有效devToolToken。

## 了解Asset compute开发工具界面{#interface}

![Asset compute开发工具](./assets/development-tool/asset-compute-dev-tool.png)

1. __Source文件：__&#x200B;源文件选择用于：
   + 已选择充当传递给Asset compute辅助进程的`source`二进制文件的资源二进制文件
   + 上载源文件
1. __Asset compute配置文件定义：__&#x200B;定义要运行的Asset compute工作进程，包括参数：包括工作进程的URL端点、生成的演绎版名称和任何参数
1. __运行：__“运行”按钮执行Asset compute配置配置文件编辑器中定义的Asset compute配置文件
1. __中止：__&#x200B;中止按钮取消通过点击“运行”按钮启动的执行
1. __请求/响应：__&#x200B;提供对Adobe I/O Runtime中运行的Asset compute工作进程的HTTP请求和响应。 这有助于调试
1. __Asset compute日志：__&#x200B;描述Activation Worker执行的日志以及任何错误。 此信息也在`aio app run`标准输出中可用
1. __演绎版：__&#x200B;显示执行Asset compute辅助进程生成的所有演绎版
1. __devToolToken查询参数：__ Asset compute开发工具令牌需要存在有效的`devToolToken`查询参数。 每次生成新的开发工具时，将自动生成此令牌

### 运行自定义工作程序

>[!VIDEO](https://video.tv.adobe.com/v/40241?quality=12&learn=on)

_在开发工具中运行Asset compute工作的点进（无音频）_

1. 确保使用`aio app run`命令从项目根目录启动Asset compute开发工具。
1. 在Asset compute开发工具中，上传或选择[示例图像文件](../assets/samples/sample-file.jpg)
   + 确保在&#x200B;__Source文件__&#x200B;下拉菜单中选择该文件
1. 查看&#x200B;__Asset compute配置文件定义__&#x200B;文本区域
   + `worker`键定义了已部署Asset compute工作进程的URL
   + `name`键定义了要生成的演绎版的名称
   + 其他键/值可在此JSON对象中提供，并可在`rendition.instructions`对象下的工作程序中提供
      + 可以选择添加`size`、`contrast`和`brightness`的值：

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
1. __节目部分__&#x200B;将填入节目占位符
1. 工作进程完成后，格式副本占位符将显示生成的格式副本

在开发工具运行时对工作代码进行代码更改将“热部署”这些更改。 “热部署”需要几秒钟的时间，因此允许部署先完成，然后再从开发工具重新运行工作程序。

## 疑难解答

+ [YAML缩进不正确](../troubleshooting.md#incorrect-yaml-indentation)
+ [memorySize限制设置过低](../troubleshooting.md#memorysize-limit-is-set-too-low)
+ [由于缺少private.key，开发工具无法启动](../troubleshooting.md#missing-private-key)
+ [Source文件下拉列表不正确](../troubleshooting.md#source-files-dropdown-incorrect)
+ [devToolToken查询参数缺失或无效](../troubleshooting.md#missing-or-invalid-devtooltoken-query-parameter)
+ [无法删除源文件](../troubleshooting.md#unable-to-remove-source-files)
+ [节目返回部分绘制/损坏](../troubleshooting.md#rendition-returned-partially-drawn-or-corrupt)
