---
title: 创建MyAccountForm
description: 创建myaccount表单，以在成功验证应用程序ID和电话号码时检索部分完成的表单。
feature: 自适应表单
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6599
thumbnail: 6599.jpg
topic: 开发
role: 业务从业者
level: 初学者
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '261'
ht-degree: 1%

---



# 创建MyAccountForm

表单&#x200B;**MyAccountForm**&#x200B;用于在用户验证了应用程序id和与应用程序id关联的移动号码后检索部分完成的自适应表单。

![我的帐户表](assets/6599.JPG)

当用户输入应用程序ID并单击&#x200B;**FetchApplication**&#x200B;按钮时，使用表单数据模型的“获取”操作从数据库中获取与应用程序ID关联的移动号码。

此表单利用表单POST调用数据模型来验证使用OTP的移动号码。 使用以下代码成功验证手机号码时，将触发表单的提交操作。 我们将触发名为&#x200B;**submitForm**&#x200B;的提交按钮的单击事件。

>[!NOTE]
> 您需要在MyAccountForm的相应字段中提供特定于[Nexmo](https://dashboard.nexmo.com/)帐户的API密钥和API机密值

![trigger-submit](assets/trigger-submit.JPG)



此表单与自定义提交操作关联，该自定义提交操作会将表单提交转发到装载在&#x200B;**/bin/renderaf**&#x200B;上的servlet

```java
com.adobe.aemds.guide.utils.GuideSubmitUtils.setForwardPath(slingRequest,"/bin/renderaf",null,null);
```

装载在&#x200B;**/bin/renderaf**&#x200B;上的servlet中的代码转发请求以呈现带附件的自适应表单，其中预填充了保存的数据。


* MyAccountForm可从此处](assets/my-account-form.zip)下载[

* 示例表单基于[自定义自适应表单模板](assets/custom-template-with-page-component.zip)，需要将其导入AEM中，以使示例表单能够正确呈现。

* [需要将](assets/custom-submit-my-account-form.zip) 与MyAccountForm提交关联的自定义提交处理程序导入AEM。
