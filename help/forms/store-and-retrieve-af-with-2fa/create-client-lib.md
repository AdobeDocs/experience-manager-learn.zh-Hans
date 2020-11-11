---
title: 创建客户端库
description: 创建客户端库以处理“保存并退出”按钮的单击事件
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6597
thumbnail: 6597.pg
translation-type: tm+mt
source-git-commit: 9d4e864f42fa6c0b2f9b895257db03311269ce2e
workflow-type: tm+mt
source-wordcount: '141'
ht-degree: 0%

---

# 创建客户端库

创 [建客户端](https://docs.adobe.com/content/help/en/experience-manager-65/developing/introduction/clientlibs.html) lib `doAjaxSubmitWithFileAttachment` ，该库将包括在CSS类savebutton标识的按钮的单击 `guideBridge` 事件上调用 **API方法的**&#x200B;代码。  我们将自适应表单数据 `fileMap`传递给 `mobileNumber` 在 `**/bin/storeafdatawithattachments`

保存表单数据后，将生成唯一的应用程序ID并在对话框中向用户显示。 取消对话框后，用户将转到表单，该表单允许用户使用唯一的应用程序ID检索保存的自适应表单。

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
> 我们已使用 [bootbox javascript库](http://bootboxjs.com/examples.html) 来显示对话框

此示例中使用的客户端库可从 [此处下载](assets/client-libraries.zip)
