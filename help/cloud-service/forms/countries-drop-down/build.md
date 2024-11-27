---
title: 构建、部署和测试国家/地区组件
description: 生成、部署和测试
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
feature: Adaptive Forms
badgeVersions: label="AEM Formsas a Cloud Service" before-title="false"
jira: KT-16517
source-git-commit: f9a1fb40aabb6fdc1157e1f2576f9c0d9cf1b099
workflow-type: tm+mt
source-wordcount: '209'
ht-degree: 0%

---

# 构建、部署和测试国家/地区组件

要生成所有模块并将`all`包部署到AEM的本地实例，请在项目根目录中运行以下命令：

```mvn clean install -PautoInstallSinglePackage```

## 测试组件

要将国家/地区组件集成到AEM Forms Cloud Ready实例中并对其进行配置，请执行以下步骤：

* 提取[国家/地区](assets/countries.zip) zip文件的内容。 每个文件都应包含特定大陆的数据。
* 在content/dam/corecomponent.This下上传json文件是代码查找json文件的位置。如果要将JSON文件存储在其他位置，则需要更新CountriesDropDownImpl类中的Java代码。 具体来说，请更新加载JSON文件的init()方法中的路径。例如，如果您要将JSON文件存储在content/dam/mydata/中，请像这样更新路径

```java
String jsonPath = "/content/dam/mydata/" + getContinent() + ".json"; // Update path accordingly
```

* 登录AEM Forms Cloud Ready实例
* 创建自适应表单并将国家/地区组件拖放到表单上
* 使用对话框编辑器配置国家/地区组件并设置包括非洲大陆在内的各种属性
  ![内容](assets/select-continent.png)
* 预览表单并确保国家/地区下拉列表按预期工作

