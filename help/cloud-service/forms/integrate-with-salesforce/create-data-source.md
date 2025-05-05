---
title: 创建云服务配置
description: 使用OAuth凭据创建数据源以连接到Salesforce
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
topic: Development
feature: Adaptive Forms, Integrations
jira: KT-7148
thumbnail: 331755.jpg
exl-id: e2d56e91-c13e-4787-a97f-255938b5d290
duration: 173
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '75'
ht-degree: 16%

---

# 创建数据Source

使用上一步中创建的swagger文件创建一个REST支持的数据源。

>[!VIDEO](https://video.tv.adobe.com/v/3411548?quality=12&learn=on&captions=chi_hans)

| 设置 | 价值 |
|---------------------|-----------------------------------------------------------------|
| OAuth URL | https://login.salesforce.com/services/oauth2/authorize |
| 授权范围 | api chatter_api完整id openid refresh_token visualforce web |
| 刷新令牌 URL | https://newfocus-dev-ed.my.salesforce.com/services/oauth2/token |
| 访问令牌 URL | https://newfocus-dev-ed.my.salesforce.com/services/oauth2/token |


**必须更改刷新和访问令牌URL的域名以匹配您的Salesforce帐户设置**