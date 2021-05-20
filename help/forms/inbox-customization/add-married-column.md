---
title: 收件箱自定义
description: 添加自定义列以显示工作流的其他数据
feature: 自适应表单
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5.5
kt: 5830
topic: 开发
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '309'
ht-degree: 0%

---


# 添加自定义列

要在收件箱中显示工作流数据，我们需要在工作流中定义和填充变量。 在将任务分配给用户之前，需要设置变量的值。 为了抢先一步，我们提供了示例工作流，可供在您的AEM服务器上部署。

* [登录AEM](http://localhost:4502/crx/de/index.jsp)
* [导入审阅工作流](assets/review-workflow.zip)
* [查看工作流](http://localhost:4502/editor.html/conf/global/settings/workflow/models/reviewworkflow.html)

此工作流定义了两个变量（isFrimed和income），其值使用设置的变量组件进行设置。 这些变量将作为列提供，以添加到AEM收件箱

## 创建服务

对于我们需要在收件箱中显示的每一列，我们需要编写一项服务。 以下服务允许我们添加一列以显示isFrimed变量的值

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
>您需要在项目中包含AEM 6.5.5 Uber.jar ，才能使上述代码正常工作

![uber-jar](assets/uber-jar.PNG)

## 在服务器上测试

* [登录AEM Web控制台](http://localhost:4502/system/console/bundles)
* [部署和启动收件箱自定义包](assets/inboxcustomization.inboxcustomization.core-1.0-SNAPSHOT.jar)
* [打开您的收件箱](http://localhost:4502/aem/inbox)
* 单击&#x200B;_创建_&#x200B;按钮旁边的&#x200B;_列表视图_&#x200B;图标以打开Admin Control
* 将“已婚”列添加到收件箱并保存您所做的更改
* [转到表单和文档UI](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* [通过选择从创](assets/snap-form.zip) 建菜单中 _上传_ 文件来导 __ 入示例
* [预览表单](http://localhost:4502/content/dam/formsanddocuments/snapform/jcr:content?wcmmode=disabled)
* 选择&#x200B;_婚姻状态_并提交表单
   [查看收件箱](http://localhost:4502/aem/inbox)

提交表单将触发工作流，并且会向“管理员”用户分配任务。 您应会在“已婚”列下看到一个值，如此屏幕快照中所示

![已婚人士](assets/married-column.PNG)
