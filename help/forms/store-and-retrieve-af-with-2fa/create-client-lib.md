---
title: 创建客户端库
description: 创建clientlibrary以处理“保存并退出”按钮的点击事件
feature: 自适应表单
type: Tutorial
version: 6.4,6.5
kt: 6597
thumbnail: 6597.pg
topic: 开发
role: Developer
level: Intermediate
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '142'
ht-degree: 2%

---

# 创建客户端库

创建[client lib](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html)，该库将包含在CSS类&#x200B;**savebutton**&#x200B;标识的按钮的点击事件上调用`guideBridge` API的方法`doAjaxSubmitWithFileAttachment`的代码。  我们将自适应表单数据`fileMap`和`mobileNumber`传递到监听`**/bin/storeafdatawithattachments`的端点

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
> 我们使用了[bootbox javascript库](http://bootboxjs.com/examples.html)来显示对话框

此示例中使用的客户端库可从此处](assets/client-libraries.zip)下载[
