---
title: 修复了使用xdp模板合并大型xml数据文件时的超时错误
description: 在AEM Forms中将大型xml文件与模板合并
type: Troubleshooting
role: Admin
level: Intermediate
version: 6.5
feature: Output Service,Forms Service
topic: Administration
kt: 11091
source-git-commit: 164741ce5ae7d00f904365589438c2eaaf1e05db
workflow-type: tm+mt
source-wordcount: '185'
ht-degree: 5%

---

# 如何通过将大xml数据文件与xdp模板合并来启用PDF文件的创建

将大型xml数据文件与xdp模板合并时，您可能会在日志文件中看到以下错误

```txt
POST /services/OutputService/GeneratePdfOutput HTTP/1.1] com.adobe.fd.output.internal.exception.OutputServiceException AEM_OUT_001_003:Unexpected Exception: client timeout reached org.omg.CORBA.TIMEOUT: client timeout reached
```

要修复上述错误，请执行以下操作

## 更改超时

* 停止AEM Server
* 创建名为 **安装** 在AEM安装的crx-quickstart文件夹下
* 创建一个名为 **org.apache.aries.transaction.config** 安装文件夹下的以下内容aries.transaction.timeout=&quot;1200&quot;。 您可以根据需要更改超时值。 超时值以秒为单位

>[!NOTE]
> 创建org.apache.aries.transaction配置后，即可从 [configMgr](http://localhost:4502/system/console/configMgr) 而不是编辑文件


## 更改Jacorb ORB提供程序设置

* [打开OSGi ConfigMgr](http://localhost:4502/system/console/configMgr)
* 搜索 **Jacorb ORB提供商**
* 添加以下条目jacorb.connection.client.pending_reply_timeout=600000上述设置将待处理的回复超时（也称为CORBA客户端超时）设置为600秒。
* 保存更改
