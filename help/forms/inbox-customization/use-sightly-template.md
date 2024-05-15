---
title: 使用Sightly模板显示收件箱数据
description: 添加自定义列以显示使用Sightly模板的工作流的附加数据
feature: Adaptive Forms
doc-type: article
version: 6.5
jira: KT-5830
topic: Development
role: Developer
level: Experienced
exl-id: d09b46ed-3516-44cf-a616-4cb6e9dfdf41
duration: 68
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '256'
ht-degree: 0%

---

# 使用Sightly模板显示收件箱数据

您可以使用Sightly模板来格式化要显示在收件箱列中的数据。 在本例中，我们将根据收入列的值显示coral-ui图标。 以下屏幕截图显示了收入列中的图标的使用
![收入图标](assets/income-column.PNG)

[美观的模板](assets/sightly-template.zip) 用于显示自定义coral ui图标的视图作为本文的一部分提供。

## Sightly模板

以下是sightly模板。 模板中的代码根据收入显示图标。 这些图标作为 [coral ui图标库](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html#availableIcons) 和AEM一起来的。

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

第12行将列与sightly模板关联

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

## 在您的服务器上测试

>[!NOTE]
>
>本文假定您已安装 [示例工作流](assets/review-workflow.zip) 和 [示例表单](assets/snap-form.zip) 从 [上一篇文章](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/inbox-customization/add-married-column.html) 本系列中。

* [以管理员用户身份登录crx](http://localhost:4502/crx/de/index.jsp)
* [导入sightly模板](assets/sightly-template.zip)
* [登录到AEM Web控制台](http://localhost:4502/system/console/bundles)
* [部署和启动收件箱自定义捆绑包](assets/income-column-customization.jar)
* [打开收件箱](http://localhost:4502/aem/inbox)
* 通过单击“创建”按钮旁边的“列表视图”来打开Admin Control
* 将收入列添加到收件箱并保存更改
* [预览表单](http://localhost:4502/content/dam/formsanddocuments/snapform/jcr:content?wcmmode=disabled)
* 选择 _婚姻状况_ 并提交表单
* [查看收件箱](http://localhost:4502/aem/inbox)

提交表单将触发工作流，并且任务会分配给“管理员”用户。 您应会在收入列下看到相应的图标
