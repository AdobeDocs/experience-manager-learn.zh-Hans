---
title: 儲存表單附件
description: 擷取表單附件並儲存在CRX存放庫中的新位置。
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
kt: 6537
thumbnail: 6537.jpg
topic: Development
role: Developer
level: Experienced
exl-id: ec50b9b1-e28c-4d84-ae90-6a21c9700688
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '192'
ht-degree: 1%

---

# 儲存表單附件

將附件新增至最適化表單時，附件會儲存在CRX存放庫中的臨時位置。 為了讓我們的使用案例發揮作用，我們需要將表單附件儲存在CRX存放庫中的新位置。

OSGi服務的建立是將表單附件儲存在CRX存放庫中的新位置。 系統會使用CRX中附件的新位置來建立新的檔案地圖，並傳回至呼叫的應用程式。
以下是傳送至servlet的FileMap。 索引鍵為最適化表單欄位，值為附件的暫時位置。 在servlet中，我們將擷取附件，並將其儲存在AEM存放庫中的新位置，並使用新位置更新FileMap

```java
{
"guide[0].guide1[0].guideRootPanel[0].idDocumentPanel[0].idcard[0]": "idcard/CA-DriversLicense.pdf",
"guide[0].guide1[0].guideRootPanel[0].documentation[0].yourBankStatements[0].table1603552612235[0].Row1[0].tableItem11[0]": "tableItem11/BankStatement-Sept-2020.pdf"
}
```

以下程式碼會從請求中擷取附件，並將其儲存在下 **/content/afattachments** 資料夾

```java
public String storeAFAttachments(JSONObject fileMap, SlingHttpServletRequest request) {
    JSONObject newFileMap = new JSONObject();
    try {
        Iterator keys = fileMap.keys();
        log.debug("The file map is " + fileMap.toString());
        while (keys.hasNext()) {
            String key = (String) keys.next();
            log.debug("#### The key is " + key);
            String attacmenPath = (String) fileMap.get((key));
            log.debug("The attachment path is " + attacmenPath);
            if (!attacmenPath.contains("/content/afattachments")) {
                String fileName = attacmenPath.split("/")[1];
                log.debug("#### The attachment name  is " + fileName);
                InputStream is = request.getPart(attacmenPath).getInputStream();
                Document aemFDDocument = new Document(is);
                String crxPath = saveDocumentInCrx("/content/afattachments", fileName, aemFDDocument);
                log.debug(" ##### written to crx repository  " + attacmenPath.split("/")[1]);
                newFileMap.put(key, crxPath);
            } else {
                log.debug("$$$$ The attachment was already added " + key);
                log.debug("$$$$ The attachment path is " + attacmenPath);
                int position = attacmenPath.indexOf("//");
                log.debug("$$$$ After substring " + attacmenPath.substring(position + 1));
                log.debug("$$$$ After splitting " + attacmenPath.split("/")[1]);
                newFileMap.put(key, attacmenPath.substring(position + 1));
            }
        }
    } catch (Exception ex) {
        log.debug(ex.getMessage());
    }
    return newFileMap.toString();

}


}
```

這是具有更新後的表單附件位置的新FileMap

```java
{
"guide[0].guide1[0].guideRootPanel[0].idDocumentPanel[0].idcard[0]": "/content/afattachments/7dc0cbde-404d-49a9-9f7b-9ab5ee7482be/CA-DriversLicense.pdf",
"guide[0].guide1[0].guideRootPanel[0].documentation[0].yourBankStatements[0].table1603552612235[0].Row1[0].tableItem11[0]": "/content/afattachments/81653de9-4967-4736-9ca3-807a11542243/BankStatement-Sept-2020.pdf"
}
```

## 后续步骤

[儲存表單資料](./store-form-data.md)