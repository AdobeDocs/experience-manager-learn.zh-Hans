---
title: 在AEM Forms OSGi中配置Reader扩展
description: 将Reader扩展凭据添加到AEM Forms OSGi中的信任存储
feature: Reader Extensions
audience: developer
type: Tutorial
version: 6.4,6.5
topic: Administration
role: Admin
level: Beginner
exl-id: 1f16acfd-e8fd-4b0d-85c4-ed860def6d02
last-substantial-update: 2020-08-01T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '209'
ht-degree: 0%

---

# 添加Reader扩展凭据{#configuring-reader-extension-osgi}

DocAssurance服务可以将使用权限应用于PDF文档。 要对PDF文档应用使用权限，请配置证书。

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


用于列出pfx文件详细信息的命令是。 以下命令假定您位于与pfx文件相同的目录中。

**keytool -v -list -storetype pkcs12 -keystore &lt;name of=&quot;&quot; your=&quot;&quot; pfx=&quot;&quot; file=&quot;&quot;>**

例如keytool -v -list -storetype pkcs12 -keystore 1005566.pfx，其中1005566.pfx是我的pfx文件的名称
