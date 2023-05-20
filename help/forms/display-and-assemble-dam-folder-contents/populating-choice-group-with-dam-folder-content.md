---
title: 將DAM資料夾專案新增至選擇群組元件
description: 動態新增專案至選擇群組元件
feature: Adaptive Forms
version: 6.5
topic: Development
role: User
level: Beginner
last-substantial-update: 2023-01-01T00:00:00Z
exl-id: 29f56d13-c2e2-4bc2-bfdc-664c848dd851
source-git-commit: bd41cd9d64253413e793479b5ba900c8e01c0eab
workflow-type: tm+mt
source-wordcount: '228'
ht-degree: 1%

---

# 動態新增專案至選擇群組元件

AEM Forms 6.5匯入動態新增專案至最適化Forms選擇群組元件（例如CheckBox、選項按鈕和影像清單）的功能。 在本文中，我們將瞭解使用DAM資料夾內容填入選擇群組元件的使用案例。 在熒幕擷圖中，這3個檔案位於名為電子報的資料夾中。每次將新電子報新增至資料夾時，選擇群組元件都會更新以自動列出其內容。 使用者可以選取一或多個要下載的電子報。

![规则编辑器](assets/newsletters-download.png)

## 建立servlet以傳回DAM資料夾內容

所寫入的下列程式碼會以JSON格式傳回DAM資料夾內容。

```java
package com.newsletters.core.servlets;
import static com.day.cq.commons.jcr.JcrConstants.JCR_CONTENT;
import java.io.IOException;
import java.io.PrintWriter;
import java.util.ArrayList;
import java.util.List;
import javax.servlet.Servlet;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.resource.Resource;
import org.apache.sling.api.servlets.SlingSafeMethodsServlet;
import org.osgi.service.component.annotations.Component;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import com.google.gson.Gson;
import com.google.gson.JsonObject;

@Component(service = {
  Servlet.class
}, property = {
  "sling.servlet.methods=get",
  "sling.servlet.paths=/bin/listfoldercontents"
})
public class ListFolderContent extends SlingSafeMethodsServlet {
  private static final long serialVersionUID = 1 L;
  private static final Logger log = LoggerFactory.getLogger(ListFolderContent.class);
  protected void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response) {
    Resource resource = request.getResourceResolver().getResource(request.getParameter("damFolder"));
    List < JsonObject > results = new ArrayList < > ();
    resource.getChildren().forEach(child -> {
      if (!JCR_CONTENT.equals(child.getName())) {
        JsonObject asset = new JsonObject();
        log.debug("##The child name is " + child.getName());
        asset.addProperty("assetname", child.getName());
        asset.addProperty("assetpath", child.getPath());
        results.add(asset);

      }
    });
    PrintWriter out = null;
    try {
      out = response.getWriter();
    } catch (IOException e) {

      log.debug(e.getMessage());
    }
    response.setContentType("application/json");
    response.setCharacterEncoding("UTF-8");
    Gson gson = new Gson();
    out.print(gson.toJson(results));
    out.flush();
  }

}
```

## 使用JavaScript函式建立使用者端資料庫

此servlet是從JavaScript函式叫用。 此函式傳回陣列物件，此物件將用於填入選擇群組元件

```javascript
/**
 * Populate drop down/choice group  with assets from specified folder
 * @return {string[]} 
 */
function getDAMFolderAssets(damFolder) {
   // strUrl is whatever URL you need to call
   var strUrl = '/bin/listfoldercontents?damFolder=' + damFolder;
   var documents = [];
   $.ajax({
      url: strUrl,
      success: function(jsonData) {
         for (i = 0; i < jsonData.length; i++) {
            documents.push(jsonData[i].assetpath + "=" + jsonData[i].assetname);
         }
      },
      async: false
   });
   return documents;
}
```

## 建立最適化表單

建立最適化表單並將表單與客戶庫建立關聯 **listfolderassets**. 將核取方塊元件新增至表單。 使用規則編輯器填入核取方塊的選項，如熒幕擷取畫面所示
![set-options](assets/set-options-newsletter.png)

我們正在叫用javascript函式，稱為 **getDAMFolderAssets** 並傳遞DAM資料夾資產的路徑以列在表單中。

## 后续步骤

[組合選取的資產](./assemble-selected-newsletters.md)
