---
title: 儲存和擷取MySQL資料庫的表單資料 — 建立使用者端程式庫
description: 多部分教學課程，逐步引導您完成儲存和擷取表單資料的相關步驟
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: eef98a55-80d0-4598-abf2-02a6c5247b64
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '147'
ht-degree: 0%

---

# 建立使用者端資源庫

AEM使用者端資料庫會管理您的所有使用者端JavaScript程式碼。 針對本文，我建立了一個簡單的JavaScript，以使用Guide Bridge API擷取最適化表單資料。 擷取調適型表單資料後，會對servlet進行POST呼叫，以在資料庫中插入或更新調適型表單資料。 函式getALLUrlParams會傳回URL中的引數。 如果URL中有guid引數，則我們需要執行更新操作（如果不是插入操作）。其餘功能會在與.savebutton類別的click事件相關聯的程式碼中處理。

>[!NOTE]
>
>使用者端資料庫是本教學課程資產的一部分

```javascript
function getAllUrlParams(url) {

    // get query string from url (optional) or window
    var queryString = url ? url.split('?')[1] : window.location.search.slice(1);

    // we'll store the parameters here
    var obj = {};

    // if query string exists
    if (queryString) {

        // stuff after # is not part of query string, so get rid of it
        queryString = queryString.split('#')[0];

        // split our query string into its component parts
        var arr = queryString.split('&');

        for (var i = 0; i < arr.length; i++) {
            // separate the keys and the values
            var a = arr[i].split('=');

            // set parameter name and value (use 'true' if empty)
            var paramName = a[0];
            var paramValue = typeof(a[1]) === 'undefined' ? true : a[1];

            // (optional) keep case consistent
            paramName = paramName.toLowerCase();
            if (typeof paramValue === 'string') paramValue = paramValue.toLowerCase();

            // if the paramName ends with square brackets, e.g. colors[] or colors[2]
            if (paramName.match(/\[(\d+)?\]$/)) {

                // create key if it doesn't exist
                var key = paramName.replace(/\[(\d+)?\]/, '');
                if (!obj[key]) obj[key] = [];

                // if it's an indexed array e.g. colors[2]
                if (paramName.match(/\[\d+\]$/)) {
                    // get the index value and add the entry at the appropriate position
                    var index = /\[(\d+)\]/.exec(paramName)[1];
                    obj[key][index] = paramValue;
                } else {
                    // otherwise add the value to the end of the array
                    obj[key].push(paramValue);
                }
            } else {
                // we're dealing with a string
                if (!obj[paramName]) {
                    // if it doesn't exist, create property
                    obj[paramName] = paramValue;
                } else if (obj[paramName] && typeof obj[paramName] === 'string') {
                    // if property does exist and it's a string, convert it to an array
                    obj[paramName] = [obj[paramName]];
                    obj[paramName].push(paramValue);
                } else {
                    // otherwise add the property
                    obj[paramName].push(paramValue);
                }
            }
        }
    }

    return obj;
}

$(document).ready(function() {
    console.log(window.location.hostname + "   " + window.location.protocol);
    var linktext = guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].info[0].linktxt[0]");
    var linktext1 = guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].info[0].linktext1[0]");
    linktext.visible = false;
    linktext1.visible = false;
    $(".savebutton").click(function() {
        var params = getAllUrlParams(window.location.href);
        console.log(getAllUrlParams(window.location.href));
        window.guideBridge.getDataXML({
            success: function(guideResultObject) {
                console.log("The data is " + guideResultObject.data);
                let xhr = new XMLHttpRequest();
                xhr.open('POST', '/bin/storeafdata');
                let formData = new FormData();
                if (typeof(params.guid) != "undefined") {
                    formData.append("operation", "update");
                    formData.append("guid", params.guid);

                }
                if (typeof(params.guid) == "undefined") {
                    formData.append("operation", "insert");


                }


                formData.append("formdata", guideResultObject.data);
                xhr.send(formData);
                xhr.onload = function(e) {
                    console.log("The data is ready");
                    if (this.status == 200) {
                        var jsonResponse = JSON.parse(this.response);
                        console.log(jsonResponse.guid);
                        var linktext = guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].info[0].linktxt[0]");
                        var linktext1 = guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].info[0].linktext1[0]");
                        linktext1.visible = true;
                        linktext.value =  "/content/dam/formsanddocuments/demostoreandretrieveformdata/jcr:content?wcmmode=disabled&guid=" + jsonResponse.guid;
                        linktext.visible = true;
                        guideBridge.setFocus("guide[0].guide1[0].guideRootPanel[0].info[0].linktxt[0]");
                    }

                }
            }
        });


    });


});
```
