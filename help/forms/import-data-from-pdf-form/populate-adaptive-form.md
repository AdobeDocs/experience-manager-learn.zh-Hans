---
title: 使用setData方法填充自适应表单
description: 发送上传的pdf文件以进行数据提取，并使用提取的数据填充自适应表单
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-14196
exl-id: f380d589-6520-4955-a6ac-2d0fcd5aaf3f
duration: 38
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '118'
ht-degree: 1%

---

# 进行Ajax调用

当用户上传了pdf文件时，我们需要对servlet进行POST调用，并将上传的PDF文档传递到POST请求中。 POST请求会返回crx存储库中导出数据的路径

```javascript
$("#fileElem").on('change', function (e) {
           console.log("submitting files");
           var filesUploaded = e.target.files;
           var ajaxData = new FormData($("#myform").get(0));
           for (var i = 0; i < filesUploaded.length; i++) {
               ajaxData.append(filesUploaded[i].name, filesUploaded[i]);
           }

           handleFiles(ajaxData);

       });

function handleFiles(formData) {
    console.log("File uploaded");

    $.ajax({
        type: 'POST',
        data: formData,
        url: '/bin/ExtractDataFromPDF',
        contentType: false,
        processData: false,
        cache: false,
        success: function (filePath) {
            console.log(filePath);
            guideBridge.setData({
                dataRef: filePath,
                error: function (guideResultObject) {
                    console.log("Error");
                }
            })
            

        }
    });
}
```

挂载的servlet **_/bin/ExtractDataFromPDF_** 从PDF文件中提取数据，并返回存储提取数据的crx节点的路径。
此 [GuideBridge设置数据](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javascript-api/GuideBridge.html#setData__anchor) 然后，使用方法设置自适应表单的数据。

## 后续步骤

[部署示例资源](./test-the-solution.md)
