---
title: 在AEM Forms中列出自定义资产类型
description: 在AEM Forms中列出自定义资产类型的第2部分
uuid: 6467ec34-e452-4c21-9bb5-504f9630466a
feature: Adaptive Forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 4b940465-0bd7-45a2-8d01-e4d640c9aedf
topic: Development
role: Developer
level: Experienced
exl-id: f221d8ee-0452-4690-a936-74bab506d7ca
last-substantial-update: 2019-07-10T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '593'
ht-degree: 0%

---

# 在AEM Forms中列出自定义资产类型 {#listing-custom-asset-types-in-aem-forms}

## 创建自定义模板 {#creating-custom-template}

为此，我们将创建一个自定义模板，以在同一页面上显示自定义资产类型和OOTB资产类型。 要创建自定义模板，请按照以下说明操作

1. 创建Sling:文件夹。 将其命名为“ myportalcomponent ”
1. 添加“fpContentType”属性。 将其值设置为“**/libs/fd/ fp/formTemplate”。**
1. 添加“title”属性，并将其值设置为“自定义模板”。 这是您将在搜索组件和Lister组件的下拉列表中看到的名称
1. 在此文件夹下创建“template.html”。 此文件将保存要设置样式的代码，并显示各种资产类型。

![appsfolder](assets/appsfolder_.png)

以下代码使用搜索和列表组件列出了各种类型的资产。 我们会为每种类型的资产创建单独的html元素，如data-type = &quot;videos&quot;标记所示。 对于“视频”的资产类型，我们使用 &lt;video> 元素来在内联播放视频。 对于“worddocuments”的资产类型，我们会使用不同的html标记。

```html
<div class="__FP_boxes-container __FP_single-color">
   <div  data-repeatable="true">
     <div class = "__FP_boxes-thumbnail" style="float:left;margin-right:20px;" data-type = "videos">
   <video width="400" controls>
       <source src="${path}" type="video/mp4">
    </video>
         <h3 class="__FP_single-color" title="${name}" tabindex="0">${name}</h3>
     </div>
     <div class="__FP_boxes-thumbnail" style="float:left;margin-right:20px;" data-type = "worddocuments">
       <a href="/assetdetails.html${path}" target="_blank">
           <img src ="/content/dam/worddocuments/worddocument.png/jcr:content/renditions/cq5dam.thumbnail.319.319.png"/>
          </a>
          <h3 class="__FP_single-color" title="${name}" tabindex="0">${name}</h3>
     </div>
  <div class="__FP_boxes-thumbnail" style="float:left;margin-right:20px;" data-type = "xfaForm">
       <a href="/assetdetails.html${path}" target="_blank">
           <img src ="${path}/jcr:content/renditions/cq5dam.thumbnail.319.319.png"/>
          </a>
          <h3 class="__FP_single-color" title="${name}" tabindex="0">${name}</h3>
                <a href="{formUrl}"><img src="/content/dam/html5.png"></a><p>

     </div>
  <div class="__FP_boxes-thumbnail" style="float:left;margin-right:20px;" data-type = "printForm">
       <a href="/assetdetails.html${path}" target="_blank">
           <img src ="${path}/jcr:content/renditions/cq5dam.thumbnail.319.319.png"/>
          </a>
          <h3 class="__FP_single-color" title="${name}" tabindex="0">${name}</h3>
                <a href="{pdfUrl}"><img src="/content/dam/pdf.png"></a><p>
     </div>
   </div>
</div>
```

>[!NOTE]
>
>第11行 — 请更改图像src以指向您在DAM中选择的图像。
>
>要在此模板中列出自适应Forms，请创建一个新div，并将其数据类型属性设置为“guide”。 您可以复制并粘贴其data-type=&quot;printForm&quot;的div，并将新复制的div的data-type设置为&quot;guide&quot;

## 配置搜索和制表器组件 {#configure-search-and-lister-component}

定义自定义模板后，我们现在必须将此自定义模板与“搜索和制表人”组件相关联。 将浏览器指向 [到此url ](http://localhost:4502/editor.html/content/AemForms/CustomPortal.html).

切换到设计模式，并配置段落系统，以在允许的组件组中包含搜索和制表器组件。 “搜索”和“制表人”组件是“文档服务”组的一部分。

切换到编辑模式，并将搜索和制表程序组件添加到ParSys中。

打开“搜索和制表人”组件的配置属性。 确保选中“资产文件夹”选项卡。 在搜索和列表组件中，选择要从中列出资产的文件夹。 为本文的目的，我已选择

* /content/dam/videosAndWordDocuments
* /content/dam/formsanddocuments/assettypes

![assetfolder](assets/selectingassetfolders.png)

选项卡。 在此，您将选择要在搜索组件和列表组件中显示资产的模板。

从下拉菜单中选择“自定义模板”，如下所示。

![搜索器](assets/searchandlistercomponent.gif)

配置要在门户中列出的资产类型。 要在“资产列表”中配置资产的选项卡类型，并配置资产类型，请执行以下操作： 在本例中，我们配置了以下类型的资产

1. MP4文件
1. Word文档
1. 文档（这是OOTB资产类型）
1. 表单模板（这是OOTB资产类型）

以下屏幕快照显示了为列表配置的资产类型

![assettypes](assets/assettypes.png)

现在，您已配置了搜索和制表人门户组件，接下来该看看制表人的行动了。 将浏览器指向 [到此url ](http://localhost:4502/content/AemForms/CustomPortal.html?wcmmode=disabled). 结果应类似于下图所示。

>[!NOTE]
>
>如果您的门户在发布服务器上列出了自定义资产类型，请确保您为“fd-service”用户向节点授予“读取”权限 **/apps/fd/fp/extensions/querybuilder**

![assettypes](assets/assettypeslistings.png)
[请使用包管理器下载并安装此包。](assets/customassettypekt1.zip) 其中包含示例mp4和word文档以及xdp文件，这些文件用作要使用搜索组件和Lister组件列出的资产类型
