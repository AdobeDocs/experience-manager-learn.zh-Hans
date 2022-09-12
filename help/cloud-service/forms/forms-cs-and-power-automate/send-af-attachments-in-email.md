---
title: 通过电子邮件发送表单附件
description: 使用电源自动化工作流在电子邮件中提取和发送已提交的表单附件
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
kt: 11077
source-git-commit: aea43a705b3959f8be26238d32b816b3953e153e
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---

# 从提交的表单数据中提取表单附件

提取表单附件并在电子邮件中以电源自动化工作流发送附件。
以下视频介绍从提交的数据构建附件所需的步骤。
>[!VIDEO](https://video.tv.adobe.com/v/3409017/?quality=12&learn=on)

以下是解析JSON架构步骤中需要使用的附件对象架构

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
