---
title: 使用最新原型更新云服务项目
description: 使用最新原型更新AEM Forms云服务项目
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
topic: Development
feature: AEM Project Archetype
jira: KT-9534
exl-id: c2cd9c52-6f00-4cfe-a972-665093990e5d
duration: 67
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '322'
ht-degree: 0%

---

# 从旧aem原型迁移

要使用最新的maven原型更新您现有的AEM Forms项目，您必须手动将代码/配置等从旧项目复制到新项目。

执行以下步骤，将使用原型30创建的项目迁移到原型33项目

## 使用最新原型创建maven项目

* 打开命令提示符并导航到c：\cloudmanager
* 使用最新原型创建maven项目。
* 在命令提示符窗口中复制并粘贴[文本文件](assets/creating-maven-project.txt)的内容。 您可能需要根据[最新版本](https://github.com/adobe/aem-project-archetype/releases)更改DarchetypeVersion=33。 原型33包括新的AEM Forms主题。
由于我们正在cloudmanager文件夹中创建新的maven项目，该文件夹已具有aem-banking-application项目，因此您应将&#x200B;**DartifactId**&#x200B;从aem-banking-application更改为其他内容。 本文使用了aem-banking-application1。

>[!NOTE]
>
>如果按原样部署此新项目，则云服务实例将没有HandleFormSubmission和SubmitToAEMServlet。 这是因为每次使用Cloud Manager部署项目时，`/apps`文件夹下的任何内容都将被删除和覆盖。

## 复制您的Java代码

成功创建项目后，您可以开始将代码/配置等从旧项目复制到此新项目

* 从```C:\CloudManager\aem-banking-application\core\src\main\java\com\aem\bankingapplication\core\servlets```复制HandleFormSubmission servlet
到
  ```C:\CloudManager\aem-banking-application1\core\src\main\java\com\aem\bankingapplication\core\servlets```

* 复制自定义提交来源
  从aem-banking-application到aem-banking-application1项目的```C:\CloudManager\aem-banking-application\ui.apps\src\main\content\jcr_root\apps\bankingapplication\SubmitToAEMServlet```

* 将新项目导入IntelliJ

* 更新aem-banking-application1项目的ui.apps模块中的filter.xml，以包含以下行
  ```<filter root="/apps/bankingapplication/SubmitToAEMServlet"/>```

将所有代码复制到新项目后，您可以将此项目推送到Cloud Manager。

>[!NOTE]
>
>要将内容(自适应Forms、表单数据模型等)同步到新项目中，您必须在IntelliJ项目中创建相应的文件夹结构，然后使用repo工具的“获取”命令将IntelliJ项目与AEM实例同步。
