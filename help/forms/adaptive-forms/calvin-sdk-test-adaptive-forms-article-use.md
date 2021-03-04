---
title: '将自动测试与AEM Adaptive Forms结合使用 '
seo-title: '将自动测试与AEM Adaptive Forms结合使用 '
description: 使用Calvin SDK自动测试自适应Forms
seo-description: 使用Calvin SDK自动测试自适应Forms
feature: 自适应表单
topics: development
audience: developer
doc-type: article
activity: develop
version: 6.3,6.4,6.5
uuid: 3ad4e6d6-d3b1-4e4d-9169-847f74ba06be
topic: 开发
role: 开发人员
level: 初学者
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '465'
ht-degree: 1%

---


# 将自动测试与AEM Adaptive Forms {#using-automated-tests-with-aem-adaptive-forms}结合使用

使用Calvin SDK自动测试自适应Forms

Calvin SDK是一个实用程序API，供Adaptive Forms开发人员测试Adaptive Forms。 Calvin SDK构建于[Hobbes.js测试框架](https://docs.adobe.com/docs/en/aem/6-3/develop/ref/test-api/index.html)之上。 从AEM Forms 6.3开始，Calvin SDK可用。

在本教程中，您将创建以下内容：

* Test Suite
* 测试套件将包含一个或多个测试用例
* 测试用例将包含一个或多个操作

## 入门 {#getting-started}

[使用包管理器下载和安装资](assets/testingadaptiveformsusingcalvinsdk1.zip)产该包包含示例脚本和多个自适应Forms。这些自适应Forms是使用AEM Forms 6.3版本构建的。如果要在AEM Forms 6.4或更高版本上测试AEM Forms，则建议创建特定于您的版本的新表单。 范例脚本演示了可用于测试自适应Forms的各种Calvin SDK API。 测试AEM Adaptive Forms的一般步骤有：

* 导航到需要测试的表单
* 设置字段的值
* 提交自适应表单
* 检查错误消息

包中的示例脚本演示了上述所有操作。
让我们浏览`mortgageForm.js`的代码

```javascript
var mortgageFormTS = new hobs.TestSuite("Mortgage Form Test", {
        path: '/etc/clientlibs/testingAFUsingCalvinSDK/mortgageForm.js',
        register: true
})
```

上面的代码将创建一个新的测试套件。

* 在本例中，TestSuite的名称为“ `Mortgage Form Test` ”。
* 提供的是AEM中包含测试套件的js文件的绝对路径。
* 设置为“ `true` ”时的register参数使测试套件在测试UI中可用。

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
>如果您正在AEM Forms 6.4或更高版本上测试此功能，请创建一个新的自适应表单并使用它进行测试。不建议使用包中提供的自适应表单。

可以将测试用例添加到测试套件，以针对自适应表单执行。

* 要向测试套件添加测试用例，请使用TestSuite对象的`addTestCase`方法。
* `addTestCase`方法将TestCase对象作为参数。
* 要创建TestCase，请使用`hobs.TestCase(..)`方法。
* 注意：第一个参数是将在UI中显示的Test Case的名称。
* 创建测试用例后，可以向测试用例添加操作。
* 可以将包括`navigateTo`、`asserts.isTrue`的操作添加为测试用例的操作。

## 运行自动测试{#running-the-automated-tests}

[](http://localhost:4502/libs/granite/testing/hobbes.html)打开测试套件展开测试套件并运行测试。如果所有内容都成功运行，您将看到以下输出。

![calvinsdk](assets/calvinimage.png)

## 试用示例测试套件{#try-out-the-sample-test-suites}

作为示例包的一部分，还有三个其他测试套件。 您可以通过在clientlibrary的js.txt文件中包含相应文件来尝试这些文件，如下所示：

```javascript
#base=.

scriptingTest.js
validationTest.js
prefillTest.js
mortgageForm.js
```
