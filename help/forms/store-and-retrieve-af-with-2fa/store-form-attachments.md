---
title: 存储表单附件
description: 提取表单附件并存储在CRX存储库的新位置。
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
jira: KT-6537
thumbnail: 6537.jpg
topic: Development
role: Developer
level: Experienced
exl-id: ec50b9b1-e28c-4d84-ae90-6a21c9700688
duration: 100
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '192'
ht-degree: 1%

---

# 存储表单附件

向自适应表单添加附件时，附件存储在CRX存储库中的临时位置。 要使用例正常工作，我们需要将表单附件存储在CRX存储库中的新位置。

创建OSGi服务以将表单附件存储在CRX存储库中的新位置。 在CRX中使用附件的新位置创建新文件映射，并返回到调用应用程序。
以下是发送到servlet的FileMap。 关键是自适应表单字段，值是附件的临时位置。 在我们的servlet中，我们将提取附件，并将其存储在AEM存储库中的新位置，并使用新位置更新FileMap

```java
{
"guide[0].guide1[0].guideRootPanel[0].idDocumentPanel[0].idcard[0]": "idcard/CA-DriversLicense.pdf",
"guide[0].guide1[0].guideRootPanel[0].documentation[0].yourBankStatements[0].table1603552612235[0].Row1[0].tableItem11[0]": "tableItem11/BankStatement-Sept-2020.pdf"
}
```

以下代码可从请求中提取附件并将其存储在下 **/content/afattachments** 文件夹

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

这是具有表单附件更新位置的新FileMap

```java
{
"guide[0].guide1[0].guideRootPanel[0].idDocumentPanel[0].idcard[0]": "/content/afattachments/7dc0cbde-404d-49a9-9f7b-9ab5ee7482be/CA-DriversLicense.pdf",
"guide[0].guide1[0].guideRootPanel[0].documentation[0].yourBankStatements[0].table1603552612235[0].Row1[0].tableItem11[0]": "/content/afattachments/81653de9-4967-4736-9ca3-807a11542243/BankStatement-Sept-2020.pdf"
}
```

## 后续步骤

[保存表单数据](./store-form-data.md)