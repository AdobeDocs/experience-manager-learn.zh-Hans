---
title: 建立使用者端資源庫
description: 用於擷取下一個要簽署的表單的使用者端程式庫代碼
feature: Adaptive Forms
version: 6.4,6.5
kt: 6907
thumbnail: 6907.jpg
topic: Development
role: Developer
level: Intermediate
exl-id: 3c148b30-2c7d-428d-9a3c-f3067ca3a239
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '91'
ht-degree: 3%

---

# 建立使用者端資源庫

建立自訂使用者端資料庫（簡稱clientlib），以擷取url引數，並在GET呼叫中傳遞這些引數。 會對/bin/getnextformtosign上掛接的servlet進行GET呼叫，該servlet會傳回下一個表單的url，以登入套件。

以下是clientlib javascript函式中使用的程式碼


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

## Assets

[clientlib可從這裡下載](assets/get-next-form-client-lib.zip)

## 后续步骤

[建立此使用案例的自訂表單範本](./create-af-template.md)