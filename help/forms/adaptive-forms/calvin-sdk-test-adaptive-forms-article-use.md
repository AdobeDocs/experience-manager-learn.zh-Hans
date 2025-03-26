---
title: 将自动测试与AEM自适应Forms结合使用
description: 使用Calvin SDK自动测试自适应Forms
feature: Adaptive Forms
doc-type: article
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
exl-id: 5a1364f3-e81c-4c92-8972-4fdc24aecab1
last-substantial-update: 2020-09-10T00:00:00Z
duration: 101
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '436'
ht-degree: 0%

---

# 将自动测试与AEM自适应Forms结合使用 {#using-automated-tests-with-aem-adaptive-forms}

使用Calvin SDK自动测试自适应Forms

Calvin SDK是一个实用程序API，供Adaptive Forms开发人员测试Adaptive Forms。 Calvin SDK是基于[Hobbes.js测试框架](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions.html)构建的。 Calvin SDK从AEM Forms 6.3开始提供。

在本教程中，您将创建以下内容：

* 测试套件
* 测试包将包含一个或多个测试用例
* 测试用例将包含一个或多个操作

## 快速入门 {#getting-started}

[使用包管理器下载并安装Assets](assets/testingadaptiveformsusingcalvinsdk1.zip)该包中包含示例脚本和多个自适应Forms。这些自适应Forms是使用AEM Forms 6.3版本生成的。 如果您要在AEM Forms 6.4或更高版本上测试新表单，则建议创建特定于您的AEM Forms版本的新表单。 示例脚本演示了可用于测试自适应Forms的各种Calvin SDK API。 测试AEM自适应Forms的常规步骤包括：

* 导航到需要测试的表单
* 设置字段的值
* 提交自适应表单
* 检查错误消息

该包中的示例脚本演示了上述所有操作。
让我们探索`mortgageForm.js`的代码

```javascript
var mortgageFormTS = new hobs.TestSuite("Mortgage Form Test", {
        path: '/etc/clientlibs/testingAFUsingCalvinSDK/mortgageForm.js',
        register: true
})
```

上述代码将创建一个新的测试包。

* 在此例中，TestSuite的名称为“`Mortgage Form Test`”。
* 提供的是AEM中指向包含测试包的js文件的绝对路径。
* 当设置为“`true`”时，register参数使测试套件在测试UI中可用。

```javascript
.addTestCase(new hobs.TestCase("Calculate amount to borrow")
        // navigate to the mortgage form  which is to be tested
        .navigateTo("/content/forms/af/cal/mortgageform.html?wcmmode=disabled")
  .asserts.isTrue(function () {
            return calvin.isFormLoaded()
        })
```

>[!NOTE]
>
>如果您在AEM Forms 6.4或更高版本上测试此功能，请创建一个新的自适应表单并使用它进行测试。不建议使用随包提供的自适应表单。

可以将测试用例添加到要针对自适应表单执行的测试包。

* 要将测试用例添加到测试包，请使用TestSuite对象的`addTestCase`方法。
* `addTestCase`方法将TestCase对象作为参数。
* 若要创建TestCase，请使用`hobs.TestCase(..)`方法。
* 注意：第一个参数是将显示在UI中的测试用例的名称。
* 创建测试用例后，您可以向测试用例添加操作。
* 包括`navigateTo`、`asserts.isTrue`在内的操作可以作为操作添加到测试用例中。

## 运行自动化测试 {#running-the-automated-tests}

[Openthetestsuite](http://localhost:4502/libs/granite/testing/hobbes.html)展开测试套件并运行测试。 如果一切运行成功，您将看到以下输出。

![calvinsdk](assets/calvinimage.png)

## 尝试使用示例测试包 {#try-out-the-sample-test-suites}

作为示例包的一部分，还有三个额外的测试包。 您可以通过在clientlibrary的js.txt文件中包含相应的文件来尝试这些文件，如下所示：

```javascript
#base=.

scriptingTest.js
validationTest.js
prefillTest.js
mortgageForm.js
```
