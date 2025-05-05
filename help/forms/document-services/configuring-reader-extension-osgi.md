---
title: 在AEM Forms OSGi中配置Reader扩展
description: 将Reader扩展凭据添加到AEM Forms OSGi中的信任存储区
feature: Reader Extensions
type: Tutorial
version: Experience Manager 6.4, Experience Manager 6.5
topic: Administration
role: Admin
level: Beginner
exl-id: 1f16acfd-e8fd-4b0d-85c4-ed860def6d02
last-substantial-update: 2020-08-01T00:00:00Z
duration: 308
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '209'
ht-degree: 0%

---

# 添加Reader扩展凭据{#configuring-reader-extension-osgi}

DocAssurance服务可以对PDF文档应用使用权限。 要对PDF文档应用使用权限，请配置证书。

## 为fd-service用户创建密钥库

Reader扩展凭据与fd-service用户相关联。 要向fd-service用户添加凭据，请执行以下步骤。 如果已为fd-service用户创建密钥库，请跳过此部分

* 以管理员身份登录AEM创作实例
* 转到Tools-Security-Users
* 向下滚动用户列表，直到找到全功能用户帐户
* 单击fd-service用户
* 单击keystore选项卡
* 单击Create KeyStore
* 设置KeyStore访问密码并保存您的设置以创建KeyStore密码

### 向fd-service用户密钥库添加凭据

请观看视频，向fd-service用户添加凭据

>[!VIDEO](https://video.tv.adobe.com/v/3447297?quality=12&learn=on&captions=chi_hans)


列出pfx文件详细信息的命令是。 以下命令假定您与pfx文件位于同一目录中。

**keytool -v -list -storetype pkcs12 -keystore &lt;您的.pfx文件的名称>**

例如，keytool -v -list -storetype pkcs12 -keystore 1005566.pfx其中1005566.pfx是my pfx文件的名称
