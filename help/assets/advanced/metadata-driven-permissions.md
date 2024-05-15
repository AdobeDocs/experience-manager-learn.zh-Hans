---
title: AEM Assets中的元数据驱动权限
description: 元数据驱动权限是一项功能，用于根据资源元数据属性而不是文件夹结构限制访问。
version: Cloud Service
topic: Administration, Security
feature: User and Groups
role: Admin
level: Intermediate
jira: KT-13757
doc-type: Tutorial
last-substantial-update: 2024-05-03T00:00:00Z
exl-id: 57478aa1-c9ab-467c-9de0-54807ae21fb1
duration: 158
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '738'
ht-degree: 0%

---

# 元数据驱动的权限{#metadata-driven-permissions}

元数据驱动权限是一项功能，可用于允许根据资源元数据属性而不是文件夹结构对AEM Assets Author做出访问控制决策。 利用此功能，您可以定义访问控制策略以评估属性，例如资源状态、类型或您定义的任何自定义元数据属性。

我们来看一个示例。 创意人员将其工作上传到AEM Assets中的营销活动相关文件夹，它可能是一个尚未批准使用的正在处理的资源。 我们希望确保营销人员仅看到此营销活动的已批准资产。 我们可以使用元数据属性来指示资产已获得批准并且可供营销人员使用。

## 工作原理

启用元数据驱动权限涉及定义哪些资产元数据属性将驱动访问限制，例如“状态”或“品牌”。 然后，这些属性可用于创建访问控制条目，以指定哪些用户组有权访问具有特定属性值的资源。

## 先决条件

要设置元数据驱动权限，需要访问更新到最新版本的AEMas a Cloud Service环境。

## OSGi配置 {#configure-permissionable-properties}

要实施元数据驱动权限，开发人员必须将OSGi配置部署到AEMas a Cloud Service，该配置使特定资源元数据属性能够增强元数据驱动权限。

1. 确定将用于访问控制的资源元数据属性。 属性名称是资产的 `jcr:content/metadata` 资源。 在我们的案例中，它将是一个名为 `status`.
1. 创建OSGi配置 `com.adobe.cq.dam.assetmetadatarestrictionprovider.impl.DefaultRestrictionProviderConfiguration.cfg.json` 在您的AEM Maven项目中。
1. 将以下JSON粘贴到创建的文件中：

   ```json
   {
     "restrictionPropertyNames":[
       "status",
       "brand"
     ],
     "enabled":true
   }
   ```

1. 将属性名称替换为所需的值。

## 重置基本资源权限

在添加基于限制的访问控制条目之前，应添加新的顶级条目，以首先拒绝对需要评估资产（例如“参与者”或类似者）权限的所有组的读取访问：

1. 导航至 __工具→安全→权限__ screen
1. 选择 __参与者__ 组（或所有用户组所属的其他自定义组）
1. 单击 __添加ACE__ 在屏幕右上角
1. 选择 `/content/dam` 对象 __路径__
1. 输入 `jcr:read` 对象 __权限__
1. 选择 `Deny` 对象 __权限类型__
1. 在限制下，选择 `rep:ntNames` 并输入 `dam:Asset` 作为 __限制值__
1. 单击 __保存__

![拒绝访问](./assets/metadata-driven-permissions/deny-access.png)

## 按元数据授予对资源的访问权限

现在可以添加访问控制条目，以根据 [已配置资源元数据属性值](#configure-permissionable-properties).

1. 导航至 __工具→安全→权限__ screen
1. 选择应具有资产访问权限的用户组
1. 单击 __添加ACE__ 在屏幕右上角
1. 选择 `/content/dam` （或子文件夹） __路径__
1. 输入 `jcr:read` 对象 __权限__
1. 选择 `Allow` 对象 __权限类型__
1. 下 __限制__，选择其中一项 [在OSGi配置中配置的资源元数据属性名称](#configure-permissionable-properties)
1. 在中输入所需的元数据属性值 __限制值__ 字段
1. 单击 __+__ 图标，以将限制添加到访问控制条目
1. 单击 __保存__

![允许访问](./assets/metadata-driven-permissions/allow-access.png)

## 元数据驱动的权限生效

示例文件夹包含几个资产。

![管理员视图](./assets/metadata-driven-permissions/admin-view.png)

在配置权限并相应地设置资产元数据属性后，用户（在此例中为“营销人员用户”）将只看到已批准的资产。

![营销人员视图](./assets/metadata-driven-permissions/marketeer-view.png)

## 优点和注意事项

元数据驱动型权限的优点包括：

- 根据特定属性对资源访问权限进行细粒度控制。
- 将访问控制策略与文件夹结构分离，允许更灵活的资产组织。
- 能够根据多个元数据属性定义复杂的访问控制规则。

>[!NOTE]
>
> 请务必注意：
> 
> - 使用根据限制评估元数据属性 __字符串相等__ (`=`)(不支持其他数据类型或运算符，因为大于(`>`)或日期属性)
> - 要允许限制属性有多个值，可以通过从“选择类型”下拉列表中选择相同的属性并输入新的限制值(例如， `status=approved`， `status=wip`)，然后单击“+”将限制添加到条目
> ![允许多个值](./assets/metadata-driven-permissions/allow-multiple-values.png)
> - __AND限制__ 通过带有不同属性名称(例如， `status=approved`， `brand=Adobe`)将被评估为AND条件，即选定的用户组将被授予对以下资产的读取访问权限： `status=approved AND brand=Adobe`
> ![允许多项限制](./assets/metadata-driven-permissions/allow-multiple-restrictions.png)
> - __OR限制__ 通过添加具有元数据属性限制的新访问控制条目来支持，将为条目建立OR条件，例如，具有限制的单个条目 `status=approved` 和单个条目 `brand=Adobe` 将被评估为 `status=approved OR brand=Adobe`
> ![允许多项限制](./assets/metadata-driven-permissions/allow-multiple-aces.png)
