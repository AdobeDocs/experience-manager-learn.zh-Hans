---
title: '在AEM自适应Forms中使用自动测试 '
description: 使用Calvin SDK自动测试自适应Forms
feature: 自适应表单
doc-type: article
activity: develop
version: 6.3,6.4,6.5
topic: 开发
role: Developer
level: Beginner
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '446'
ht-degree: 1%

---


# 在AEM自适应Forms中使用自动测试 {#using-automated-tests-with-aem-adaptive-forms}

使用Calvin SDK自动测试自适应Forms

Calvin SDK是一个实用程序API，供自适应Forms开发人员测试自适应Forms。 Calvin SDK是基于[Hobbes.js测试框架](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions.html)构建的。 Calvin SDK从AEM Forms 6.3开始提供。

在本教程中，您将创建以下内容：

* 测试套件
* 测试包将包含一个或多个测试用例
* 测试用例将包含一个或多个操作

## 入门 {#getting-started}

[使用包管理器下载和安装资](assets/testingadaptiveformsusingcalvinsdk1.zip)产该包包含示例脚本和多个自适应Forms。这些自适应Forms是使用AEM Forms 6.3版本构建的。如果您在AEM Forms 6.4或更高版本上测试此功能，则建议创建特定于您的AEM Forms版本的新表单。 示例脚本演示了可用于测试自适应Forms的各种Calvin SDK API。 测试AEM自适应Forms的一般步骤如下：

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

上述代码将创建一个新的测试包。

* 在本例中， TestSuite的名称为“ `Mortgage Form Test` ”。
* 提供了AEM中包含测试包的js文件的绝对路径。
* 设置为“ `true` ”时，register参数会使测试套件在测试UI中可用。

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
>如果您正在AEM Forms 6.4或更高版本上测试此功能，请创建新的自适应表单并使用它进行测试。不建议使用随包提供的自适应表单。

可以将测试用例添加到要针对自适应表单执行的测试包。

* 要向测试包添加测试用例，请使用TestSuite对象的`addTestCase`方法。
* `addTestCase`方法采用TestCase对象作为参数。
* 要创建TestCase，请使用`hobs.TestCase(..)`方法。
* 注意：第一个参数是将在UI中显示的测试案例名称。
* 创建测试用例后，您可以向测试用例添加操作。
* 可以将包括`navigateTo`、`asserts.isTrue`的操作作为操作添加到测试案例中。

## 运行自动测试 {#running-the-automated-tests}

[](http://localhost:4502/libs/granite/testing/hobbes.html)打开测试包展开测试包并运行测试。如果所有内容都成功运行，您将看到以下输出。

![calvinsdk](assets/calvinimage.png)

## 试用示例测试包 {#try-out-the-sample-test-suites}

作为示例包的一部分，还有三个其他测试包。 您可以通过在clientlibrary的js.txt文件中包含相应的文件来尝试这些操作，如下所示：

```javascript
#base=.

scriptingTest.js
validationTest.js
prefillTest.js
mortgageForm.js
```
