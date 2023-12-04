---
title: 将项目添加到选项组组件
description: 将项目动态添加到选择组组件
feature: Adaptive Forms
version: 6.5
topic: Development
role: User
level: Beginner
exl-id: 8fbea634-7949-417f-a4d6-9e551fff63f3
last-substantial-update: 2021-09-10T00:00:00Z
duration: 374
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '456'
ht-degree: 0%

---

# 将项目动态添加到选择组组件

AEM Forms 6.5引入了将项目动态添加到自适应Forms选择组组件（如复选框、单选按钮和图像列表）的功能。


您可以使用可视编辑器和代码编辑器添加项目，具体取决于您的用例。

**使用可视编辑器：** 您可以根据函数调用或服务调用的结果填充选择组的项目。 例如，您可以通过使用REST API调用的响应来设置选择组的项目。

在下面的屏幕截图中，我们将为名为getLoanPeriods的服务调用结果设置Loan Period(years)选项。

![规则编辑器](assets/ruleeditor.png)

**使用代码编辑器**：当您要根据表单中输入的值动态设置选择组中的项目时。 例如，以下代码片段将复选框的项目设置为在自适应表单的申请人名称和配偶字段中输入的值。

在代码片段中，我们将设置作为复选框组件的WorkingMembers的项目。 项的数组通过提取自适应表单的applicantName和配偶文本字段的值进行动态构建

```javascript
 
 if(MaritalStatus.value=="Married")
  {
WorkingMembers.items =["spouse="+spouse.value,"applicant="+applicantName.value];
  }
else
  {
    WorkingMembers.items =["applicant="+applicantName.value];
  }
```

提交的数据如下

```xml
<afUnboundData>

<data>

<applicantName>John Jacobs</applicantName>

<MaritalStatus>Married</MaritalStatus>

<spouse>Gloria Rios</spouse>

<WorkingMembers>spouse,applicant</WorkingMembers>

</data>

</afUnboundData>
```

**使用规则编辑器添加项目**

>[!VIDEO](https://video.tv.adobe.com/v/26847?quality=12&learn=on)

**使用代码编辑器添加项目**

>[!VIDEO](https://video.tv.adobe.com/v/26848?quality=12&learn=on)

要在系统上尝试此操作，请执行以下操作：

**使用代码编辑器添加项目**

* [下载资源](assets/usingthecodeeditor.zip)
* [打开Forms和文档](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* 单击“创建” |文件上传”并上传您在上一步中下载的文件
* [预览表单](http://localhost:4502/content/dam/formsanddocuments/simpleform/jcr:content?wcmmode=disabled)
* 输入申请人姓名并选择已婚的婚姻状况
* 输入配偶姓名
* 单击“下一步”
* 如果婚姻状况已婚，您应该看到填写申请人姓名和配偶姓名的复选框

**使用可视编辑器添加项目**

* [下载资源](assets/usingthevisualeditor.zip)
* 安装Tomcat（如果尚未安装）。 [此处提供了安装tomcat的说明](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-print-channel-tutorial/introduction.html)
* [将此zip文件中包含的SampleRest.war文件部署到Tomcat中](assets/sample-rest.zip)
* [打开Forms和文档](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* 单击“创建” |文件上传”并上传您在上一步中下载的文件
* [预览表单](http://localhost:4502/content/dam/formsanddocuments/amortizationschedule/jcr:content?wcmmode=disabled)
* 在字段中输入贷款金额并制表符。 这将触发显示贷款期间字段的规则。
* 选择适当的贷款期间（从其余调用填充贷款期间的项目）
* 选择利率并单击“获取摊销计划”
* 应填充摊销表。 使用REST调用获取摊销计划。

>[!NOTE]
> 假定tomcat在端口8080上运行，AEM在端口4502上运行
