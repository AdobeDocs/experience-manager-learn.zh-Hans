---
title: 在自适应表单中显示二维码
description: 在自适应表单中显示二维码
feature: Adaptive Forms
type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-15603
last-substantial-update: 2024-05-28T00:00:00Z
exl-id: 0c6079f4-601e-4a82-976c-71dbb2faa671
source-git-commit: 1977e5103de72a0db5f446eba539d4ae5b810e74
workflow-type: tm+mt
source-wordcount: '203'
ht-degree: 0%

---

# 示例二维码组件

在自适应表单中嵌入二维码可以大大提高用户访问表单相关附加信息的方便性和效率。

示例组件使用[QRCode.js](https://davidshimjs.github.io/qrcodejs/)。

QRCode.js是一个用于生成QRCode的javascript库，它支持带有HTML5画布的跨浏览器以及DOM中的表标签。

组件根据在组件的configuration属性中指定的值生成二维码。
![图像](assets/qr-code-url.png)

以下代码在qr代码生成器组件的body.jsp中使用。

“url”是需要嵌入到二维码中的url。 此URL在QR代码组件的配置属性中指定。

```java
<%@include file="/libs/foundation/global.jsp"%>
<body>
    <h2>Scan the QR Code for more information related to this form</h2>
    <div data-url="<%=properties.get("url")%>">
    </div>
    <div id="qrcode">
    </div>
</body>
```



以下代码使用qr-code-generator组件的客户端库中QRCode.js库的makeCode方法。生成的QR代码将附加到id **&quot;qrcode&quot;**&#x200B;标识的div中。

```javascript
$(document).ready(function()
  {
      var qrcode = new QRCode("qrcode");
      qrcode.makeCode(document.querySelector("[data-url]").getAttribute("data-url"));
      
 });
```

## 在本地服务器上部署资产

* [使用包管理器下载并安装二维码组件。](assets/qrcode.zip)
* [使用包管理器下载并安装自适应表单示例。](assets/form-with-qr-code.zip)
* [预览表单](http://localhost:4502/content/dam/formsanddocuments/qrcode/w9form/jcr:content?wcmmode=disabled)。 表单的帮助部分包含二维码。
