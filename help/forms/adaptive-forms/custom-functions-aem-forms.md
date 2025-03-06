---
title: AEM Forms中的自定义函数
description: 在自适应表单中创建和使用自定义函数
feature: Adaptive Forms
version: 6.5
topic: Development
role: User
level: Beginner
jira: KT-9685
exl-id: 07fed661-0995-41ab-90c4-abde35a14a4c
last-substantial-update: 2021-06-09T00:00:00Z
duration: 286
source-git-commit: a72f533b36940ce735d5c01d1625c6f477ef4850
workflow-type: tm+mt
source-wordcount: '319'
ht-degree: 0%

---

# 自定义函数

AEM Forms 6.5引入了用于定义JavaScript函数的功能，这些函数可用于使用规则编辑器定义复杂的业务规则。
AEM Forms提供了许多现成的此类自定义函数，但您需要定义自己的自定义函数并在多个表单中使用它们。

要定义您的第一个自定义函数，请执行以下步骤：
* [登录crx](http://localhost:4502/crx/de/index.jsp#/apps/experience-league/clientlibs)
* 在名为experience-league的应用程序下创建新文件夹（此文件夹名称可以是您选择的名称）
* 保存更改。
* 在experience-league文件夹下，创建一个名为clientlibs的cq：ClientLibraryFolder类型的新节点。
* 选择新创建的clientlibs文件夹并添加allowProxy和类别属性，如屏幕快照中所示，并保存更改。

![client-lib](assets/custom-functions.png)
* 在&#x200B;**clientlibs**&#x200B;文件夹下创建名为&#x200B;**js**&#x200B;的文件夹
* 在&#x200B;**js**&#x200B;文件夹下创建名为&#x200B;**functions.js**&#x200B;的文件
* 在&#x200B;**clientlibs**&#x200B;文件夹下创建名为&#x200B;**js.txt**&#x200B;的文件。 保存更改。
* 您的文件夹结构应类似于下面的屏幕快照。

![规则编辑器](assets/folder-structure.png)

* 双击functions.js以打开编辑器。
将以下代码复制到functions.js中并保存更改。

```javascript
/**
* Get List of County names
* @name getCountyNamesList Get list of county names
* @returns {string[]} An array of county names
 */
function getCountyNamesList()
{
    return ["Santa Clara", "Alameda", "Buxor", "Contra Costa", "Merced"];

}
/**
* Covert UTC to Local Time
* @name convertUTC Convert UTC Time to Local Time
* @param {string} strUTCString in Stringformat
* @return {string}
*/
function convertUTC(strUTCString)
{
    var dt = new Date(strUTCString);
    console.log(dt.toLocaleString());
    return dt.toLocaleString();
}
```

有关对javascript函数添加注释的更多详细信息，请[参阅jsdoc](https://jsdoc.app/index.html)。
上述代码具有两个功能：
**getCountyNamesList** — 返回字符串数组
**convertUTC** — 将UTC时间戳转换为本地时区

打开js.txt并粘贴以下代码并保存更改。

```javascript
#base=js
functions.js
```

#base=js行指定JavaScript文件所在的目录。
以下行指示JavaScript文件相对于基础位置的位置。

如果您在创建自定义函数时遇到问题，请随时[下载此包](assets/custom-functions.zip)并将其安装在您的AEM实例中。

## 使用自定义函数

以下视频将指导您完成在自适应表单的规则编辑器中使用自定义函数所涉及的步骤
>[!VIDEO](https://video.tv.adobe.com/v/340305?quality=12&learn=on)
