---
title: 创建MyAccountForm
description: 创建myaccount表单以在成功验证应用程序ID和电话号码时检索部分完成的表单。
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
jira: KT-6599
thumbnail: 6599.jpg
topic: Development
role: User
level: Beginner
exl-id: 1ecd8bc0-068f-4557-bce4-85347c295ce0
duration: 60
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '264'
ht-degree: 0%

---

# 创建MyAccountForm

表单 **我的帐户表单** 用于在用户验证应用程序id以及与应用程序id关联的手机号码后检索部分完成的自适应表单。

![我的帐户表单](assets/6599.JPG)

当用户输入应用程序ID并单击 **FetchApplication** 按钮，则使用表单数据模型的Get操作从数据库获取与应用程序ID关联的手机号码。

该表单利用表单数据模型的POST调用，利用OTP验证手机号码。 使用以下代码成功验证手机号码时，会触发表单提交操作。 我们将触发提交按钮的点击事件，该按钮名为 **submitform**.

>[!NOTE]
> 您需要提供特定于您的 [Nexmo](https://dashboard.nexmo.com/) 帐户在MyAccountForm的相应字段中

![trigger-submit](assets/trigger-submit.JPG)



此表单与将表单提交转发到所装载servlet的自定义提交操作相关联 **/bin/renderaf**

```java
com.adobe.aemds.guide.utils.GuideSubmitUtils.setForwardPath(slingRequest,"/bin/renderaf",null,null);
```

装载在Servlet中的代码 **/bin/renderaf** 转发请求以呈现使用保存的数据预填充的storeafwithattachments自适应表单。


* 我的帐户表单可以 [从此处下载](assets/my-account-form.zip)

* 示例表单基于 [自定义自适应表单模板](assets/custom-template-with-page-component.zip) 这些规则需要导入到AEM中，才能正确呈现示例表单。

* [自定义提交处理程序](assets/custom-submit-my-account-form.zip) 需要将与提交的MyAccountForm关联的导入到AEM中。

## 后续步骤

[通过部署示例资源测试解决方案](./deploy-this-sample.md)
