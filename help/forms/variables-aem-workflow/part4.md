---
title: AEM Workflow中的变量[Part4]
seo-title: AEM Workflow中的变量[Part4]
description: 在aem工作流中使用xml、json、arraylist、文档类型的变量
seo-description: 在aem工作流中使用xml、json、arraylist、文档类型的变量
feature: 工作流
topics: development
audience: developer
doc-type: tutorial
activity: understand
version: 6.5
topic: 开发
role: 开发人员
level: 初学者
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '475'
ht-degree: 0%

---


# AEM Workflow中的ArrayList变量

AEM Forms 6.5中引入了ArrayList类型的变量。使用ArrayList变量的一个常见用例是定义要在AssignTask中使用的自定义路由。

要在AEM工作流中使用ArrayList变量，您需要创建一个自适应表单，该表单在提交的数据中生成重复元素。 通常的做法是定义一个包含数组元素的模式。 为本文之目的，我创建了一个包含数组元素的简单JSON模式。 用例是员工填写费用报告。 在费用报表中，我们会捕获提交者的经理姓名和经理的经理姓名。 管理器的名称存储在名为managerchain的数组中。 以下屏幕截图显示了费用报表表单以及Adaptive Forms提交中的数据。

![支出报表](assets/expensereport.jpg)

以下是自适应表单提交的数据。 自适应表单基于JSON模式，绑定到模式的数据存储在afBoundData元素的数据元素下。 managerchain是一个数组，我们需要用managerchain数组内对象的名称元素填充ArrayList。

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

要初始化子类型字符串的ArrayList变量，可以使用JSON点表示法或XPath映射模式。 以下屏幕截图显示了使用JSON点表示法填充名为CustomRoutes的ArrayList变量。 确保您指向数组对象中的元素，如以下屏幕截图所示。 我们将使用managerchain数组对象的名称填充CustomRoutes ArrayList。
然后，使用CustomRoutes ArrayList填充AssignTask组件中的路由
![customroutes](assets/arraylist.jpg)
使用提交数据中的值初始化CustomRoutes ArrayList变量后，将使用CustomRoutes变量填充AssignTask组件的路由。 以下屏幕截图显示了AssignTask中的自定义路由
![ asingtask](assets/customactions.jpg)

要在系统上测试此工作流，请执行以下步骤

* 下载ArrayListVariable.zip文件并将其保存到您的文件系统
* [使用AEM包](assets/arraylistvariable.zip) 管理器导入zip文件
* [打开TravelExpenseReport表单](http://localhost:4502/content/dam/formsanddocuments/helpx/travelexpensereport/jcr:content?wcmmode=disabled)
* 输入几个费用和2个经理的姓名
* 单击提交按钮
* [打开您的收件箱](http://localhost:4502/aem/inbox)
* 您应当看到一个标题为“分配给费用管理员”的新任务
* 打开与任务关联的表单
* 您应当看到两个具有管理器名称的自定义路由
   [浏览ReviewExpenseReportWorkflow。](http://localhost:4502/editor.html/conf/global/settings/workflow/models/ReviewExpenseReport.html) 此工作流使用ArrayList变量、JSON类型变量、或拆分组件中的规则编辑器
