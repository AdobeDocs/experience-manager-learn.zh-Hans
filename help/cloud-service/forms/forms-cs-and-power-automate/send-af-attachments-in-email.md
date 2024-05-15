---
title: 通过电子邮件发送表单附件
description: 使用Power Automate工作流在电子邮件中提取和发送提交的表单附件
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-11077
exl-id: 1be90d9b-3669-44a0-84fb-cbdec44074d8
duration: 391
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '72'
ht-degree: 0%

---

# 从提交的表单数据中提取表单附件

在power automate工作流中提取表单附件并通过电子邮件发送附件。
以下视频介绍根据提交的数据形成附件所需的步骤。
>[!VIDEO](https://video.tv.adobe.com/v/3409017?quality=12&learn=on)

以下是您需要在“解析JSON架构”步骤中使用的附件对象架构

```json
{
    "type": "object",
    "properties": {
        "filename": {
            "type": "string"
        },
        "data": {
            "type": "string"
        },
        "contentType": {
            "type": "string"
        },
        "size": {
            "type": "integer"
        }
    }
}
```
