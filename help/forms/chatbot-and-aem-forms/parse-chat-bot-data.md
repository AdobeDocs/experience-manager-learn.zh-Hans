---
title: 将AEM Forms用于聊天机器人
description: 解析ChatBot数据
feature: Adaptive Forms
version: 6.5
jira: KT-15344
topic: Development
role: User
level: Intermediate
exl-id: 3c304b0a-33f8-49ed-a576-883df4759076
duration: 22
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '97'
ht-degree: 2%

---

# 解析ChatBot数据

A [ChatBot webhook](https://www.chatbot.com/help/webhooks/what-are-webhooks/) 用于将ChatBot数据发送到AEM servlet。
在ChatBot中捕获的数据采用JSON格式，用户在属性对象中输入了数据，如下所示
![聊天机器人数据](assets/chat-bot-data.png)

要将数据与XDP模板合并，我们需要创建以下XML。 请注意xml的根元素，必须与XDP模板的根元素匹配，才能成功合并数据。


```xml
<topmostSubForm>
    <f1_01>David Smith</f1_01>
    <signmethod>ESIGN</signmethod>
    <corporation>1</corporation>
    <f1_08>San Jose, CA, 95110</f1_08>
    <f1_07>345 Park Avenue</f1_07>
    <ssn>123-45-6789</ssn>
    <form_name>W-9</form_name>
</topmostSubForm>
```

![xdp-template](assets/xdp-template.png)

## 后续步骤

[将数据与XDP模板合并](./merge-data-with-template.md)
