---
title: 创建客户端库
description: 创建clientlibrary以处理“保存并退出”按钮的点击事件
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
kt: 6597
thumbnail: 6597.pg
topic: Development
role: Developer
level: Intermediate
exl-id: c90eea73-bd44-40af-aa98-d766aa572415
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '146'
ht-degree: 6%

---

# 创建客户端库

创建 [客户端库](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html) 其中将包含用于调用方法的代码 `doAjaxSubmitWithFileAttachment` 的 `guideBridge` 由CSS类标识的按钮的点击事件上的API **保存按钮**.  我们传递自适应表单数据， `fileMap`和 `mobileNumber` 侦听的端点 `**/bin/storeafdatawithattachments`

保存表单数据后，将生成一个唯一的应用程序ID，并在对话框中向用户显示该ID。 取消对话框后，用户将转到表单，通过该表单，用户可以使用唯一的应用程序ID检索保存的自适应表单。

```java
$(document).ready(function () {
  
  $(".savebutton").click(function () {
    var tel = guideBridge.resolveNode(
      "guide[0].guide1[0].guideRootPanel[0].contactInformation[0].basicContact[0].telephoneNumber[0]"
    );
    var telephoneNumber = tel.value;
    guideBridge.getFormDataString({
      success: function (data) {
        var map = guideBridge._getFileAttachmentMapForSubmit();
        guideBridge.doAjaxSubmitWithFileAttachment(
          "/bin/storeafdatawithattachments",
          {
            data: data.data,
            fileMap: map,
            mobileNumber: telephoneNumber,
          },
          {
            success: function (x) {
              bootbox.alert(
                "This is your reference number.<br>" +
                  x.data.path +
                  " <br>You will need this to retrieve your application",
                function () {
                  console.log(
                    "This was logged in the callback! After the ok button was pressed"
                  );
                  window.location.href =
                    "http://localhost:4502/content/dam/formsanddocuments/myaccountform/jcr:content?wcmmode=disabled";
                }
              );
              console.log(x.data.path);
            },
          },
          guideBridge._getFileAttachmentsList()
        );
      },
    });
  });
});
```

>[!NOTE]
> 我们用过 [引导框javascript库](http://bootboxjs.com/examples.html) 显示对话框

此示例中使用的客户端库可以是 [从此处下载](assets/client-libraries.zip)

## 后续步骤

[使用OTP服务验证用户](./verify-users-with-otp.md)