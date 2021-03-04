---
title: 收件箱自定义
description: 添加自定义列，以使用美观的模板显示工作流程的其他数据
feature: 自适应表单
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5.5
kt: 5830
topic: 开发
role: 开发人员
level: 富有经验
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '296'
ht-degree: 1%

---

# 使用Sightly模板显示收件箱数据

您可以使用Sightly模板来设置要显示在收件箱列中的数据的格式。 在此示例中，我们将根据收入列的值显示coral-ui图标。 以下屏幕截图显示了收入列中图标的使用情况
![income-icons](assets/income-column.PNG)

[用于](assets/sightly-template.zip) 显示自定义coral ui图标的美观模板作为本文的一部分提供。

## Sightly模板

以下是美观的模板。 模板中的代码会根据收入显示图标。 这些图标是AEM附带的[coral ui图标库](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html#availableIcons)的一部分。

```java
<template data-sly-template.incomeTemplate="${@ item}>">
    <td is="coral-table-cell" class="payload-income-cell">
         <div data-sly-test="${(item.workflowMetadata && item.workflowMetadata.income)}" data-sly-set.income ="${item.workflowMetadata.income}">
                 <coral-icon icon="confidenceOne" size="M" data-sly-test="${income >=0 && income <10000}"></coral-icon>
                 <coral-icon icon="confidenceTwo" size="M" data-sly-test="${income >=10000 && income <100000}"></coral-icon>
                 <coral-icon icon="confidenceThree" size="M" data-sly-test="${income >=100000 && income <500000}"></coral-icon>
                 <coral-icon icon="confidenceFour" size="M" data-sly-test="${income >=500000}"></coral-icon>
          </div>
    </td>
</template>
```

## 服务实施

以下代码是用于显示收入列的服务实现。

第12行将列与Sightly模板关联

```java
import java.util.Map;
import org.osgi.service.component.annotations.Component;
import com.adobe.cq.inbox.ui.InboxItem;
import com.adobe.cq.inbox.ui.column.Column;
import com.adobe.cq.inbox.ui.column.provider.ColumnProvider;

@Component(service = ColumnProvider.class, immediate = true)
public class IncomeProvider implements ColumnProvider {
@Override
public Column getColumn() {

return new Column("income", "Income", String.class.getName(),"inbox/customization/column-templates.html", "incomeTemplate");
}

@Override
public Object getValue(InboxItem inboxItem) {
Object val = null;

Map workflowMetadata = inboxItem.getWorkflowMetadata();

if (workflowMetadata != null && workflowMetadata.containsKey("income"))
    val = workflowMetadata.get("income");

return val;
}
}
```

## 在服务器上测试

>[!NOTE]
>
>本文假定您已安装本系列中前一篇文章](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/inbox-customization/add-married-column.md)中的[示例工作流](assets/review-workflow.zip)和[示例表单](assets/snap-form.zip)。[

* [以管理员用户身份登录crx](http://localhost:4502/crx/de/index.jsp)
* [导入](assets/sightly-template.zip)
* [登录AEM Web控制台](http://localhost:4502/system/console/bundles)
* [部署和开始收件箱自定义包](assets/income-column-customization.jar)
* [打开您的收件箱](http://localhost:4502/aem/inbox)
* 单击“创建”按钮旁边的列表视图，打开Admin Control
* 将收入列添加到收件箱并保存更改
* [预览表单](http://localhost:4502/content/dam/formsanddocuments/snapform/jcr:content?wcmmode=disabled)
* 选择&#x200B;_婚姻状态_&#x200B;并提交表单
* [视图收件箱](http://localhost:4502/aem/inbox)

提交表单将触发工作流，并且任务会分配给“admin”用户。 您应在收入列下看到相应的图标
