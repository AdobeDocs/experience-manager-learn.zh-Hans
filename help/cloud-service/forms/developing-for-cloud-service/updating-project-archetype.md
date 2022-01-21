---
title: 使用最新原型更新云服务项目
description: 使用最新原型更新AEM Forms云服务项目
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: cloud-service
topic: Development
kt: 9534
source-git-commit: cea9a9dc003b76369db1b7fedb9549062885258d
workflow-type: tm+mt
source-wordcount: '326'
ht-degree: 0%

---

# 从旧aem原型迁移

要使用最新的Maven原型更新现有AEM Forms项目，您必须手动将代码/配置等从旧项目复制到新项目。

按照以下步骤将使用原型30创建的项目迁移到原型33项目

## 使用最新原型创建Maven项目

* 打开命令提示符并导航到c:\cloudmanager
* 使用最新原型创建Maven项目。
* 复制并粘贴 [文本文件](assets/creating-maven-project.txt) 在命令提示符窗口中。 您可能需要根据 [最新版本](https://github.com/adobe/aem-project-archetype/releases). 原型33包括新的AEM Forms主题。
由于我们在cloudmanager文件夹中创建新的maven项目（该文件夹已具有aem银行应用程序项目），因此您应当更改 **DartifactId** 从aem银行应用程序到其他功能。 我已将aem-banking-application1用于本文。

>[!NOTE]
>
>如果您以云服务实例的形式部署此新项目，则将没有HandleFormSubmission和SubmitToAEMServlet。 这是因为每次您使用cloud manager部署项目时，应用程序文件夹下的所有内容都将被删除和覆盖。

## 复制Java代码

成功创建项目后，您可以开始将代码/配置等从旧项目复制到此新项目

* 从以下位置复制HandleFormSubmission Servlet ```C:\CloudManager\aem-banking-application\core\src\main\java\com\aem\bankingapplication\core\servlets```
to

   ```C:\CloudManager\aem-banking-application1\core\src\main\java\com\aem\bankingapplication\core\servlets```

* 从以下位置复制CustomSubmit
   ```C:\CloudManager\aem-banking-application\ui.apps\src\main\content\jcr_root\apps\bankingapplication\SubmitToAEMServlet``` 从aem-banking-application到aem-banking-application1项目

* 将新项目导入IntelliJ

* 更新aem-banking-application1项目ui.apps模块中的filter.xml以包含以下行
   ```<filter root="/apps/bankingapplication/SubmitToAEMServlet"/>```

将所有代码复制到新项目后，您可以将此项目推送到Cloud Manager。

>[!NOTE]
>
>要将内容(自适应Forms、表单数据模型等)同步到新项目，您必须在IntelliJ项目中创建相应的文件夹结构，然后使用存储库工具的“获取”命令将您的IntelliJ项目与AEM实例同步。
