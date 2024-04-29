---
title: AEM Assets中的元数据驱动权限
description: 元数据驱动权限是一项功能，用于根据资源元数据属性而不是文件夹结构限制访问。
version: Cloud Service
topic: Administration, Security
feature: User and Groups
role: Admin
level: Intermediate
jira: KT-13757
thumbnail: xx.jpg
doc-type: Tutorial
exl-id: 57478aa1-c9ab-467c-9de0-54807ae21fb1
source-git-commit: 03cb7ef0cf79a21ec1b96caf6c11e6f5119f777c
workflow-type: tm+mt
source-wordcount: '682'
ht-degree: 0%

---

# 元数据驱动的权限{#metadata-driven-permissions}

元数据驱动权限是一项功能，可用于允许根据资源元数据属性而不是文件夹结构对AEM Assets Author做出访问控制决策。 利用此功能，您可以定义访问控制策略以评估属性，例如资源状态、类型或您定义的任何自定义元数据属性。

我们来看一个示例。 创意人员将其工作上传到AEM Assets中的营销活动相关文件夹，它可能是一个尚未批准使用的正在处理的资源。 我们希望确保营销人员仅看到此营销活动的已批准资产。 我们可以使用元数据属性来指示资产已获得批准并且可供营销人员使用。

## 工作原理

启用元数据驱动权限涉及定义哪些资产元数据属性将驱动访问限制，例如“状态”或“品牌”。 然后，这些属性可用于创建访问控制条目，以指定哪些用户组有权访问具有特定属性值的资源。

## 先决条件

要设置元数据驱动权限，需要访问更新到最新版本的AEMas a Cloud Service环境。


## 开发步骤

要实施元数据驱动的权限：

1. 确定将用于访问控制的资源元数据属性。 在我们的案例中，它将是一个名为 `status`.
1. 创建OSGi配置 `com.adobe.cq.dam.assetmetadatarestrictionprovider.impl.DefaultRestrictionProviderConfiguration.cfg.json` 在您的项目中。
1. 将以下JSON粘贴到创建的文件中

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


在添加基于限制的访问控制条目之前，应添加新的顶级条目，以首先拒绝对需要评估资产（例如“参与者”或类似者）权限的所有组的读取访问：

1. 导航到工具→安全→权限屏幕
1. 选择“参与者”组（或其他所有用户组所属的自定义组）
1. 单击屏幕右上角的“添加ACE”
1. 为路径选择/content/dam
1. 为权限输入jcr：read
1. 选择拒绝权限类型
1. 在“限制”下，选择rep：ntNames并输入dam：Asset作为限制值
1. 单击“保存”

![拒绝访问](./assets/metadata-driven-permissions/deny-access.png)

现在可以添加访问控制条目，以根据资产元数据属性值向用户组授予读取权限。

1. 导航到工具→安全→权限屏幕
1. 选择所需的组
1. 单击屏幕右上角的“添加ACE”
1. 为Path选择/content/dam（或子文件夹）
1. 为权限输入jcr：read
1. 选择“允许权限类型”
1. 在限制下，选择一个已配置的资产元数据属性名称（此处将包含OSGi配置中定义的属性）
1. 在限制值字段中输入所需的元数据属性值
1. 单击“+”图标以将限制添加到访问控制条目
1. 单击“保存”

![允许访问](./assets/metadata-driven-permissions/allow-access.png)

示例文件夹包含几个资产。

![管理员视图](./assets/metadata-driven-permissions/admin-view.png)

在配置权限并相应地设置资产元数据属性后，用户（在此例中为“营销人员用户”）将只看到已批准的资产。

![营销人员视图](./assets/metadata-driven-permissions/marketeer-view.png)

## 优点和注意事项

元数据驱动型权限的好处包括：

- 根据特定属性对资源访问权限进行细粒度控制。
- 将访问控制策略与文件夹结构分离，允许更灵活的资产组织。
- 能够根据多个元数据属性定义复杂的访问控制规则。

>[!NOTE]
>
> 请务必注意：
> 
> - 使用字符串等式根据限制评估元数据属性（其他尚不受支持的数据类型，如日期）
> - 要允许限制属性有多个值，可以通过从“选择类型”下拉列表中选择相同的属性并输入新的限制值(例如， `status=approved`， `status=wip`)，然后单击“+”将限制添加到条目
> ![允许多个值](./assets/metadata-driven-permissions/allow-multiple-values.png)
> - 在单个访问控制条目中具有不同的属性名称(例如， `status=approved`， `brand=Adobe`)将被评估为AND条件，即选定的用户组将被授予对以下资产的读取访问权限： `status=approved AND brand=Adobe`
> ![允许多项限制](./assets/metadata-driven-permissions/allow-multiple-restrictions.png)
> - 添加具有元数据属性限制的新访问控制条目将建立条目的OR条件，例如，具有限制的单个条目 `status=approved` 和单个条目 `brand=Adobe` 将被评估为 `status=approved OR brand=Adobe`
> ![允许多项限制](./assets/metadata-driven-permissions/allow-multiple-aces.png)
