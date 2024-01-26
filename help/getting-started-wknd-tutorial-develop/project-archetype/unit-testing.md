---
title: 单元测试
description: 实施单元测试，以验证在自定义组件教程中创建的Byline组件的Sling模型的行为。
version: 6.5, Cloud Service
feature: APIs, AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
jira: KT-4089
mini-toc-levels: 1
thumbnail: 30207.jpg
doc-type: Tutorial
exl-id: b926c35e-64ad-4507-8b39-4eb97a67edda
recommendations: noDisplay, noCatalog
duration: 870
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '2923'
ht-degree: 0%

---

# 单元测试 {#unit-testing}

本教程介绍单元测试的实施，该测试将验证在中创建的署名组件的Sling模型的行为。 [自定义组件](./custom-component.md) 教程。

## 前提条件 {#prerequisites}

查看所需的工具和设置说明 [本地开发环境](overview.md#local-dev-environment).

_如果系统上同时安装了Java™ 8和Java™ 11，则VS代码测试运行程序在执行测试时可能会选择较低的Java™运行时，从而导致测试失败。 如果发生这种情况，请卸载Java™ 8。_

### 入门项目

>[!NOTE]
>
> 如果成功完成了上一章，则可以重用该项目并跳过签出入门项目的步骤。

查看本教程所基于的基本行代码：

1. 查看 `tutorial/unit-testing-start` 分支自 [GitHub](https://github.com/adobe/aem-guides-wknd)

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
   > 如果使用AEM 6.5或6.4，请附加 `classic` 配置文件到任何Maven命令。

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

您始终可以在上查看完成的代码 [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/unit-testing-start) 或者切换到分支机构在本地签出代码 `tutorial/unit-testing-start`.

## 目标

1. 了解单元测试的基础知识。
1. 了解通常用于测试AEM代码的框架和工具。
1. 了解在编写单元测试时模拟或模拟AEM资源的选项。

## 背景 {#unit-testing-background}

在本教程中，我们将探讨如何编写代码 [单元测试](https://en.wikipedia.org/wiki/Unit_testing) 对于我们的署名组件的 [Sling模型](https://sling.apache.org/documentation/bundles/models.html) (创建于 [创建自定义AEM组件](custom-component.md))。 单元测试是用Java™编写的构建时测试，用于验证Java™代码的预期行为。 每个单元测试通常很小，可根据预期结果验证方法（或工作单元）的输出。

我们采用AEM最佳实践，并采用：

* [JUnit 5](https://junit.org/junit5/)
* [Mockito测试框架](https://site.mockito.org/)
* [wcm.io测试框架](https://wcm.io/testing/) (构建于 [Apache Sling Mocks](https://sling.apache.org/documentation/development/sling-mock.html))

## 单元测试和AdobeCloud Manager {#unit-testing-and-adobe-cloud-manager}

[AdobeCloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/introduction.html) 集成单元测试执行和 [代码覆盖率报表](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/code-quality-testing.html) 添加到其CI/CD管道中，以帮助鼓励和推广单元测试AEM代码的最佳实践。

虽然单元测试代码是任何代码库的一个良好实践，但在使用Cloud Manager时，通过为Cloud Manager运行提供单元测试来利用其代码质量测试和报告工具非常重要。

## 更新测试Maven依赖项 {#inspect-the-test-maven-dependencies}

第一步是检查Maven依赖项以支持编写和运行测试。 需要四个依赖项：

1. JUnit5
1. Mockito测试框架
1. Apache Sling Mocks
1. AEM Mocks Test Framework （由io.wcm提供）

此 **JUnit5**、 **Mockito和 **AEM Mocks** 测试依赖项在设置过程中使用自动添加到项目中 [AEM Maven原型](project-setup.md).

1. 要查看这些依赖关系，请在以下位置打开父Reactor POM **aem-guides-wknd/pom.xml**，导航到 `<dependencies>..</dependencies>` AEM并查看io.wcm在 `<!-- Testing -->`.
1. 确保 `io.wcm.testing.aem-mock.junit5` 设置为 **4.1.0**：

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
   > 原型 **35** 生成项目，使用 `io.wcm.testing.aem-mock.junit5` 版本 **4.1.8**. 请降级到 **4.1.0** 以遵循本章的其余部分。

1. 打开 **aem-guides-wknd/core/pom.xml** 并查看相应的测试依赖项是否可用。

   中的并行源文件夹 **核心** 项目将包含单元测试和任何支持的测试文件。 此 **测试** 文件夹将测试类与源代码分离，但允许测试像它们与源代码位于同一包中一样运行。

## 创建JUnit测试 {#creating-the-junit-test}

单元测试通常使用Java™类将1映射到1。 在本章中，我们将为 **BylineImpl.java**，这是支持Byline组件的Sling模型。

![单元测试src文件夹](assets/unit-testing/core-src-test-folder.png)

*存储单元测试的位置。*

1. 为创建单元测试 `BylineImpl.java` 通过在下创建新的Java™类 `src/test/java` 在Java™包文件夹结构中，该结构镜像要测试的Java™类的位置。

   ![创建新的BylineImplTest.java文件](assets/unit-testing/new-bylineimpltest.png)

   因为我们正在测试

   * `src/main/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImpl.java`

   在上创建相应的单元测试Java™类

   * `src/test/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.java`

   此 `Test` 单元测试文件上的后缀， `BylineImplTest.java` 是一个惯例，允许我们

   1. 轻松将其识别为测试文件 _对象_ `BylineImpl.java`
   1. 但是，还要区分测试文件 _从_ 被测试的班级， `BylineImpl.java`

## 查看BylineImplTest.java {#reviewing-bylineimpltest-java}

此时，JUnit测试文件是一个空的Java™类。

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

1. 第一种方法 `public void setUp() { .. }` 用JUnit注释 `@BeforeEach`，它会指示JUnit测试运行程序先执行此方法，然后再运行此类中的每个测试方法。 这为初始化所有测试所需的通用测试状态提供了一个方便的位置。

1. 后续的方法是测试方法，其名称带有前缀 `test` 按惯例，并以 `@Test` 注释。 请注意，默认情况下，我们的所有测试都设置为失败，因为尚未实施它们。

   首先，我们先对我们测试的类中的每个公共方法使用一个测试方法，因此：

   | BylineImpl.java |              | BylineImplTest.java |
   | ------------------|--------------|---------------------|
   | getName() | 测试方 | testGetName() |
   | getOccupations() | 测试方 | testGetOccupations() |
   | isEmpty() | 测试方 | testIsEmpty() |

   这些方法可根据需要进行扩展，我们将在本章的后面部分中看到。

   运行此JUnit测试类（也称为JUnit测试用例）时，每个方法都使用 `@Test` 将作为测试执行，测试可能会通过或失败。

![生成的BylineImplTest](assets/unit-testing/bylineimpltest-stub-methods.png)

*`core/src/test/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.java`*

1. 通过右键单击 `BylineImplTest.java` 文件，并点按 **运行**.
如预期的那样，所有测试都会失败，因为它们尚未实施。

   ![作为junit测试运行](assets/unit-testing/run-junit-tests.png)

   *右键单击“BylineImplTests.java”>“运行”*

## 查看BylineImpl.java {#reviewing-bylineimpl-java}

在编写单元测试时，有两种主要方法：

* [TDD或测试驱动开发](https://en.wikipedia.org/wiki/Test-driven_development)，包括增量写入单元测试，紧接着在实施开发之前；编写测试，编写实现以使测试通过。
* 以实施为先的开发，包括先开发工作代码，然后编写测试来验证该代码。

在本教程中，将使用后一种方法(因为我们已经创建了一个 **BylineImpl.java** （例如在上一章中）。 因此，我们必须回顾和了解其公共方法的行为，同时也要了解其实施细节。 这听起来可能恰恰相反，因为一个良好的测试应该只关注输入和输出，然而在AEM中工作时，为了构建工作测试，需要理解各种实施注意事项。

在AEM的上下文中，TDD需要一定程度的专业知识，并且最适合于精通AEM代码的AEM开发和单元测试的AEM开发人员。

## 设置AEM测试上下文  {#setting-up-aem-test-context}

大多数为AEM编写的代码依赖于JCR、Sling或AEM API，这反过来又需要运行的AEM的上下文才能正确执行。

由于单元测试是在构建时执行的，因此在正在运行的AEM实例的上下文之外，没有此类上下文。 为了促进这一进程， [wcm.io的AEM模拟](https://wcm.io/testing/aem-mock/usage.html) 创建模拟上下文以允许这些API _大部分_ 就像它们在AEM中运行一样。

1. 创建AEM上下文，使用 **wcm.io的** `AemContext` 在 **BylineImplTest.java** 添加为装饰有的JUnit扩展 `@ExtendWith` 到 **BylineImplTest.java** 文件。 该扩展会处理所有所需的初始化和清理任务。 创建类变量 `AemContext` 可用于所有测试方法。

   ```java
   import org.junit.jupiter.api.extension.ExtendWith;
   import io.wcm.testing.mock.aem.junit5.AemContext;
   import io.wcm.testing.mock.aem.junit5.AemContextExtension;
   ...
   
   @ExtendWith(AemContextExtension.class)
   class BylineImplTest {
   
       private final AemContext ctx = new AemContext();
   ```

   此变量， `ctx`，会公开提供一些AEM和Sling抽象的模拟AEM上下文：

   * BylineImpl Sling模型已注册到此上下文中
   * 在此上下文中创建模拟JCR内容结构
   * 可在此上下文中注册自定义OSGi服务
   * 提供各种常见的必需模拟对象和帮助程序，如SlingHttpServletRequest对象；各种模拟Sling和AEM OSGi服务，如ModelFactory、PageManager、Page、Template、ComponentManager、Component、TagManager、Tag等。
      * *并非针对这些对象的所有方法都已实现！*
   * 和 [更多](https://wcm.io/testing/aem-mock/usage.html)！

   此 **`ctx`** 对象将作为大多数模拟上下文的入口点。

1. 在 `setUp(..)` 方法，该方法在每次 `@Test` 方法，定义一个常见的模拟测试状态：

   ```java
   @BeforeEach
   public void setUp() throws Exception {
       ctx.addModelsForClasses(BylineImpl.class);
       ctx.load().json("/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.json", "/content");
   }
   ```

   * **`addModelsForClasses`** 将要测试的Sling模型注册到模拟AEM上下文中，以便可在 `@Test` 方法。
   * **`load().json`** 将资源结构加载到模拟上下文中，使代码能够与这些资源交互，就像它们由真实存储库提供一样。 文件中的资源定义 **`BylineImplTest.json`** 加载到模拟JCR上下文中 **/content**.
   * **`BylineImplTest.json`** 尚不存在，因此让我们创建它并定义测试所需的JCR资源结构。

1. 表示模拟资源结构的JSON文件存储在 **core/src/test/resources** 执行与JUnit Java™测试文件相同的包路径。

   在上创建JSON文件 `core/test/resources/com/adobe/aem/guides/wknd/core/models/impl` 已命名 **BylineImplTest.json** ，内容如下：

   ```json
   {
       "byline": {
       "jcr:primaryType": "nt:unstructured",
       "sling:resourceType": "wknd/components/content/byline"
       }
   }
   ```

   ![BylineImplTest.json](assets/unit-testing/bylineimpltest-json.png)

   此JSON为Byline组件单元测试定义了一个模拟资源（JCR节点）。 此时，JSON具有表示署名组件内容资源所需的最小属性集， `jcr:primaryType` 和 `sling:resourceType`.

   处理单元测试时的一般规则是创建满足每个测试所需的最小模拟内容、上下文和代码集。 避免在编写测试之前构建完整的模拟上下文的诱惑，因为这通常会导致不需要的工件。

   现在，随着 **BylineImplTest.json**，时间 `ctx.json("/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.json", "/content")` 执行时，模拟资源定义将加载到路径下的上下文中 **/content.**

## 测试getName() {#testing-get-name}

现在，我们有了基本的模拟上下文设置，让我们编写第一个测试 **BylineImpl的getName()**. 此测试必须确保方法 **getName()** 返回在资源的&#39;&#39;中存储的正确编写的名称&#x200B;**name”** 属性。

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

   * **`String expected`** 设置预期值。 我们会将此参数设置为&quot;**珍已完成**“。
   * **`ctx.currentResource`** 设置模拟资源的上下文以评估代码，因此将此值设置为 **/content/byline** 因为这是加载模拟署名内容资源的位置。
   * **`Byline byline`** 通过从模拟请求对象中调整来实例化Byline Sling模型。
   * **`String actual`** 调用我们正在测试的方法， `getName()`，在Byline Sling模型对象上。
   * **`assertEquals`** 声明预期值与署名Sling模型对象返回的值匹配。 如果这些值不相等，测试将失败。

1. 运行测试……但测试失败 `NullPointerException`.

   此测试不会失败，因为我们从未定义 `name` 属性，即使测试尚未达到该阶段，模拟JSON中的属性仍会导致测试失败！ 此测试失败的原因是 `NullPointerException` 署名对象本身。

1. 在 `BylineImpl.java`， if `@PostConstruct init()` 引发异常，阻止Sling模型实例化，并导致Sling模型对象为空。

   ```java
   @PostConstruct
   private void init() {
       image = modelFactory.getModelFromWrappedRequest(request, request.getResource(), Image.class);
   }
   ```

   原来，虽然ModelFactory OSGi服务是通过 `AemContext` （通过Apache Sling Context），未实现所有方法，包括 `getModelFromWrappedRequest(...)` 在BylineImpl的 `init()` 方法。 这会导致 [AbstractMethodError](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/AbstractMethodError.html)，具体而言会导致 `init()` 失败的情况，以及由此产生的 `ctx.request().adaptTo(Byline.class)` 是null对象。

   由于提供的模拟无法容纳我们的代码，因此我们必须自己实现模拟上下文。为此，我们可以使用Mockito创建一个模拟ModelFactory对象，该对象将在以下情况下返回模拟Image对象： `getModelFromWrappedRequest(...)` 将在其中调用。

   由于甚至要实例化Byline Sling模型，此模拟上下文必须到位，因此我们可以将其添加到 `@Before setUp()` 方法。 我们还需要添加 `MockitoExtension.class` 到 `@ExtendWith` 注释位于 **BylineImplTest** 类。

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

   * **`@ExtendWith({AemContextExtension.class, MockitoExtension.class})`** 标记要与一起运行的测试用例类 [Mockito JUnit Jupiter扩展](https://www.javadoc.io/static/org.mockito/mockito-junit-jupiter/4.11.0/org/mockito/junit/jupiter/MockitoExtension.html) 允许使用@Mock批注在类级别定义模拟对象。
   * **`@Mock private Image`** 创建类型为的模拟对象 `com.adobe.cq.wcm.core.components.models.Image`. 在类级别上定义，这样在需要时即可， `@Test` 方法可以根据需要更改其行为。
   * **`@Mock private ModelFactory`** 创建ModelFactory类型的模拟对象。 这是一个纯粹的Mockito模拟，没有实现任何方法。 在类级别上定义，这样在需要时即可， `@Test`方法可以根据需要更改其行为。
   * **`when(modelFactory.getModelFromWrappedRequest(..)`** 注册模拟行为时间 `getModelFromWrappedRequest(..)` 在模拟ModelFactory对象上调用。 中定义的结果 `thenReturn (..)` 是返回模拟Image对象。 仅当出现以下情况时才会调用此行为：第一个参数等于 `ctx`的请求对象，第二个参数是任何Resource对象，第三个参数必须是核心组件Image类。 我们接受任何资源，因为在整个测试中，我们设置 `ctx.currentResource(...)` 中定义的各种模拟资源 **BylineImplTest.json**. 请注意，我们将 **lenient()** 严格性，因为我们以后会想要覆盖ModelFactory的这种行为。
   * **`ctx.registerService(..)`。** 将模拟ModelFactory对象注册到AemContext中，具有最高的服务排名。 这是必需的，因为BylineImpl的 `init()` 通过 `@OSGiService ModelFactory model` 字段。 要插入的AemContext **我们的** 模拟对象，处理对的调用 `getModelFromWrappedRequest(..)`，我们必须将其注册为该类型的最高级别服务(ModelFactory)。

1. 重新运行测试，再次失败，但这次消息清楚地表明了失败的原因。

   ![测试名称失败断言](assets/unit-testing/testgetname-failure-assertion.png)

   *testGetName()失败，因为断言*

   我们收到 **AssertionError** 这意味着测试中的断言条件失败了，它告诉我们 **预期值为“Jane Doe”** 但是 **实际值为null**. 这是有道理的，因为“**name”** 属性尚未添加到模拟 **/content/byline** 中的资源定义 **BylineImplTest.json**，因此让我们添加它：

1. 更新 **BylineImplTest.json** 以定义 `"name": "Jane Doe".`

   ```json
   {
       "byline": {
       "jcr:primaryType": "nt:unstructured",
       "sling:resourceType": "wknd/components/content/byline",
       "name": "Jane Doe"
       }
   }
   ```

1. 重新运行测试，并且 **`testGetName()`** 现在过去！

   ![测试名称通过](assets/unit-testing/testgetname-pass.png)


## 测试getOccupations() {#testing-get-occupations}

太好了！ 第一个测试已经通过！ 让我们继续并测试 `getOccupations()`. 由于模拟上下文的初始化是在 `@Before setUp()`方法，这适用于所有 `@Test` 本测试用例中的方法，包括 `getOccupations()`.

请记住，此方法必须返回按字母顺序排序的占有列表（降序）存储在occutions属性中。

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
   * **`ctx.currentResource`** 设置当前资源以根据/content/byline处的模拟资源定义评估上下文。 这可确保 **BylineImpl.java** 在模拟资源的上下文中执行。
   * **`ctx.request().adaptTo(Byline.class)`** 通过从模拟请求对象中调整来实例化Byline Sling模型。
   * **`byline.getOccupations()`** 调用我们正在测试的方法， `getOccupations()`，在Byline Sling模型对象上。
   * **`assertEquals(expected, actual)`** 断言预期列表与实际列表相同。

1. 记住，就象 **`getName()`** 在上面， **BylineImplTest.json** 不定义职业，因此如果运行该测试，测试将失败，因为 `byline.getOccupations()` 将返回空列表。

   更新 **BylineImplTest.json** 以包含职业列表，这些职业将按非字母顺序设置，以确保我们的测试验证职业是否按字母顺序排序 **`getOccupations()`**.

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

1. 运行测试，再次通过！ 好像获取已排序的职业可以正常进行！

   ![获取占用通过](assets/unit-testing/testgetoccupations-pass.png)

   *testGetOccupations()通过*

## 测试isEmpty() {#testing-is-empty}

要测试的最后一个方法 **`isEmpty()`**.

测试 `isEmpty()` 很有趣，因为它需要针对各种条件进行测试。 审核 **BylineImpl.java**&#x200B;的 `isEmpty()` 方法必须测试以下条件：

* 当名称为空时返回true
* 当占用为null或空时返回true
* 当图像为null或没有src URL时返回true
* 当名称、占用和图像（带有src URL）存在时，返回false

为此，我们需要创建测试方法，每个测试方法都在中测试特定条件和新的模拟资源结构 `BylineImplTest.json` 来推动这些测试。

此检查允许我们跳过以下时间的测试： `getName()`， `getOccupations()` 和 `getImage()` 为空，因为该状态的预期行为是通过测试的 `isEmpty()`.

1. 第一个测试将测试未设置属性的全新组件的状态。

   将新的资源定义添加到 `BylineImplTest.json`，为其提供语义名称“**空**&quot;

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

   **`"empty": {...}`** 定义名为“empty”的新资源定义，该定义只具有 `jcr:primaryType` 和 `sling:resourceType`.

   记住我们要加载 `BylineImplTest.json` 到 `ctx` 执行中的每个测试方法之前 `@setUp`，因此我们可以在以下位置的测试中立即找到此新资源定义： **/content/empty.**

1. 更新 `testIsEmpty()` 如下所示，将当前资源设置为新&#39;&#39;**空**”模拟资源定义。

   ```java
   @Test
   public void testIsEmpty() {
       ctx.currentResource("/content/empty");
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertTrue(byline.isEmpty());
   }
   ```

   运行测试并确保测试通过。

1. 接下来，创建一组方法，以确保如果任何所需的数据点（名称、职业或图像）为空， `isEmpty()` 返回真。

   对于每个测试，使用离散模拟资源定义，更新 **BylineImplTest.json** ，其中包含其他资源定义 **without-name** 和 **不占用**.

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

   创建以下测试方法以测试每种状态。

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

   **`testIsEmpty()`** 针对空模拟资源定义进行测试，并断言 `isEmpty()` 是真的。

   **`testIsEmpty_WithoutName()`** 针对具有占用但没有名称的模拟资源定义进行测试。

   **`testIsEmpty_WithoutOccupations()`** 针对具有名称但不占用资源的模拟资源定义进行测试。

   **`testIsEmpty_WithoutImage()`** 针对具有名称和占用情况的模拟资源定义进行测试，但将模拟图像设置为返回空值。 请注意，我们要覆盖 `modelFactory.getModelFromWrappedRequest(..)`在中定义的行为 `setUp()` 以确保此调用返回的Image对象为null。 Mockito桩模块功能非常严格，并且不需要重复的代码。 因此我们用这个模型 **`lenient`** 设置明确说明我们正在覆盖 `setUp()` 方法。

   **`testIsEmpty_WithoutImageSrc()`** 针对具有名称和占用情况的模拟资源定义进行测试，但在以下情况下将模拟图像设置为返回空白字符串 `getSrc()` 将调用。

1. 最后，编写测试以确保 **isEmpty()** 正确配置组件后返回false。 对于这种情况，我们可以重用 **/content/byline** 表示完全配置的Byline组件。

   ```java
   @Test
   public void testIsNotEmpty() {
       ctx.currentResource("/content/byline");
       when(image.getSrc()).thenReturn("/content/bio.png");
   
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertFalse(byline.isEmpty());
   }
   ```

1. 现在，运行BylineImplTest.java文件中的所有单元测试，并查看Java™测试报告输出。

![所有测试均通过](./assets/unit-testing/all-tests-pass.png)

## 在构建过程中运行单元测试 {#running-unit-tests-as-part-of-the-build}

执行单元测试，并需要作为maven构建的一部分通过。 这可确保在部署应用程序之前成功通过所有测试。 执行包或安装等Maven目标会自动调用，并需要通过项目中的所有单元测试。

```shell
$ mvn package
```

![mvn包成功](assets/unit-testing/mvn-package-success.png)

```shell
$ mvn package
```

同样，如果将测试方法更改为“失败”，则构建将失败并报告哪些测试失败以及失败原因。

![mvn包失败](assets/unit-testing/mvn-package-fail.png)

## 查看代码 {#review-the-code}

查看完成的代码 [GitHub](https://github.com/adobe/aem-guides-wknd) 或在Git分支上本地查看和部署代码 `tutorial/unit-testing-solution`.
