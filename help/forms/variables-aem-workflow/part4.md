---
title: AEM Workflow[Part4]中的变量
seo-title: AEM Workflow[Part4]中的变量
description: 在aem工作流中使用xml、json、arraylist、文档类型的变量
seo-description: 在aem工作流中使用xml、json、arraylist、文档类型的变量
feature: workflow
topics: development
audience: developer
doc-type: tutorial
activity: understand
version: 6.5
translation-type: tm+mt
source-git-commit: defefc1451e2873e81cd81e3cccafa438aa062e3
workflow-type: tm+mt
source-wordcount: '471'
ht-degree: 0%

---


# AEM Workflow中的ArrayList变量

在AEM Forms6.5中引入了ArrayList类型的变量。使用ArrayList变量的一个常见用例是定义要在AssignTask中使用的自定义路由。

要在AEM工作流中使用ArrayList变量，您需要创建一个自适应表单，它在提交的数据中生成重复元素。 通常的做法是定义包含数组元素的模式。 为了本文的目的，我创建了一个包含数组元素的简单JSON模式。 用例是员工填写费用报表。 在费用报告中，我们会捕获提交者的经理姓名和经理的经理姓名。 管理器的名称存储在名为managerchain的阵列中。 以下屏幕截图显示了费用报告表单和来自自适应Forms提交的数据。

![费用报表](assets/expensereport.jpg)

以下是自适应表单提交中的数据。 自适应表单基于JSON模式，绑定到模式的数据存储在afBoundData元素的数据元素下。 managerchain是一个数组，我们需要用managerchain数组中对象的名称元素填充ArrayList。

```json
{
    "afData": {
        "afUnboundData": {
            "data": {
                "numericbox_2762582281554154833426": 700
            }
        },
        "afBoundData": {
            "data": {
                "Employee": {
                    "Name": "Conrad Simms",
                    "Department": "IT",
                    "managerchain": [{
                        "name": "Gloria Rios"
                    }, {
                        "name": "John Jacobs"
                    }]
                },
                "expense": [{
                    "description": "Hotel",
                    "amount": 300
                }, {
                    "description": "Air Fare",
                    "amount": 400
                }]
            }
        },
        "afSubmissionInfo": {
            "computedMetaInfo": {},
            "stateOverrides": {},
            "signers": {},
            "afPath": "/content/dam/formsanddocuments/helpx/travelexpensereport",
            "afSubmissionTime": "20190402102953"
            }
        }
}
```

要初始化子类型字符串的ArrayList变量，可以使用JSON点表示法或XPath映射模式。 以下屏幕截图显示了使用JSON点记号填充名为CustomRoutes的ArrayList变量。 确保您指向数组对象中的元素，如下面的屏幕截图所示。 我们使用managerchain数组对象的名称填充CustomRoutes ArrayList。
然后，CustomRoutes ArrayList用于填充AssignTask组件![](assets/arraylist.jpg)customroutes中的路由。一旦CustomRoutes ArrayList变量初始化为提交数据中的值，AssignTask组件的路由就使用CustomRoutes变量填充。 下面的屏幕截图显示了AssignTask任务中的自定义![路由](assets/customactions.jpg)

要在系统上测试此工作流，请按照以下步骤操作

* 下载ArrayListVariable.zip文件并将其保存到文件系统
* [使用AEM](assets/arraylistvariable.zip) Package Manager导入zip文件
* [打开TravelExpenseReport表单](http://localhost:4502/content/dam/formsanddocuments/helpx/travelexpensereport/jcr:content?wcmmode=disabled)
* 输入几项费用和2名经理的姓名
* 点击提交按钮
* [打开收件箱](http://localhost:4502/aem/inbox)
* 您应当看到一个标题为“分配给费用管理员”的新任务
* 打开与任务关联的表单
* 您应当看到两个具有管理器名称的自定义路由
   [浏览ReviewExpenseReportWorkflow。](http://localhost:4502/editor.html/conf/global/settings/workflow/models/ReviewExpenseReport.html) 此工作流使用ArrayList变量、JSON类型变量、或拆分组件中的规则编辑器
