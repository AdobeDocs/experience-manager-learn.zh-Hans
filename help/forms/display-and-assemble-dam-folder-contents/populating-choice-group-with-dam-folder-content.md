---
title: 将DAM文件夹项目添加到选择的组组件
description: 将项目动态添加到选择组组件
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

# 将项目动态添加到选择的组组件

AEM Forms 6.5引入了将项目动态添加到自适应Forms选择组组件（如复选框、单选按钮和图像列表）的功能。 在本文中，我们将讨论使用DAM文件夹内容填充选择组组件的用例。 在屏幕快照中，3个文件位于名为Newsletter的文件夹中。每次将新Newsletter添加到文件夹时，选择组组件都会更新以自动列出其内容。 用户可以选择要下载的一个或多个新闻稿。

![规则编辑器](assets/newsletters-download.png)

## 创建Servlet以返回DAM文件夹内容

编写以下代码以返回JSON格式的DAM文件夹内容。

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

## 使用JavaScript函数创建客户端库

Servlet通过JavaScript函数调用。 该函数返回一个用于填充选择组组件的数组对象

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

## 创建自适应表单

创建自适应表单并将表单与客户端库关联 **listfolderassets**. 向表单中添加复选框组件。 使用规则编辑器填充复选框的选项，如屏幕截图所示
![set-options](assets/set-options-newsletter.png)

我们正在调用名为 **getDAMFolderAssets** 和以表单形式传递DAM文件夹资产列表的路径。

## 后续步骤

[组合选定资产](./assemble-selected-newsletters.md)
