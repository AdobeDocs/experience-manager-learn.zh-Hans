---
title: 创建客户端库
description: 用于提取下一个要签名的表单的客户端库代码
feature: Adaptive Forms
version: 6.4,6.5
jira: KT-6907
thumbnail: 6907.jpg
topic: Development
role: Developer
level: Intermediate
exl-id: 3c148b30-2c7d-428d-9a3c-f3067ca3a239
duration: 58
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '91'
ht-degree: 3%

---

# 创建客户端库

创建自定义客户端库（简称clientlib）以提取url参数，并在GET调用中传递这些参数。 将对挂载在/bin/getnextformtosign上的servlet进行GET调用，该servlet将返回用于登录包的下一个表单的url。

以下是clientlib javascript函数中使用的代码


```java
function getUrlVars()
{
    var vars = {};
    var parts = window.location.href.replace(/[?&]+([^=&]+)=([^&]*)/gi, function(m, key, value)
    {
        vars[key] = value;
    });
    return vars;
}

function navigateToNextForm()
{
    
    console.log("The id is " + guidelib.runtime.adobeSign.submitData.agreementId);
    var guid = getUrlVars()["guid"];
    var customerID = getUrlVars()["customerID"];
    console.log("The customer Id is " + customerID);
    $.ajax(
    {
        type: 'GET',
        url: '/bin/getnextformtosign?guid=' + guid + '&customerID=' + customerID,
        contentType: false,
        processData: false,
        cache: false,
        success: function(response)
        {
            console.log(response);
            var jsonResponse = JSON.parse(JSON.stringify(response));
            console.log(jsonResponse.nextFormToSign);
            var nextFormToSign = jsonResponse.nextFormToSign;
            if (nextFormToSign != "AllDone")
            {
                window.open(nextFormToSign, '_self');
            }
            else
            {
                window.open("http://localhost:4502/content/forms/af/formsandsigndemo/alldone.html", '_self');
            }

}
    });
}
$(document).ready(function()
{
    $(document).on("click", ".nextform", navigateToNextForm);
});
```

## 资源

[可以从此处下载clientlib](assets/get-next-form-client-lib.zip)

## 后续步骤

[为此用例创建自定义表单模板](./create-af-template.md)