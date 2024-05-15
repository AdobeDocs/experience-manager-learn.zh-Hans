---
title: 使用最新原型更新云服务项目
description: 使用最新原型更新AEM Forms云服务项目
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
feature: AEM Project Archetype
jira: KT-9534
exl-id: c2cd9c52-6f00-4cfe-a972-665093990e5d
duration: 67
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
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
* 复制并粘贴的内容 [文本文件](assets/creating-maven-project.txt) 命令提示符窗口中。 您可能需要更改DarchetypeVersion=33，具体取决于 [最新版本](https://github.com/adobe/aem-project-archetype/releases). 原型33包括新的AEM Forms主题。
由于我们在cloudmanager文件夹中创建的新maven项目已经具有aem-banking-application项目，因此您应该更改 **DartifactId** 从aem-banking-application到其他应用程序。 本文使用了aem-banking-application1。

>[!NOTE]
>
>如果按原样部署此新项目，则云服务实例将没有HandleFormSubmission和SubmitToAEMServlet。 这是因为每次使用Cloud Manager部署项目时， `/apps` 文件夹将被删除和覆盖。

## 复制您的Java代码

成功创建项目后，您可以开始将代码/配置等从旧项目复制到此新项目

* 从以下位置复制HandleFormSubmission servlet ```C:\CloudManager\aem-banking-application\core\src\main\java\com\aem\bankingapplication\core\servlets```
到
  ```C:\CloudManager\aem-banking-application1\core\src\main\java\com\aem\bankingapplication\core\servlets```

* 复制自定义提交来源
  ```C:\CloudManager\aem-banking-application\ui.apps\src\main\content\jcr_root\apps\bankingapplication\SubmitToAEMServlet``` 从aem-banking-application到aem-banking-application1项目

* 将新项目导入IntelliJ

* 更新aem-banking-application1项目的ui.apps模块中的filter.xml，以包含以下行
  ```<filter root="/apps/bankingapplication/SubmitToAEMServlet"/>```

将所有代码复制到新项目后，您可以将此项目推送到Cloud Manager。

>[!NOTE]
>
>要将内容(自适应Forms、表单数据模型等)同步到新项目中，您必须在IntelliJ项目中创建相应的文件夹结构，然后使用repo工具的“获取”命令将IntelliJ项目与AEM实例同步。
