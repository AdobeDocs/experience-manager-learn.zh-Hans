---
title: 创建客户端库
description: 创建clientlibrary以处理“保存并退出”按钮的单击事件
feature: 自适应表单
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6597
thumbnail: 6597.pg
topic: 开发
role: 开发人员
level: 中间
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '146'
ht-degree: 2%

---

# 创建客户端库

创建[客户端lib](https://docs.adobe.com/content/help/en/experience-manager-65/developing/introduction/clientlibs.html)，其中将包含在CSS类&#x200B;**savebutton**&#x200B;所标识的按钮的单击事件上调用`guideBridge` API方法`doAjaxSubmitWithFileAttachment`的代码。  我们将自适应表单数据`fileMap`和`mobileNumber`传递给侦听`**/bin/storeafdatawithattachments`的端点

保存表单数据后，将生成唯一的应用程序ID并在对话框中向用户显示。 取消对话框后，用户会转到表单，该表单允许用户使用唯一的应用程序id检索保存的自适应表单。

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
> 我们已使用[bootbox javascript库](http://bootboxjs.com/examples.html)显示对话框

此示例中使用的客户端库可从此处](assets/client-libraries.zip)下载[
