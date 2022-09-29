---
title: 具有JSON模式和数据的AEM Forms[Part4]
seo-title: AEM Forms with JSON Schema and Data[Part4]
description: 多部分教程，用于指导您完成使用JSON模式创建自适应表单以及查询提交数据时涉及的步骤。
seo-description: Multi-Part tutorial to walk you through the steps involved in creating Adaptive Form with JSON schema and querying the submitted data.
feature: Adaptive Forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: a8d8118d-f4a1-483f-83b4-77190f6a42a4
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '442'
ht-degree: 0%

---

# 查询提交的数据


下一步是查询提交的数据并以表格形式显示结果。 为完成此操作，我们使用以下软件：

[QueryBuilder](https://querybuilder.js.org/)  — 用于创建查询的UI组件

[数据表](https://datatables.net/) — 以表格形式显示查询结果。

构建以下UI以允许查询提交的数据。 只有在JSON架构中标记为必需的元素才可用于查询。 在以下屏幕截图中，我们正在查询所有提交，其中投放头为短信。

用于查询已提交数据的示例UI不使用QueryBuilder中提供的所有高级功能。 我们鼓励你自己试试。

![查询生成器](assets/querybuilderui.gif)

>[!NOTE]
>
>本教程的当前版本不支持查询多个列。

选择表单以执行查询时，将对GET调用 **/bin/getdatakeysfromschema**. 此GET调用会返回与表单架构关联的必填字段。 然后，查询生成器的下拉列表中会填充必填字段，以便您生成查询。

以下代码片段对JSONSchemaOperations服务的getRequiredColumnsFromSchema方法进行了调用。 我们将架构的属性和必需元素传递到此方法调用。 此函数调用返回的数组随后用于填充查询生成器下拉列表

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

单击GetResult按钮时，将向 **&quot;/bin/querydata&quot;**. 我们通过查询参数将QueryBuilder UI构建的查询传递到Servlet。 然后，Servlet将此查询按摩到SQL查询中，以用于查询数据库。 例如，如果您搜索以检索名为“Mouse”的所有产品，则查询生成器查询字符串为 `$.productname = 'Mouse'`. 然后，此查询将转换为以下内容

选择 &#42; 从aemformswithjson 。  JSON_EXTRACT(formsubmissions .formdata，&quot;$.productName &quot;)= &#39;Mouse&#39;的表单提交

然后，返回此查询的结果以填充UI中的表。

要在本地系统上运行此示例，请执行以下步骤

1. [确保已执行此处提到的所有步骤](part2.md)
1. [使用AEM包管理器导入Dashboardv2.zip。](assets/dashboardv2.zip) 此包包含所有必需的包、配置设置、自定义提交和用于查询数据的示例页。
1. 使用示例json模式创建自适应表单
1. 配置自适应表单以提交到“customsubmithelpx”自定义提交操作
1. 填写表格并提交
1. 将您的浏览器指向 [dashboard.html](http://localhost:4502/content/AemForms/dashboard.html)
1. 选择表单并执行简单查询
