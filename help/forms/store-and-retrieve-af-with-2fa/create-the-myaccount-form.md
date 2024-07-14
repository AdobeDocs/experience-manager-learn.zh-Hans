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
duration: 53
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '264'
ht-degree: 0%

---

# 创建MyAccountForm

表单&#x200B;**MyAccountForm**&#x200B;用于在用户验证应用程序ID以及与应用程序ID关联的手机号码之后检索部分完成的自适应表单。

![我的帐户表单](assets/6599.JPG)

当用户输入应用程序ID并单击&#x200B;**FetchApplication**&#x200B;按钮时，将使用表单数据模型的“获取”操作从数据库中获取与应用程序ID关联的手机号码。

该表单利用表单数据模型的POST调用，利用OTP验证手机号码。 使用以下代码成功验证手机号码时，会触发表单提交操作。 我们正在触发名为&#x200B;**submitForm**&#x200B;的提交按钮的点击事件。

>[!NOTE]
> 您需要在MyAccountForm的相应字段中提供特定于您的[Nexmo](https://dashboard.nexmo.com/)帐户的API密钥和API密钥值

![trigger-submit](assets/trigger-submit.JPG)



此表单与将表单提交转发到挂载在&#x200B;**/bin/renderaf**&#x200B;上的servlet的自定义提交操作相关联

```java
com.adobe.aemds.guide.utils.GuideSubmitUtils.setForwardPath(slingRequest,"/bin/renderaf",null,null);
```

装载在&#x200B;**/bin/renderaf**&#x200B;上的servlet中的代码将转发请求，以呈现使用保存的数据预填充的storeafwithattachments自适应表单。


* 可以从此处[下载MyAccountForm](assets/my-account-form.zip)

* 示例表单基于[自定义自适应表单模板](assets/custom-template-with-page-component.zip)，该模板需要导入到AEM中，才能正确呈现示例表单。

* 需要将与提交MyAccountForm关联的[自定义提交处理程序](assets/custom-submit-my-account-form.zip)导入到AEM中。

## 后续步骤

[通过部署示例资源测试解决方案](./deploy-this-sample.md)
