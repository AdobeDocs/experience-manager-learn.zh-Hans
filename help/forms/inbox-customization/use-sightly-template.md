---
title: 收件箱自定义
description: 添加自定义列，以使用Sightly模板显示工作流的其他数据
feature: Adaptive Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5.5
kt: 5830
topic: Development
role: Developer
level: Experienced
source-git-commit: 0049c9fd864bd4dd4f8c33b1e40e94aad3ffc5b9
workflow-type: tm+mt
source-wordcount: '289'
ht-degree: 0%

---

# 使用Sightly模板显示收件箱数据

您可以使用Sightly模板来设置要显示在收件箱列中的数据的格式。 在本例中，我们将根据收入列的值显示coral-ui图标。 以下屏幕截图显示了收入列中图标的使用情况
![income-icons](assets/income-column.PNG)

[本文提](assets/sightly-template.zip) 供了用于显示自定义coral ui图标的美观模板。

## Sightly模板

以下是美观的模板。 模板中的代码会根据收入显示图标。 这些图标作为AEM附带的[coral ui图标库](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html#availableIcons)的一部分提供。

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

以下代码是用于显示收入列的服务实施。

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
>本文假定您已安装此系列中[上一篇文章](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/inbox-customization/add-married-column.html)中的[示例工作流](assets/review-workflow.zip)和[示例表单](assets/snap-form.zip)。

* [以管理员用户身份登录crx](http://localhost:4502/crx/de/index.jsp)
* [导入sightly模板](assets/sightly-template.zip)
* [登录AEM Web控制台](http://localhost:4502/system/console/bundles)
* [部署和启动收件箱自定义包](assets/income-column-customization.jar)
* [打开您的收件箱](http://localhost:4502/aem/inbox)
* 单击创建按钮旁边的列表视图以打开Admin Control
* 将收入列添加到收件箱并保存您所做的更改
* [预览表单](http://localhost:4502/content/dam/formsanddocuments/snapform/jcr:content?wcmmode=disabled)
* 选择&#x200B;_婚姻状态_&#x200B;并提交表单
* [查看收件箱](http://localhost:4502/aem/inbox)

提交表单将触发工作流，并且会向“管理员”用户分配任务。 您应会在收入列下看到相应的图标
