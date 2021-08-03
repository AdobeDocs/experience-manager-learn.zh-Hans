---
title: 在AEM Forms OSGi中配置Reader扩展
description: 将Reader扩展凭据添加到AEM Forms OSGi中的信任存储
feature: Reader扩展
feature-set: Reader Extensions
topics: development
audience: developer
doc-type: Tutorial
activity: implement
version: 6.4,6.5
topic: 管理
role: Admin
level: Beginner
source-git-commit: 55a6ff5d01898b994aee60f214126c5c18a06a5e
workflow-type: tm+mt
source-wordcount: '158'
ht-degree: 0%

---


# 添加Reader扩展凭据{#configuring-reader-extension-osgi}

DocAssurance服务可以对PDF文档应用使用权限。 要对PDF文档应用使用权限，请配置证书。

## 为fd-service用户创建密钥库

读取器扩展凭据与fd-service用户关联。 要向fd-service用户添加凭据，请执行以下步骤。 如果已为fd-service用户创建密钥库，请跳过此部分

* 以管理员身份登录AEM创作实例
* 转到Tools-Security-Users
* 向下滚动用户列表，直到找到fd-service用户帐户
* 单击fd-service用户
* 单击KeyStore选项卡
* 单击Create KeyStore
* 设置KeyStore访问密码并保存您的设置以创建KeyStore密码

### 将凭据添加到fd-service用户密钥库

请观看视频，将凭据添加到fd-service用户

>[!VIDEO](https://video.tv.adobe.com/v/335849?quality=9&learn=on)











