---
title: 添加自定义列
description: 添加自定义列以显示工作流的附加数据
feature: Adaptive Forms
doc-type: article
version: Experience Manager 6.5
jira: KT-5830
topic: Development
role: Developer
level: Experienced
exl-id: 0b141b37-6041-4f87-bd50-dade8c0fee7d
duration: 75
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '276'
ht-degree: 0%

---

# 添加自定义列

要在收件箱中显示工作流数据，我们需要在工作流中定义和填充变量。 在将任务分配给用户之前，需要设置变量的值。 为了让您开一个好头，我们提供了准备好部署在AEM服务器上的示例工作流。

* [登录AEM](http://localhost:4502/crx/de/index.jsp)
* [导入审核工作流](assets/review-workflow.zip)
* [审阅工作流](http://localhost:4502/editor.html/conf/global/settings/workflow/models/reviewworkflow.html)

此工作流定义了两个变量（isMarried和income），其值使用设置变量组件进行设置。 这些变量可用作要添加到AEM收件箱中的列

## 创建服务

对于每个需要显示在收件箱中的列，我们需要编写一项服务。 以下服务允许我们添加列以显示isMarried变量的值

```java
import com.adobe.cq.inbox.ui.column.Column;
import com.adobe.cq.inbox.ui.column.provider.ColumnProvider;

import com.adobe.cq.inbox.ui.InboxItem;
import org.osgi.service.component.annotations.Component;

import java.util.Map;

/**
 * This provider does not require any sightly template to be defined.
 * It is used to display the value of 'ismarried' workflow variable as a column in inbox
 */
@Component(service = ColumnProvider.class, immediate = true)
public class MaritalStatusProvider implements ColumnProvider {@Override
public Column getColumn() {
return new Column("married", "Married", Boolean.class.getName());
}

// Return True or False if 'ismarried' is set. Else returns null
private Boolean isMarried(InboxItem inboxItem) {
Boolean ismarried = null;

Map metaDataMap = inboxItem.getWorkflowMetadata();
if (metaDataMap != null) {
if (metaDataMap.containsKey("isMarried")) {
    ismarried = (Boolean) metaDataMap.get("isMarried");
}
}

return ismarried;
}

@Override
public Object getValue(InboxItem inboxItem) {
return isMarried(inboxItem);
}
}
```

>[!NOTE]
>
>您需要在项目中包含AEM 6.5.5 Uber.jar，以上代码才能正常工作

![uber-jar](assets/uber-jar.PNG)

## 在您的服务器上测试

* [登录到AEM Web控制台](http://localhost:4502/system/console/bundles)
* [部署和启动收件箱自定义捆绑包](assets/inboxcustomization.inboxcustomization.core-1.0-SNAPSHOT.jar)
* [打开您的收件箱](http://localhost:4502/aem/inbox)
* 单击&#x200B;_创建_&#x200B;按钮旁边的&#x200B;_列表视图_&#x200B;图标以打开Admin Control
* 将“已婚”列添加到收件箱并保存更改
* [转到FormsAndDocuments UI](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* [通过从&#x200B;_创建_&#x200B;菜单中选择&#x200B;_文件上传_&#x200B;导入示例表单](assets/snap-form.zip)
* [预览表单](http://localhost:4502/content/dam/formsanddocuments/snapform/jcr:content?wcmmode=disabled)
* 选择&#x200B;_婚姻状况_并提交表单
  [查看收件箱](http://localhost:4502/aem/inbox)

提交表单将触发工作流，并且任务会分配给“管理员”用户。 您应会在“已婚”列下看到一个值，如此屏幕快照中所示

![已婚列](assets/married-column.PNG)

## 后续步骤

[显示已婚列](./use-sightly-template.md)
