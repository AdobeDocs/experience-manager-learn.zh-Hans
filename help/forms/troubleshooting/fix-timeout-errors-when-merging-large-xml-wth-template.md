---
title: 修复将大型xml数据文件与xdp模板合并时的超时错误
description: 在AEM Forms中将大型xml文件与模板合并
type: Troubleshooting
role: Admin
level: Intermediate
version: 6.5
feature: Output Service,Forms Service
topic: Administration
kt: 11091
exl-id: 933ec5f6-3e9c-4271-bc35-4ecaf6dbc434
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '185'
ht-degree: 5%

---

# 如何通过将大型xml数据文件与xdp模板合并来启用pdf文件的创建

将大型xml数据文件与xdp模板合并时，可能会在日志文件中看到以下错误

```txt
POST /services/OutputService/GeneratePdfOutput HTTP/1.1] com.adobe.fd.output.internal.exception.OutputServiceException AEM_OUT_001_003:Unexpected Exception: client timeout reached org.omg.CORBA.TIMEOUT: client timeout reached
```

要修复上述错误，请执行以下操作

## 更改aries超时

* 停止AEM服务器
* 创建名为的文件夹 **安装** 在AEM安装的crx-quickstart文件夹下
* 创建一个名为的文件 **org.apache.aries.transaction.config** 安装文件夹下包含以下content aries.transaction.timeout=&quot;1200&quot;。 您可以根据需要更改超时值。 超时值以秒为单位

>[!NOTE]
> 创建org.apache.aries.transaction配置后，您可以从以下位置编辑事务超时值： [configMgr](http://localhost:4502/system/console/configMgr) 而不是编辑文件


## 更改Jacorb ORB提供程序设置

* [打开OSGi ConfigMgr](http://localhost:4502/system/console/configMgr)
* 搜索 **Jacorb ORB提供程序**
* 添加以下条目jacorb.connection.client.pending_reply_timeout=600000以上设置将挂起回复超时（也称为CORBA客户端超时）设置为600秒。
* 保存更改
