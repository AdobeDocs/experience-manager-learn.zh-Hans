---
title: 创建可单击的图像组件
description: 在AEM Forms as a Cloud Service中创建可单击的图像组件
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-15968
exl-id: b635f171-775d-480e-bf7a-c92ab4af0aee
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '223'
ht-degree: 1%

---

# 创建组件

本文假定您具备为AEM Forms CS进行开发的一些经验。还假定您创建了一个AEM Forms原型项目。

在IntelliJ或您选择的任何其他IDE中打开您的AEM Forms项目。 在下创建一个名为svg的新节点

```
apps\corecomponent\components\adaptiveForm
```

>[!NOTE]
>
> ``corecomponent``是在创建Maven项目时提供的appId。 此appId在您的环境中可能不同。


## 创建.content.xml文件

在svg节点下创建名为.content.xml的文件。 将以下内容添加到新创建的文件。 您可以根据要求更改jcr：description、jcr：title和componentGroup。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:sling="http://sling.apache.org/jcr/sling/1.0"
    jcr:description="USA MAP"
    jcr:primaryType="cq:Component"
    jcr:title="USA MAP"
    sling:resourceSuperType="wcm/foundation/components/responsivegrid"
    componentGroup="CustomCoreComponent - Adaptive Form"/>
```

## 创建svg.html

创建名为svg.html的文件。 此文件将呈现USA map的SVG。将[svg.html](assets/svg.html)的内容复制到新创建的文件中。 您复制的是美国地图的SVG。 保存文件。

## 部署项目

将项目部署到本地云就绪实例以测试组件。

要部署项目，需要在命令提示符窗口中导航到项目的根文件夹，然后执行以下命令。

```
mvn clean install -PautoInstallSinglePackage
```

这会将该项目部署到本地AEM Forms实例，并且组件将包含在您的自适应表单中

![usa-map](./assets/usa-map.png)
