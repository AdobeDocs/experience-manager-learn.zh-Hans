---
title: 修复将大型xml数据文件与xdp模板合并时的超时错误
description: 在AEM Forms中将大型xml文件与模板合并
type: Troubleshooting
role: Admin
level: Intermediate
version: Experience Manager 6.5
feature: Output Service,Forms Service
topic: Administration
jira: KT-11091
exl-id: 933ec5f6-3e9c-4271-bc35-4ecaf6dbc434
duration: 37
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '178'
ht-degree: 0%

---

# 如何通过将大型xml数据文件与xdp模板合并来创建pdf文件

将大型xml数据文件与xdp模板合并时，可能会在日志文件中看到以下错误

```txt
POST /services/OutputService/GeneratePdfOutput HTTP/1.1] com.adobe.fd.output.internal.exception.OutputServiceException AEM_OUT_001_003:Unexpected Exception: client timeout reached org.omg.CORBA.TIMEOUT: client timeout reached
```

要修复上述错误，请执行以下操作

## 更改aries超时

* 停止AEM服务器
* 在AEM安装的crx-quickstart文件夹下创建名为&#x200B;**install**&#x200B;的文件夹
* 创建名为&#x200B;**org.apache.aries.transaction.config**的文件，该文件包含以下内容
aries.transaction.timeout=&quot;1200&quot;
在“安装文件夹”下。 您可以根据需要更改超时值。 超时值以秒为单位

>[!NOTE]
> 创建org.apache.aries.transaction配置后，您可以从[configMgr](http://localhost:4502/system/console/configMgr)中编辑事务超时值，而不是编辑文件


## 更改Jacorb ORB提供程序设置

* [打开OSGi ConfigMgr](http://localhost:4502/system/console/configMgr)
* 搜索&#x200B;**Jacorb ORB提供程序**
* 添加以下条目
jacorb.connection.client.pending_reply_timeout=600000
上述设置将挂起回复超时（也称为CORBA客户端超时）设置为600秒。
* 保存更改
