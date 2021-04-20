---
title: AEM Forms，含JSON模式和数据[Part4]
seo-title: AEM Forms，含JSON模式和数据[Part4]
description: 多部分教程，用于指导您完成创建带有JSON模式的自适应表单和查询提交数据所涉及的步骤。
seo-description: 多部分教程，用于指导您完成创建带有JSON模式的自适应表单和查询提交数据所涉及的步骤。
feature: Adaptive Forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4,6.5
topic: Development
role: Developer
level: Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '480'
ht-degree: 1%

---


# 查询已提交的数据


下一步是查询提交的数据并以表格形式显示结果。 为此，我们将使用以下软件

[QueryBuilder](https://querybuilder.js.org/)  — 用于创建查询的UI组件

[查询表](https://datatables.net/) — 以表格形式显示结果。

以下UI是为启用查询已提交数据而构建的。 只有在JSON模式中标记为必需的元素才可用于查询。 在以下屏幕截图中，我们正在查询所有提交项，其中deliverypref为SMS。

查询提交数据的示例UI不使用QueryBuilder中提供的所有高级功能。 我鼓励你自己试试。

![查询生成器](assets/querybuilderui.gif)

>[!NOTE]
>
>本教程的当前版本不支持查询多列。

选择表单以执行查询时，将对&#x200B;**/bin/getdatakeysfromschema**&#x200B;进行GET调用。 此GET调用返回与表单的模式关联的必填字段。 然后，在QueryBuilder的下拉列表中填充必填字段，以便您构建查询。

以下代码段调用JSONSchemaOperations服务的getRequiredColumnsFromSchema方法。 我们将模式的属性和必需元素传递给此方法调用。 随后，此函数调用返回的数组用于填充查询 builder下拉列表

```java
public JSONArray getData(String formName) throws SQLException, IOException {

  org.json.JSONArray arrayOfDataKeys = new org.json.JSONArray();
  JSONObject jsonSchema = jsonSchemaOperations.getJSONSchemaFromDataBase(formName);
  Map<String, String> refKeys = new HashMap<String, String>();

  try {
   JSONObject properties = jsonSchema.getJSONObject("properties");
   JSONArray requiredFields = jsonSchema.has("required") ? jsonSchema.getJSONArray("required") : null;
   jsonSchemaOperations.getRequiredColumnsFromSchema(properties, arrayOfDataKeys, "", jsonSchema, refKeys,
     requiredFields);
  } catch (JSONException e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  }
  return arrayOfDataKeys;

 }
```

单击GetResult按钮时，将对&#x200B;**&quot;/bin/querydata&quot;**&#x200B;发出Get调用。 我们通过查询参数将QueryBuilder UI构建的查询传递给servlet。 然后，Servlet将此查询按摩到SQL查询中，以用于查询数据库。 例如，如果要搜索以检索名为“Mouse”的所有产品，查询 Builder查询字符串将为$.productname = &#39;Mouse&#39;。 此查询随后将转换为以下内容

从aemformswithjson中选择*。  formsubmissions where JSON_EXTRACT(formsubmissions .formdata，&quot;$.productName &quot;)= &#39;Mouse&#39;

然后返回此查询的结果以填充UI中的表。

要在本地系统上运行此示例，请执行以下步骤

1. [确保您遵循了此处提到的所有步骤](part2.md)
1. [使用AEM包管理器导入Dashboardv2.zip。](assets/dashboardv2.zip) 此包包含所有必需的包、配置设置、自定义提交和示例页以查询数据。
1. 使用示例json模式创建自适应表单
1. 配置自适应表单以提交到“customsubmithelpx”自定义提交操作
1. 填写表单并提交
1. 将浏览器指向[仪表板.html](http://localhost:4502/content/AemForms/dashboard.html)
1. 选择表单并执行简单查询

