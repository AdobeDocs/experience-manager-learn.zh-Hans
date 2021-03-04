---
title: 将项目添加到选择组组件
seo-title: 将项目添加到选择组组件
description: 将项目动态添加到选择组组件
seo-description: 将项目动态添加到选择组组件
feature: 自适应表单
topics: authoring
audience: developer
doc-type: tutorial
activity: understand
version: 6.5
topic: 开发
role: 业务从业者
level: 初学者
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '533'
ht-degree: 0%

---



# 将项目动态添加到选择组组件

AEM Forms 6.5引入了将项目动态添加到自适应Forms选择组组件(如复选框、单选按钮和图像列表)的功能。

[此功能可在Samples Server上实时使用](https://forms.enablementadobe.com/content/samples/samples.html?query=0)。搜索动态复选框项目卡，然后单击“试用”


您可以根据用例使用可视编辑器以及代码编辑器添加项目。

**使用可视编辑器：** 您可以根据函数调用或服务调用的结果填充所选组的项目。例如，您可以通过使用REST API调用的响应来设置选择组的项目。

在以下屏幕截图中，我们将为名为getLoanPeriods的服务调用的结果设置贷款期（年）选项。

![规则编辑器](assets/ruleeditor.png)

**使用代码编辑器**:当您要根据在表单中输入的值动态设置选择组中的项目时。例如，下面的代码片断将复选框的项目设置为自适应表单的申请人姓名和配偶字段中输入的值。

在代码片断中，我们设置WorkingMembers的项，它是一个复选框组件。 通过获取自适应表单的applicantName和配偶文本字段的值，动态构建项目的数组

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

要在系统上试用它：

**使用代码编辑器添加项目**

* [下载资源](assets/usingthecodeeditor.zip)
* [打开Forms和文档](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* 单击“创建” |文件上传”，并上传您在上一步中下载的文件
* [预览表单](http://localhost:4502/content/dam/formsanddocuments/simpleform/jcr:content?wcmmode=disabled)
* 输入申请人姓名并选择要结婚的婚姻状态
* 输入配偶姓名
* 单击“下一步”
* 如果婚姻状况已结婚，则应该显示复选框，其中填入了申请人姓名和配偶姓名

**使用可视编辑器添加项目**

* [下载资源](assets/usingthevisualeditor.zip)
* 如果尚未安装Tomcat，请安装它。 [此处提供安装tomcat的说明](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/ic-print-channel-tutorial/introduction.html)
* [在Tomcat中部署SampleRest.war文件](https://forms.enablementadobe.com/content/DemoServerBundles/SampleRest.war)
* [打开Forms和文档](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* 单击“创建” |文件上传”，并上传您在上一步中下载的文件
* [预览表单](http://localhost:4502/content/dam/formsanddocuments/amortizationschedule/jcr:content?wcmmode=disabled)
* 输入贷款金额并跳出字段。 这将触发显示贷款期间字段的规则。
* 选择适当的贷款期（从其余呼叫中填充贷款期的项目）
* 选择利率并单击“获得摊销计划”
* 应填充摊销表。 使用REST调用获取摊销计划。

>[!NOTE]
> 假定tomcat在端口8080和AEM端口4502上运行
