---
title: 安装所需的自适应表单react库
description: 将所需的依赖项添加到您的react项目
feature: Adaptive Forms
version: Experience Manager 6.5
jira: KT-13285
topic: Development
role: User
level: Intermediate
exl-id: 0ed44016-d52a-4980-a0b1-06da149c3cb1
duration: 48
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '210'
ht-degree: 0%

---

# 安装所需的依赖项

要在react项目中使用Headless自适应表单，请在react项目中安装以下依赖项

* @aemforms/af-react-components
* @aemforms/af-react-renderer

更新package.json以包含以下依赖项。 在编写本报告时，0.22.41是当前版本

```json
"@aemforms/af-react-components": "^0.22.41",
"@aemforms/af-react-renderer": "^0.22.41",
```

>[!NOTE]
>
>本教程中的下拉列表和卡片布局是使用[材料UI库](https://mui.com/)创建的。 您需要下载相应的材料UI包，以使代码在您的系统中正常工作。

## 设置代理

跨源资源共享(CORS)是一种安全机制，可限制Web浏览器向应用程序托管的域以外的其他域发出请求。 当您尝试从托管在不同域上的API获取数据时，可能会发生CORS错误。 通过设置代理，您可以绕过CORS限制，从React应用程序向API发出请求。 我在src文件夹中名为setUpProxy.js的文件中使用了以下代码。 **确保将目标更改为指向发布实例。**

```
const { createProxyMiddleware } = require('http-proxy-middleware');
const proxy = {
    target: 'https://mypublishinstance:4503/',
    changeOrigin: true
}
module.exports = function(app) {
  app.use(
    '/adobe',
    createProxyMiddleware(proxy)
  ),
  app.use(
    '/content',
    createProxyMiddleware(proxy)
  );
};
```

您还需要安装&#x200B;**http-proxy-middleware**&#x200B;模块并将其添加到您的项目中。

## 后续步骤

[获取要嵌入的表单](./fetch-the-form.md)
