---
title: 创建MyAccountForm
description: 创建myaccount表单，以在成功验证应用程序ID和电话号码时检索部分完成的表单。
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
kt: 6599
thumbnail: 6599.jpg
topic: Development
role: User
level: Beginner
exl-id: 1ecd8bc0-068f-4557-bce4-85347c295ce0
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '265'
ht-degree: 0%

---

# 创建MyAccountForm

表单 **MyAccountForm** 在用户验证了应用程序id和与应用程序id关联的移动号码后，用于检索部分完成的自适应表单。

![我的帐户表单](assets/6599.JPG)

当用户输入应用程序ID并单击 **FetchApplication** 按钮，则使用表单数据模型的Get操作从数据库中获取与应用程序id关联的移动号码。

此表单利用表单数据模型的POST调用来使用OTP验证移动号码。 使用以下代码成功验证手机号码时，会触发表单的提交操作。 我们将触发名为的提交按钮的点击事件 **submitForm**.

>[!NOTE]
> 您需要提供特定于您的的API密钥和API密钥值 [Nexmo](https://dashboard.nexmo.com/) 帐户

![trigger-submit](assets/trigger-submit.JPG)



此表单与自定义提交操作关联，该操作会将表单提交转发到装载在上的Servlet **/bin/renderaf**

```java
com.adobe.aemds.guide.utils.GuideSubmitUtils.setForwardPath(slingRequest,"/bin/renderaf",null,null);
```

Servlet中装载的代码 **/bin/renderaf** 转发请求以呈现预填充了保存数据的storeafwithattachments自适应表单。


* MyAccountForm可以 [从此处下载](assets/my-account-form.zip)

* 示例表单基于 [自定义自适应表单模板](assets/custom-template-with-page-component.zip) 需要导入到AEM中才能正确呈现示例表单。

* [自定义提交处理程序](assets/custom-submit-my-account-form.zip) 需要将与MyAccountForm提交关联的URL导入AEM。

## 后续步骤

[通过部署示例资产来测试解决方案](./deploy-this-sample.md)
