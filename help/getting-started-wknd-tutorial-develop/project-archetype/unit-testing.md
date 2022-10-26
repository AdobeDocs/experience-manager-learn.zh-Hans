---
title: 单元测试
description: 实施单元测试，以验证在自定义组件教程中创建的署名组件Sling模型的行为。
version: 6.5, Cloud Service
type: Tutorial
feature: APIs, AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
kt: 4089
mini-toc-levels: 1
thumbnail: 30207.jpg
exl-id: b926c35e-64ad-4507-8b39-4eb97a67edda
recommendations: noDisplay, noCatalog
source-git-commit: de2fa2e4c29ce6db31233ddb1abc66a48d2397a6
workflow-type: tm+mt
source-wordcount: '3014'
ht-degree: 0%

---

# 单元测试 {#unit-testing}

本教程涵盖单元测试的实施，该测试将验证在 [自定义组件](./custom-component.md) 教程。

## 前提条件 {#prerequisites}

查看设置 [本地开发环境](overview.md#local-dev-environment).

_如果系统上同时安装了Java 8和Java 11，则VS Code测试运行程序在执行测试时可能会选择较低的Java运行时，从而导致测试失败。 如果发生这种情况，请卸载Java 8。_

### 入门项目

>[!NOTE]
>
> 如果您成功完成了上一章，则可以重复使用该项目并跳过签出起始项目的步骤。

查看本教程构建的基行代码：

1. 查看 `tutorial/unit-testing-start` 分支 [GitHub](https://github.com/adobe/aem-guides-wknd)

   ```shell
   $ cd aem-guides-wknd
   $ git checkout tutorial/unit-testing-start
   ```

1. 使用您的Maven技能将代码库部署到本地AEM实例：

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > 如果使用AEM 6.5或6.4，请在 `classic` 配置文件。

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

您始终可以在 [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/unit-testing-start) 或通过切换到分支在本地检出代码 `tutorial/unit-testing-start`.

## 目标

1. 了解单元测试的基础知识。
1. 了解常用于测试AEM代码的框架和工具。
1. 了解在编写单元测试时用于模拟或模拟AEM资源的选项。

## 背景 {#unit-testing-background}

在本教程中，我们将探讨如何编写 [单元测试](https://en.wikipedia.org/wiki/Unit_testing) 对于我们的署名组件 [Sling模型](https://sling.apache.org/documentation/bundles/models.html) (在中创建 [创建自定义AEM组件](custom-component.md))。 单元测试是使用Java编写的内部版本时测试，用于验证Java代码的预期行为。 每个单元测试通常都较小，并根据预期结果验证方法（或工作单元）的输出。

我们使用AEM最佳实践，并采用：

* [JUnit 5](https://junit.org/junit5/)
* [Mockito测试框架](https://site.mockito.org/)
* [wcm.io测试框架](https://wcm.io/testing/) (建立于 [Apache Sling Mocks](https://sling.apache.org/documentation/development/sling-mock.html))

## 单元测试和AdobeCloud Manager {#unit-testing-and-adobe-cloud-manager}

[AdobeCloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/introduction-to-cloud-manager.html?lang=zh-Hans) 集成单元测试执行和 [代码覆盖报告](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/how-to-use/understand-your-test-results.html#code-quality-testing) 到其CI/CD管道中，以帮助鼓励和促进单元测试AEM代码的最佳实践。

虽然单元测试代码是任何代码库的最佳实践，但在使用Cloud Manager时，务必要通过为Cloud Manager运行单元测试来利用其代码质量测试和报告功能。

## 更新测试Maven依赖项 {#inspect-the-test-maven-dependencies}

第一步是检查Maven依赖项，以支持编写和运行测试。 需要四个依赖项：

1. JUnit5
1. Mockito测试框架
1. Apache Sling Mocks
1. AEM Mocks测试框架（由io.wcm提供）

的 **JUnit5**, **莫基托** 和 **AEM Mocks** 在使用 [AEM Maven原型](project-setup.md).

1. 要查看这些依赖项，请在 **aem-guides-wknd/pom.xml**，导航到 `<dependencies>..</dependencies>` 并在io.wcm下查看JUnit、Mockito、Apache Sling Mocks和AEM Mok Tests的依赖项 `<!-- Testing -->`.
1. 确保 `io.wcm.testing.aem-mock.junit5` 设置为 **4.1.0**:

   ```xml
   <dependency>
       <groupId>io.wcm</groupId>
       <artifactId>io.wcm.testing.aem-mock.junit5</artifactId>
       <version>4.1.0</version>
       <scope>test</scope>
   </dependency>
   ```

   >[!CAUTION]
   >
   > 原型 **35** 生成项目时使用 `io.wcm.testing.aem-mock.junit5` 版本 **4.1.8**. 请降级为 **4.1.0** 以遵循本章的其余部分。

1. 打开 **aem-guides-wknd/core/pom.xml** 并查看相应的测试依赖关系是否可用。

   中的并行源文件夹 **核心** 项目将包含单元测试和任何支持测试文件。 此 **测试** 文件夹提供了与源代码分离的测试类，但允许测试像与源代码位于同一包中一样进行操作。

## 创建JUnit测试 {#creating-the-junit-test}

单元测试通常使用Java类将1对1映射。 在本章中，我们将为 **BylineImpl.java**，即支持Byline组件的Sling模型。

![单元测试src文件夹](assets/unit-testing/core-src-test-folder.png)

*存储单元测试的位置。*

1. 为 `BylineImpl.java` 通过在 `src/test/java` 在镜像要测试的Java类位置的Java包文件夹结构中。

   ![创建新的BylineImplTest.java文件](assets/unit-testing/new-bylineimpltest.png)

   因为我们在测试

   * `src/main/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImpl.java`

   在

   * `src/test/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.java`

   的 `Test` 单元测试文件的后缀， `BylineImplTest.java` 是一项公约，它允许我们

   1. 轻松将其识别为测试文件 _表示_ `BylineImpl.java`
   1. 但是，要区分测试文件 _从_ 正在接受测试的班级， `BylineImpl.java`



## 查看BylineImplTest.java {#reviewing-bylineimpltest-java}

此时，JUnit测试文件是空Java类。

1. 使用以下代码更新文件：

   ```java
   package com.adobe.aem.guides.wknd.core.models.impl;
   
   import static org.junit.jupiter.api.Assertions.*;
   
   import org.junit.jupiter.api.BeforeEach;
   import org.junit.jupiter.api.Test;
   
   public class BylineImplTest {
   
       @BeforeEach
       void setUp() throws Exception {
   
       }
   
       @Test 
       void testGetName() { 
           fail("Not yet implemented");
       }
   
       @Test 
       void testGetOccupations() { 
           fail("Not yet implemented");
       }
   
       @Test 
       void testIsEmpty() { 
           fail("Not yet implemented");
       }
   }
   ```

1. 第一种方法 `public void setUp() { .. }` 带有JUnit注释 `@BeforeEach`，指示JUnit测试运行器在运行此类中的每个测试方法之前执行此方法。 这为初始化所有测试所需的常见测试状态提供了方便的位置。

1. 后续方法是测试方法，其名称前缀为 `test` 以公约为标志 `@Test` 注释。 请注意，默认情况下，由于我们尚未实施这些测试，因此所有测试都将设置为失败。

   首先，我们从一个测试方法开始，针对我们测试的类中的每个公共方法，这样：

   | BylineImpl.java |  | BylineImplTest.java |
   | ------------------|--------------|---------------------|
   | getName() | 测试方式 | testGetName() |
   | getScriptions() | 测试方式 | testGetSchories() |
   | isEmpty() | 测试方式 | testIsEmpty() |

   这些方法可以根据需要进行扩展，我们将在本章的后面部分看到。

   运行此JUnit测试类（也称为JUnit测试案例）时，每个方法均使用 `@Test` 将作为测试执行，测试可能通过或失败。

![生成的BylineImplTest](assets/unit-testing/bylineimpltest-stub-methods.png)

*`core/src/test/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.java`*

1. 通过右键单击 `BylineImplTest.java` 文件，然后点按 **运行**.
如预期，所有测试都会失败，因为尚未实施。

   ![作为junit测试运行](assets/unit-testing/run-junit-tests.png)

   *右键单击BylineImplTests.java >运行*

## 查看BylineImpl.java {#reviewing-bylineimpl-java}

编写单元测试时，主要采用以下两种方法：

* [TDD或测试驱动开发](https://en.wikipedia.org/wiki/Test-driven_development)，即在实施开发之前，逐步编写单元测试；编写测试，编写实现以使测试通过。
* 实施优先开发，包括先开发工作代码，然后编写验证该代码的测试。

在本教程中，将使用后一种方法(因为我们已经创建了一个 **BylineImpl.java** （在上一章中）。 因此，我们必须对其公共方法的行为进行审查和理解，并了解其实施细节。 这听上去可能相反，因为良好的测试应该只关心输入和输出，但在AEM中工作时，需要了解各种实施考虑因素，才能构建工作测试。

对于AEM,TDD需要一定的专业技能，最能被精通AEM代码开发和单元测试的AEM开发人员采用。

## 设置AEM测试上下文  {#setting-up-aem-test-context}

为AEM编写的大多数代码都依赖于JCR、Sling或AEM API，而JCR、Sling或API反过来又需要运行AEM的上下文才能正确执行。

由于单元测试是在生成时执行的，因此在运行AEM实例的上下文之外，不存在此类上下文。 为了方便， [wcm.io的AEM Tacks](https://wcm.io/testing/aem-mock/usage.html) 创建允许这些API的模拟上下文 _大部分_ 就好像它们在AEM中运行一样。

1. 使用创建AEM上下文 **wcm.io的** `AemContext` in **BylineImplTest.java** 添加为JUnit扩展，其中 `@ExtendWith` 到 **BylineImplTest.java** 文件。 扩展负责处理所有所需的初始化和清理任务。 为创建类变量 `AemContext` 可用于所有测试方法。

   ```java
   import org.junit.jupiter.api.extension.ExtendWith;
   import io.wcm.testing.mock.aem.junit5.AemContext;
   import io.wcm.testing.mock.aem.junit5.AemContextExtension;
   ...
   
   @ExtendWith(AemContextExtension.class)
   class BylineImplTest {
   
       private final AemContext ctx = new AemContext();
   ```

   此变量， `ctx`，会公开一个模拟AEM上下文，其中提供了许多AEM和Sling抽象概念：

   * BylineImpl Sling模型已注册到此上下文中
   * 在此上下文中创建模拟JCR内容结构
   * 可在此上下文中注册自定义OSGi服务
   * 提供各种常见的必需模拟对象和帮助程序，如SlingHttpServletRequest对象、各种模拟Sling和AEM OSGi服务，如ModelFactory、PageManager、Page、Template、ComponentManager、Component、TagManager、Tag等。
      * *请注意，并非这些对象的所有方法都已实施！*
   * 和 [更多](https://wcm.io/testing/aem-mock/usage.html)!

   的 **`ctx`** 对象将作为我们大多数模拟上下文的入口点。

1. 在 `setUp(..)` 方法，在每个 `@Test` 方法，定义通用的模拟测试状态：

   ```java
   @BeforeEach
   public void setUp() throws Exception {
       ctx.addModelsForClasses(BylineImpl.class);
       ctx.load().json("/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.json", "/content");
   }
   ```

   * **`addModelsForClasses`** 在模拟AEM上下文中注册要测试的Sling模型，以便它可以在 `@Test` 方法。
   * **`load().json`** 将资源结构加载到模拟上下文中，从而允许代码与这些资源进行交互，就像这些资源是由真实存储库提供的一样。 文件中的资源定义 **`BylineImplTest.json`** 将加载到下的模拟JCR上下文中 **/content**.
   * **`BylineImplTest.json`** 尚不存在，因此我们创建它并定义测试所需的JCR资源结构。

1. 表示模拟资源结构的JSON文件存储在 **核心/src/test/resources** 遵循与JUnit Java测试文件相同的包路径。

   在以下位置创建新的JSON文件 `core/test/resources/com/adobe/aem/guides/wknd/core/models/impl` 已命名 **BylineImplTest.json** ，其内容如下：

   ```json
   {
       "byline": {
       "jcr:primaryType": "nt:unstructured",
       "sling:resourceType": "wknd/components/content/byline"
       }
   }
   ```

   ![BylineImplTest.json](assets/unit-testing/bylineimpltest-json.png)

   此JSON为Byline组件单元测试定义了一个模拟资源（JCR节点）。 此时，JSON具有表示署名组件内容资源( `jcr:primaryType` 和 `sling:resourceType`.

   使用单元测试时，一般规则是创建满足每个测试所需的最小模拟内容、上下文和代码集。 避免在编写测试之前构建完整的模拟上下文的诱惑，因为这通常会产生不需要的工件。

   现在有了 **BylineImplTest.json**，当 `ctx.json("/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.json", "/content")` ，则模拟资源定义将加载到路径的上下文中 **/content.**

## 测试getName() {#testing-get-name}

既然我们有了基本的模拟上下文设置，让我们为 **BylineImpl的getName()**. 此测试必须确保方法 **getName()** 返回存储在资源“**name&quot;** 属性。

1. 更新 **testGetName**()方法 **BylineImplTest.java** 如下所示：

   ```java
   import com.adobe.aem.guides.wknd.core.models.Byline;
   ...
   @Test
   public void testGetName() {
       final String expected = "Jane Doe";
   
       ctx.currentResource("/content/byline");
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       String actual = byline.getName();
   
       assertEquals(expected, actual);
   }
   ```

   * **`String expected`** 设置预期值。 我们将此参数设置为“**珍·多内**&quot;
   * **`ctx.currentResource`** 设置模拟资源的上下文以评估针对的代码，因此将其设置为 **/content/byline** 因为模拟署名内容资源就是在这里加载的。
   * **`Byline byline`** 通过从模拟请求对象中对其进行改编，来实例化Byline Sling模型。
   * **`String actual`** 调用我们测试的方法， `getName()`，位于Byline Sling模型对象上。
   * **`assertEquals`** 声明预期值与署名Sling模型对象返回的值匹配。 如果这些值不相等，测试将失败。

1. 运行测试……并且失败，但 `NullPointerException`.

   请注意，此测试不会失败，因为我们从未定义 `name` 属性，这会导致测试失败，但测试执行尚未达到该时间点！ 由于 `NullPointerException` 在署名对象本身上。

1. 在 `BylineImpl.java`，如果 `@PostConstruct init()` 会引发异常，从而阻止Sling模型实例化，并导致Sling模型对象为空。

   ```java
   @PostConstruct
   private void init() {
       image = modelFactory.getModelFromWrappedRequest(request, request.getResource(), Image.class);
   }
   ```

   事实上，虽然ModelFactory OSGi服务是通过 `AemContext` （通过Apache Sling Context），并未实施所有方法，包括 `getModelFromWrappedRequest(...)` 在BylineImpl的 `init()` 方法。 这会导致 [AbstractMethodError](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/AbstractMethodError.html)，其原因 `init()` 失败，并随之调整 `ctx.request().adaptTo(Byline.class)` 是空对象。

   由于提供的吊床无法容纳我们的代码，因此我们必须自己实施模拟上下文。为此，我们可以使用Mockito创建一个模拟ModelFactory对象，该对象在 `getModelFromWrappedRequest(...)` 将对其调用。

   由于为了甚至实例化署名Sling模型，此模拟上下文必须就地，因此我们可以将其添加到 `@Before setUp()` 方法。 我们还需要将 `MockitoExtension.class` 到 `@ExtendWith` 注释上方 **BylineImplTest** 类。

   ```java
   package com.adobe.aem.guides.wknd.core.models.impl;
   
   import org.mockito.junit.jupiter.MockitoExtension;
   import org.mockito.Mock;
   
   import com.adobe.aem.guides.wknd.core.models.Byline;
   import com.adobe.cq.wcm.core.components.models.Image;
   
   import io.wcm.testing.mock.aem.junit5.AemContext;
   import io.wcm.testing.mock.aem.junit5.AemContextExtension;
   
   import org.apache.sling.models.factory.ModelFactory;
   import org.junit.jupiter.api.BeforeEach;
   import org.junit.jupiter.api.Test;
   import org.junit.jupiter.api.extension.ExtendWith;
   
   import static org.junit.jupiter.api.Assertions.*;
   import static org.mockito.Mockito.*;
   import org.apache.sling.api.resource.Resource;
   
   @ExtendWith({ AemContextExtension.class, MockitoExtension.class })
   public class BylineImplTest {
   
       private final AemContext ctx = new AemContext();
   
       @Mock
       private Image image;
   
       @Mock
       private ModelFactory modelFactory;
   
       @BeforeEach
       public void setUp() throws Exception {
           ctx.addModelsForClasses(BylineImpl.class);
   
           ctx.load().json("/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.json", "/content");
   
           lenient().when(modelFactory.getModelFromWrappedRequest(eq(ctx.request()), any(Resource.class), eq(Image.class)))
                   .thenReturn(image);
   
           ctx.registerService(ModelFactory.class, modelFactory, org.osgi.framework.Constants.SERVICE_RANKING,
                   Integer.MAX_VALUE);
       }
   
       @Test
       void testGetName() { ...
   }
   ```

   * **`@ExtendWith({AemContextExtension.class, MockitoExtension.class})`** 标记要使用 [模仿JUnit Jupiter扩展](https://www.javadoc.io/page/org.mockito/mockito-junit-jupiter/latest/org/mockito/junit/jupiter/MockitoExtension.html) 允许使用@Mock注释在类级别定义模拟对象。
   * **`@Mock private Image`** 创建类型的模拟对象 `com.adobe.cq.wcm.core.components.models.Image`. 请注意，这是在类级别定义的，以便根据需要， `@Test` 方法可以根据需要更改其行为。
   * **`@Mock private ModelFactory`** 创建ModelFactory类型的模拟对象。 请注意，这是纯Mockito模型，没有在其上实施任何方法。 请注意，这是在类级别定义的，以便根据需要， `@Test`方法可以根据需要更改其行为。
   * **`when(modelFactory.getModelFromWrappedRequest(..)`** 在 `getModelFromWrappedRequest(..)` 在模拟ModelFactory对象中调用。 在中定义的结果 `thenReturn (..)` 是返回模拟图像对象。 请注意，仅在以下情况下才会调用此行为：第1个参数等于 `ctx`的请求对象，第2个参数是任何资源对象，第3个参数必须是核心组件图像类。 我们接受任何资源，因为在整个测试过程中，我们将设置 `ctx.currentResource(...)` 到 **BylineImplTest.json**. 请注意，我们将 **enlight()** 严格，因为我们稍后会希望覆盖ModelFactory的此行为。
   * **`ctx.registerService(..)`。** 在AemContext中注册模拟ModelFactory对象，其服务排名最高。 由于BylineImpl中使用的ModelFactory `init()` 通过 `@OSGiService ModelFactory model` 字段。 为了使AemContext插入 **我们的** mock对象，用于处理调用 `getModelFromWrappedRequest(..)`，则必须将其注册为该类型(ModelFactory)的最高级别服务。

1. 重新运行测试，然后再次失败，但这一次，消息明确了测试失败的原因。

   ![测试名称失败断言](assets/unit-testing/testgetname-failure-assertion.png)

   *testGetName()由于断言失败*

   我们收到 **断言错误** 这意味着测试中的断言条件失败，它告诉我们 **预期值为“Jane Doe”** 但 **实际值为空**. 这是有道理的，因为“**name&quot;** 未将属性添加到模拟 **/content/byline** 资源定义 **BylineImplTest.json**，因此，让我们添加它：

1. 更新 **BylineImplTest.json** 定义 `"name": "Jane Doe".`

   ```json
   {
       "byline": {
       "jcr:primaryType": "nt:unstructured",
       "sling:resourceType": "wknd/components/content/byline",
       "name": "Jane Doe"
       }
   }
   ```

1. 重新运行测试，并 **`testGetName()`** 快过去！

   ![测试名称通过](assets/unit-testing/testgetname-pass.png)


## 测试getSchories() {#testing-get-occupations}

很好！ 我们的第一个测试通过了！ 让我们继续测试 `getOccupations()`. 由于模拟上下文的初始化在 `@Before setUp()`方法，它适用于所有 `@Test` 方法，包括 `getOccupations()`.

请记住，此方法必须返回按字母顺序排序的职业列表（降序），这些职业列表存储在职业属性中。

1. 更新 **`testGetOccupations()`** 如下所示：

   ```java
   import java.util.List;
   import com.google.common.collect.ImmutableList;
   ...
   @Test
   public void testGetOccupations() {
       List<String> expected = new ImmutableList.Builder<String>()
                               .add("Blogger")
                               .add("Photographer")
                               .add("YouTuber")
                               .build();
   
       ctx.currentResource("/content/byline");
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       List<String> actual = byline.getOccupations();
   
       assertEquals(expected, actual);
   }
   ```

   * **`List<String> expected`** 定义预期结果。
   * **`ctx.currentResource`** 设置当前资源以根据/content/byline上的模拟资源定义评估上下文。 这可确保 **BylineImpl.java** 在模拟资源的上下文中执行。
   * **`ctx.request().adaptTo(Byline.class)`** 通过从模拟请求对象中对其进行改编，来实例化Byline Sling模型。
   * **`byline.getOccupations()`** 调用我们测试的方法， `getOccupations()`，位于Byline Sling模型对象上。
   * **`assertEquals(expected, actual)`** 断言预期列表与实际列表相同。

1. 记住，就象 **`getName()`** 上面， **BylineImplTest.json** 不定义职业，因此如果我们运行它，测试将失败，因为 `byline.getOccupations()` 将返回空列表。

   更新 **BylineImplTest.json** 列入职业清单，并按非字母顺序排列，以确保我们的测试验证职业是否按字母顺序排序 **`getOccupations()`**.

   ```json
   {
       "byline": {
       "jcr:primaryType": "nt:unstructured",
       "sling:resourceType": "wknd/components/content/byline",
       "name": "Jane Doe",
       "occupations": ["Photographer", "Blogger", "YouTuber"]
       }
   }
   ```

1. 运行测试，再次通过！ 看来分类的职业很管用！

   ![职业通过](assets/unit-testing/testgetoccupations-pass.png)

   *testGetSchories()通过*

## 测试isEmpty() {#testing-is-empty}

最后一种测试方法 **`isEmpty()`**.

测试 `isEmpty()` 非常有趣，因为它需要对各种条件进行测试。 审核 **BylineImpl.java**&#39;s `isEmpty()` 方法必须测试以下条件：

* 名称为空时返回true
* 当职业为空时返回true
* 当图像为null或没有src URL时，返回true
* 当名称、职业和图像（使用src URL）存在时，返回false

为此，我们需要创建新的测试方法，每个测试都具有特定的条件，并且还需要在 `BylineImplTest.json` 来进行这些测试。

请注意，此检查允许我们在何时跳过测试 `getName()`, `getOccupations()` 和 `getImage()` 为空，因为通过测试该状态的预期行为 `isEmpty()`.

1. 第一个测试将测试没有设置属性的全新组件的条件。

   将新资源定义添加到 `BylineImplTest.json`，为其提供语义名称“**空**&quot;

   ```json
   {
       "byline": {
           "jcr:primaryType": "nt:unstructured",
           "sling:resourceType": "wknd/components/content/byline",
           "name": "Jane Doe",
           "occupations": ["Photographer", "Blogger", "YouTuber"]
       },
       "empty": {
           "jcr:primaryType": "nt:unstructured",
           "sling:resourceType": "wknd/components/content/byline"
       }
   }
   ```

   **`"empty": {...}`** 定义名为“empty”的新资源定义，该定义仅具有 `jcr:primaryType` 和 `sling:resourceType`.

   记住我们加载了 `BylineImplTest.json` into `ctx` 执行 `@setUp`，因此，我们可在的测试中立即使用此新资源定义 **/content/empty。**

1. 更新 `testIsEmpty()` 如下所示，将当前资源设置为新的“**空**&quot;模拟资源定义。

   ```java
   @Test
   public void testIsEmpty() {
       ctx.currentResource("/content/empty");
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertTrue(byline.isEmpty());
   }
   ```

   运行测试并确保测试通过。

1. 接下来，创建一组方法，以确保当任何所需数据点（名称、职位或图像）为空时， `isEmpty()` 返回true。

   对于每个测试，都使用离散的模拟资源定义，更新 **BylineImplTest.json** 的其他资源定义 **with-name** 和 **不从事职业**.

   ```json
   {
       "byline": {
           "jcr:primaryType": "nt:unstructured",
           "sling:resourceType": "wknd/components/content/byline",
           "name": "Jane Doe",
           "occupations": ["Photographer", "Blogger", "YouTuber"]
       },
       "empty": {
           "jcr:primaryType": "nt:unstructured",
           "sling:resourceType": "wknd/components/content/byline"
       },
       "without-name": {
           "jcr:primaryType": "nt:unstructured",
           "sling:resourceType": "wknd/components/content/byline",
           "occupations": "[Photographer, Blogger, YouTuber]"
       },
       "without-occupations": {
           "jcr:primaryType": "nt:unstructured",
           "sling:resourceType": "wknd/components/content/byline",
           "name": "Jane Doe"
       }
   }
   ```

   请创建以下测试方法来测试这些状态中的每种状态。

   ```java
   @Test
   public void testIsEmpty() {
       ctx.currentResource("/content/empty");
   
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertTrue(byline.isEmpty());
   }
   
   @Test
   public void testIsEmpty_WithoutName() {
       ctx.currentResource("/content/without-name");
   
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertTrue(byline.isEmpty());
   }
   
   @Test
   public void testIsEmpty_WithoutOccupations() {
       ctx.currentResource("/content/without-occupations");
   
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertTrue(byline.isEmpty());
   }
   
   @Test
   public void testIsEmpty_WithoutImage() {
       ctx.currentResource("/content/byline");
   
       lenient().when(modelFactory.getModelFromWrappedRequest(eq(ctx.request()),
           any(Resource.class),
           eq(Image.class))).thenReturn(null);
   
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertTrue(byline.isEmpty());
   }
   
   @Test
   public void testIsEmpty_WithoutImageSrc() {
       ctx.currentResource("/content/byline");
   
       when(image.getSrc()).thenReturn("");
   
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertTrue(byline.isEmpty());
   }
   ```

   **`testIsEmpty()`** 针对空的模拟资源定义进行测试，并声明 `isEmpty()` 是真的。

   **`testIsEmpty_WithoutName()`** 针对有职业但没有名字的模拟资源定义进行测试。

   **`testIsEmpty_WithoutOccupations()`** 针对名为但没有职业的模拟资源定义进行测试。

   **`testIsEmpty_WithoutImage()`** 对名称和职业的模拟资源定义进行测试，但将模拟图像设置为返回空值。 请注意，我们要覆盖 `modelFactory.getModelFromWrappedRequest(..)`在 `setUp()` 以确保此调用返回的图像对象为null。 模仿作品的小作品功能很严格，不需要重复的代码。 因此我们用 **`lenient`** 要明确注意的设置，我们正在覆盖 `setUp()` 方法。

   **`testIsEmpty_WithoutImageSrc()`** 针对名称和职业的模拟资源定义进行测试，但将模拟图像设置为在 `getSrc()` 将调用。

1. 最后，编写测试以确保 **isEmpty()** 正确配置组件后，返回false。 对于此情况，我们可以重复使用 **/content/byline** 表示已完全配置的Byline组件。

   ```java
   @Test
   public void testIsNotEmpty() {
       ctx.currentResource("/content/byline");
       when(image.getSrc()).thenReturn("/content/bio.png");
   
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertFalse(byline.isEmpty());
   }
   ```

1. 现在，在BylineImplTest.java文件中运行所有单元测试，并查看Java测试报表输出。

![所有测试均通过](./assets/unit-testing/all-tests-pass.png)

## 运行单元测试作为内部版本的一部分 {#running-unit-tests-as-part-of-the-build}

在maven内部版本中，需要执行单元测试才能通过。 这可确保在部署应用程序之前成功通过所有测试。 执行Maven目标（如包或安装）时，会自动调用并要求通过项目中的所有单元测试。

```shell
$ mvn package
```

![mvn包成功](assets/unit-testing/mvn-package-success.png)

```shell
$ mvn package
```

同样，如果我们将测试方法更改为失败，则生成会失败并报告测试失败的原因。

![mvn包失败](assets/unit-testing/mvn-package-fail.png)

## 查看代码 {#review-the-code}

在上查看完成的代码 [GitHub](https://github.com/adobe/aem-guides-wknd) 或在Git浏览器的本地查看和部署代码 `tutorial/unit-testing-solution`.
