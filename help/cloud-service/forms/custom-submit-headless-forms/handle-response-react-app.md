---
title: 从自定义提交中提取响应
description: 在成功提交表单时提取自定义响应
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-13520
exl-id: e5f76d6a-2ea8-4909-9cfb-b673077cf8fd
duration: 16
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '51'
ht-degree: 0%

---

# 从响应中提取json对象

当您向自定义提交处理程序提交Headless自适应表单时，可以提取提交处理程序返回的数据，并将其显示在您的react应用程序中。 以下代码片段显示

```javascript
// associate event handler for the onSubmitSuccess event
<AdaptiveForm mappings={extendMappings} onSubmitSuccess={onSuccess} formJson={selectedForm}/>
```

```javascript
// extract the json returned by the custom submit service
const onSuccess=(action) =>{
        let body = action.payload?.body;
        let FirstName = JSON.parse(body.metadata.json).firstName;
        setThankYouMessage(FirstName+" your request has been received");
        
      }
```
