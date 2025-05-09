---
title: 通过电子邮件发送表单附件
description: 使用Power Automate工作流在电子邮件中提取和发送提交的表单附件
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-11077
exl-id: 1be90d9b-3669-44a0-84fb-cbdec44074d8
duration: 391
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '78'
ht-degree: 0%

---

# 从提交的表单数据中提取表单附件

在power automate工作流中提取表单附件并通过电子邮件发送附件。
以下视频介绍根据提交的数据形成附件所需的步骤。
>[!VIDEO](https://video.tv.adobe.com/v/3413026?quality=12&learn=on&captions=chi_hans)

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
