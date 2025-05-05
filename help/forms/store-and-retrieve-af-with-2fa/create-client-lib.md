---
title: 创建客户端库
description: 创建客户端库以处理“保存并退出”按钮的点击事件
feature: Adaptive Forms
type: Tutorial
version: Experience Manager 6.4, Experience Manager 6.5
jira: KT-6597
thumbnail: 6597.pg
topic: Development
role: Developer
level: Intermediate
exl-id: c90eea73-bd44-40af-aa98-d766aa572415
duration: 42
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '140'
ht-degree: 1%

---

# 创建客户端库

创建[客户端库](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html?lang=zh-Hans)，该库将包含用于在CSS类&#x200B;**保存按钮**&#x200B;标识的按钮的单击事件上调用`guideBridge` API的方法`doAjaxSubmitWithFileAttachment`的代码。  我们将自适应表单数据`fileMap`和`mobileNumber`传递到在`**/bin/storeafdatawithattachments`上侦听的终结点

保存表单数据后，会生成一个唯一的应用程序ID并在对话框中向用户显示。 在禁用对话框时，用户将被带入表单，表单允许他们使用唯一的应用程序ID检索保存的自适应表单。

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
                  x.data.applicationID +
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
> 我们已使用[bootbox JavaScript库](https://bootboxjs.com/examples.html)来显示对话框

此示例中使用的客户端库可以从此处[下载。](assets/store-af-with-attachments-client-lib.zip)

## 后续步骤

[验证具有OTP服务的用户](./verify-users-with-otp.md)
