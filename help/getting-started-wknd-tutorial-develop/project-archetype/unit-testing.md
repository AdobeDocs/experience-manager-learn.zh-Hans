---
title: 单元测试
description: 实施单元测试，以验证在自定义组件教程中创建的Byline组件的Sling模型的行为。
version: Experience Manager 6.5, Experience Manager as a Cloud Service
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
duration: 706
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '2923'
ht-degree: 0%

---

# 单元测试 {#unit-testing}

本教程介绍单元测试的实施，该单元测试将验证在[自定义组件](./custom-component.md)教程中创建的Byline组件的Sling模型的行为。

## 先决条件 {#prerequisites}

查看设置[本地开发环境](overview.md#local-dev-environment)所需的工具和说明。

_如果系统上同时安装了Java™ 8和Java™ 11，则VS代码测试运行程序在执行测试时可能会选择较低的Java™运行时，从而导致测试失败。 如果发生这种情况，请卸载Java™ 8._

### 入门项目

>[!NOTE]
>
> 如果成功完成了上一章，则可以重用该项目并跳过签出入门项目的步骤。

查看本教程所基于的基本行代码：

1. 从[GitHub](https://github.com/adobe/aem-guides-wknd)中签出`tutorial/unit-testing-start`分支

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
   > 如果使用AEM 6.5或6.4，请将`classic`配置文件附加到任何Maven命令。

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

您始终可以在[GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/unit-testing-start)上查看完成的代码，或通过切换到分支`tutorial/unit-testing-start`在本地签出代码。

## 目标

1. 了解单元测试的基础知识。
1. 了解通常用于测试AEM代码的框架和工具。
1. 了解在编写单元测试时模拟或模拟AEM资源的选项。

## 背景 {#unit-testing-background}

在本教程中，我们将了解如何为署名组件的[Sling模型](https://sling.apache.org/documentation/bundles/models.html)&#x200B;(在[创建自定义AEM组件](custom-component.md)中创建)编写[单元测试](https://en.wikipedia.org/wiki/Unit_testing)。 单元测试是用Java™编写的构建时测试，用于验证Java™代码的预期行为。 每个单元测试通常很小，可根据预期结果验证方法（或工作单元）的输出。

我们使用AEM最佳实践，并采用：

* [JUnit 5](https://junit.org/junit5/)
* [Mockito测试框架](https://site.mockito.org/)
* [wcm.io测试框架](https://wcm.io/testing/) （基于[Apache Sling Mocks](https://sling.apache.org/documentation/development/sling-mock.html)构建）

## 单元测试和Adobe Cloud Manager {#unit-testing-and-adobe-cloud-manager}

[Adobe Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/introduction.html)在其CI/CD管道中集成了单元测试执行和[代码覆盖率报告](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/code-quality-testing.html)，以帮助鼓励和推广单元测试AEM代码的最佳实践。

虽然单元测试代码是任何代码库的一个良好实践，但在使用Cloud Manager时，通过为Cloud Manager提供单元测试来利用其代码质量测试和报告工具非常重要。

## 更新测试Maven依赖项 {#inspect-the-test-maven-dependencies}

第一步是检查Maven依赖项以支持编写和运行测试。 需要四个依赖项：

1. JUnit5
1. Mockito测试框架
1. Apache Sling Mocks
1. AEM Mocks Test Framework （由io.wcm提供）

使用[AEM Maven原型](project-setup.md)在安装期间，会自动将&#x200B;**JUnit5**、**Mockito和&#x200B;**AEM Mocks**&#x200B;测试依赖项添加到项目中。

1. 要查看这些依赖项，请打开位于&#x200B;**aem-guides-wknd/pom.xml**&#x200B;的父Reactor POM，导航到`<dependencies>..</dependencies>`并查看`<!-- Testing -->`下io.wcm的JUnit、Mockito、Apache Sling Mocks和AEM Mock Tests的依赖项。
1. 确保`io.wcm.testing.aem-mock.junit5`设置为&#x200B;**4.1.0**：

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
   > 原型&#x200B;**35**&#x200B;生成版本为&#x200B;**4.1.8**&#x200B;的`io.wcm.testing.aem-mock.junit5`项目。 请降级到&#x200B;**4.1.0**&#x200B;以遵循本章的其余部分。

1. 打开&#x200B;**aem-guides-wknd/core/pom.xml**&#x200B;并查看相应的测试依赖项是否可用。

   **core**&#x200B;项目中的并行源文件夹将包含单元测试和任何支持的测试文件。 此&#x200B;**test**&#x200B;文件夹提供了测试类与源代码的分离，但允许测试像它们与源代码位于同一包中一样运行。

## 创建JUnit测试 {#creating-the-junit-test}

单元测试通常使用Java™类将1映射到1。 在本章中，我们将为&#x200B;**BylineImpl.java**&#x200B;编写JUnit测试，该程序是支持Byline组件的Sling模型。

![单元测试src文件夹](assets/unit-testing/core-src-test-folder.png)

*存储单元测试的位置。*

1. 通过在Java™包文件夹结构的`src/test/java`下创建一个新的Java™类来为`BylineImpl.java`创建单元测试，该结构镜像了要测试的Java™类的位置。

   ![创建新的BylineImplTest.java文件](assets/unit-testing/new-bylineimpltest.png)

   因为我们正在测试

   * `src/main/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImpl.java`

   在上创建相应的单元测试Java™类

   * `src/test/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.java`

   单元测试文件`BylineImplTest.java`上的`Test`后缀是一个约定，允许我们

   1. 轻松将其标识为&#x200B;_`BylineImpl.java`的测试文件_
   1. 但是，还要区分测试文件&#x200B;_和_&#x200B;所测试的类`BylineImpl.java`

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

1. 第一个方法`public void setUp() { .. }`使用JUnit的`@BeforeEach`进行注释，它指示JUnit测试运行程序在运行该类中的每个测试方法之前执行此方法。 这为初始化所有测试所需的通用测试状态提供了一个方便的位置。

1. 后续方法是测试方法，其名称按惯例以`test`为前缀，并以`@Test`注释标记。 请注意，默认情况下，我们的所有测试都设置为失败，因为尚未实施它们。

   首先，我们先对我们测试的类中的每个公共方法使用一个测试方法，因此：

   | BylineImpl.java |              | BylineImplTest.java |
   | ------------------|--------------|---------------------|
   | getName() | 测试方 | testGetName() |
   | getOccupations() | 测试方 | testGetOccupations() |
   | isEmpty() | 测试方 | testIsEmpty() |

   这些方法可根据需要进行扩展，我们将在本章的后面部分中看到。

   运行此JUnit测试类（也称为JUnit测试用例）时，每个标记为`@Test`的方法将作为测试执行，测试可能会通过或失败。

![生成的BylineImplTest](assets/unit-testing/bylineimpltest-stub-methods.png)

*`core/src/test/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.java`*

1. 通过右键单击`BylineImplTest.java`文件并点按&#x200B;**运行**来运行JUnit测试用例。
如预期的那样，所有测试都会失败，因为它们尚未实施。

   ![作为junit测试运行](assets/unit-testing/run-junit-tests.png)

   *右键单击BylineImplTests.java > Run*

## 查看BylineImpl.java {#reviewing-bylineimpl-java}

在编写单元测试时，有两种主要方法：

* [TDD或测试驱动开发](https://en.wikipedia.org/wiki/Test-driven_development)，它涉及在开发实现之前增量写入单元测试；请编写测试，然后编写实现以使测试通过。
* 以实施为先的开发，包括先开发工作代码，然后编写测试来验证该代码。

在本教程中，将使用后一种方法（因为我们在上一章中创建了有效的&#x200B;**BylineImpl.java**）。 因此，我们必须回顾和了解其公共方法的行为，同时也要了解其实施细节。 这听起来可能恰恰相反，因为良好的测试应该只关注输入和输出，然而在AEM中工作时，需要理解各种实施考虑因素，才能构建工作测试。

在AEM的上下文中，TDD需要一定程度的专业知识，并且最能被精通AEM开发和AEM代码单元测试的AEM开发人员采用。

## 设置AEM测试上下文  {#setting-up-aem-test-context}

大多数为AEM编写的代码依赖于JCR、Sling或AEM API，这反过来又需要运行的AEM的上下文才能正确执行。

由于单元测试是在构建时执行的，因此在正在运行的AEM实例的上下文之外，没有此类上下文。 为此，[wcm.io的AEM Mocks](https://wcm.io/testing/aem-mock/usage.html)创建了模拟上下文，使这些API _大部分_&#x200B;可以像在AEM中运行一样。

1. 在&#x200B;**BylineImplTest.java**&#x200B;中使用&#x200B;**wcm.io的** `AemContext`创建AEM上下文，方法是将其添加为使用`@ExtendWith`修饰的JUnit扩展名到&#x200B;**BylineImplTest.java**&#x200B;文件中。 该扩展会处理所有所需的初始化和清理任务。 为`AemContext`创建一个可用于所有测试方法的类变量。

   ```java
   import org.junit.jupiter.api.extension.ExtendWith;
   import io.wcm.testing.mock.aem.junit5.AemContext;
   import io.wcm.testing.mock.aem.junit5.AemContextExtension;
   ...
   
   @ExtendWith(AemContextExtension.class)
   class BylineImplTest {
   
       private final AemContext ctx = new AemContext();
   ```

   此变量`ctx`公开了一个模拟AEM上下文，该上下文提供了一些AEM和Sling抽象：

   * BylineImpl Sling模型已注册到此上下文中
   * 在此上下文中创建模拟JCR内容结构
   * 可在此上下文中注册自定义OSGi服务
   * 提供各种常见的必需模拟对象和帮助程序，如SlingHttpServletRequest对象；各种模拟Sling和AEM OSGi服务，如ModelFactory、PageManager、Page、Template、ComponentManager、Component、TagManager、Tag等。
      * *并非这些对象的所有方法都已实现！*
   * 以及[更多](https://wcm.io/testing/aem-mock/usage.html)！

   **`ctx`**&#x200B;对象将作为大多数模拟上下文的入口点。

1. 在每个`@Test`方法之前执行的`setUp(..)`方法中，定义一个常见的模拟测试状态：

   ```java
   @BeforeEach
   public void setUp() throws Exception {
       ctx.addModelsForClasses(BylineImpl.class);
       ctx.load().json("/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.json", "/content");
   }
   ```

   * **`addModelsForClasses`**&#x200B;将要测试的Sling模型注册到模拟AEM上下文中，以便在`@Test`方法中实例化。
   * **`load().json`**&#x200B;将资源结构加载到模拟上下文中，允许代码与这些资源交互，就像它们由真实存储库提供一样。 文件&#x200B;**`BylineImplTest.json`**&#x200B;中的资源定义已加载到&#x200B;**/content**&#x200B;下的模拟JCR上下文中。
   * **`BylineImplTest.json`**&#x200B;尚不存在，因此让我们创建它并定义测试所需的JCR资源结构。

1. 表示模拟资源结构的JSON文件存储在&#x200B;**core/src/test/resources**&#x200B;下，遵循与JUnit Java™测试文件相同的包路径。

   在`core/test/resources/com/adobe/aem/guides/wknd/core/models/impl`处创建名为&#x200B;**BylineImplTest.json**&#x200B;的JSON文件，该文件包含以下内容：

   ```json
   {
       "byline": {
       "jcr:primaryType": "nt:unstructured",
       "sling:resourceType": "wknd/components/content/byline"
       }
   }
   ```

   ![BylineImplTest.json](assets/unit-testing/bylineimpltest-json.png)

   此JSON为Byline组件单元测试定义了一个模拟资源（JCR节点）。 此时，JSON具有表示Byline组件内容资源所需的最小属性集`jcr:primaryType`和`sling:resourceType`。

   处理单元测试时的一般规则是创建满足每个测试所需的最小模拟内容、上下文和代码集。 避免在编写测试之前构建完整的模拟上下文的诱惑，因为这通常会导致不需要的工件。

   现在存在&#x200B;**BylineImplTest.json**，执行`ctx.json("/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.json", "/content")`时，模拟资源定义将加载到路径&#x200B;**/content.**&#x200B;处的上下文中

## 测试getName() {#testing-get-name}

现在，我们有了基本的模拟上下文设置，让我们为&#x200B;**BylineImpl的getName()**&#x200B;编写第一个测试。 此测试必须确保方法&#x200B;**getName()**&#x200B;返回存储在资源的“**name”**&#x200B;属性中的正确编写的名称。

1. 按如下方式更新&#x200B;**BylineImplTest.java**&#x200B;中的&#x200B;**testGetName**()方法：

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

   * **`String expected`**&#x200B;设置预期值。 我们将此项设置为&quot;**Jane Done**&quot;。
   * **`ctx.currentResource`**&#x200B;设置模拟资源的上下文以评估代码，因此设置为&#x200B;**/content/byline**，因为这是加载模拟署名内容资源的位置。
   * **`Byline byline`**&#x200B;通过从模拟请求对象中调整来实例化署名Sling模型。
   * **`String actual`**&#x200B;在Byline Sling模型对象上调用我们正在测试的方法`getName()`。
   * **`assertEquals`**&#x200B;声明预期值与署名Sling模型对象返回的值匹配。 如果这些值不相等，测试将失败。

1. 运行测试……但测试失败，出现`NullPointerException`。

   此测试不会失败，因为我们从未在模拟JSON中定义`name`属性，这将导致测试失败，但测试执行尚未到达该点！ 此测试失败，因为署名对象本身存在`NullPointerException`。

1. 在`BylineImpl.java`中，如果`@PostConstruct init()`引发异常，则会阻止Sling模型实例化，并导致该Sling模型对象为空。

   ```java
   @PostConstruct
   private void init() {
       image = modelFactory.getModelFromWrappedRequest(request, request.getResource(), Image.class);
   }
   ```

   结果发现，虽然ModelFactory OSGi服务是通过`AemContext`（通过Apache Sling Context）提供的，但并非所有方法都已实现，包括在BylineImpl的`init()`方法中调用的`getModelFromWrappedRequest(...)`。 这会导致[AbstractMethodError](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/AbstractMethodError.html)，它导致`init()`失败，因此`ctx.request().adaptTo(Byline.class)`的自适应结果为null对象。

   由于提供的模拟无法容纳我们的代码，因此我们必须自己实现模拟上下文。为此，我们可以使用Mockito创建模拟ModelFactory对象，当对其调用`getModelFromWrappedRequest(...)`时，它会返回模拟Image对象。

   由于甚至要实例化署名Sling模型，此模拟上下文必须就位，因此我们可以将其添加到`@Before setUp()`方法。 我们还需要将`MockitoExtension.class`添加到&#x200B;**BylineImplTest**&#x200B;类上方的`@ExtendWith`批注。

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

   * **`@ExtendWith({AemContextExtension.class, MockitoExtension.class})`**&#x200B;将测试用例类标记为使用[Mockito JUnit Jupiter扩展](https://www.javadoc.io/static/org.mockito/mockito-junit-jupiter/4.11.0/org/mockito/junit/jupiter/MockitoExtension.html)运行，该扩展允许使用@Mock注释在类级别定义模拟对象。
   * **`@Mock private Image`**&#x200B;创建类型为`com.adobe.cq.wcm.core.components.models.Image`的模拟对象。 这是在类级别上定义的，这样`@Test`方法就可以根据需要更改其行为。
   * **`@Mock private ModelFactory`**&#x200B;创建ModelFactory类型的模拟对象。 这是一个纯粹的Mockito模拟，没有实现任何方法。 这是在类级别上定义的，这样`@Test`方法就可以根据需要更改其行为。
   * **`when(modelFactory.getModelFromWrappedRequest(..)`**&#x200B;在模拟ModelFactory对象上调用`getModelFromWrappedRequest(..)`时为其注册模拟行为。 在`thenReturn (..)`中定义的结果是返回模拟图像对象。 仅在以下情况下调用此行为：第一个参数等于`ctx`的请求对象，第二个参数是任何Resource对象，第三个参数必须是核心组件Image类。 我们接受任何资源，因为在整个测试中，我们将`ctx.currentResource(...)`设置为&#x200B;**BylineImplTest.json**&#x200B;中定义的各种模拟资源。 请注意，我们添加了&#x200B;**lenient()**&#x200B;严格性，因为稍后我们将要覆盖ModelFactory的此行为。
   * **`ctx.registerService(..)`。**&#x200B;将模拟ModelFactory对象注册到AemContext中，具有最高的服务等级。 这是必需的，因为BylineImpl的`init()`中使用的ModelFactory是通过`@OSGiService ModelFactory model`字段注入的。 为了使AemContext注入&#x200B;**我们的**&#x200B;模拟对象（该对象处理对`getModelFromWrappedRequest(..)`的调用），我们必须将其注册为该类型的最高级别服务(ModelFactory)。

1. 重新运行测试，再次失败，但这次消息清楚地表明了失败的原因。

   ![测试名称失败断言](assets/unit-testing/testgetname-failure-assertion.png)

   由于断言&#x200B;*，* testGetName()失败

   我们收到一个&#x200B;**AssertionError**，表示测试中的断言条件失败，它告诉我们&#x200B;**预期值为“Jane Doe”**，但&#x200B;**实际值为null**。 这是有道理的，因为“**name”**&#x200B;属性尚未添加到&#x200B;**BylineImplTest.json**&#x200B;中的模拟&#x200B;**/content/byline**&#x200B;资源定义中，因此让我们添加它：

1. 更新&#x200B;**BylineImplTest.json**&#x200B;以定义`"name": "Jane Doe".`

   ```json
   {
       "byline": {
       "jcr:primaryType": "nt:unstructured",
       "sling:resourceType": "wknd/components/content/byline",
       "name": "Jane Doe"
       }
   }
   ```

1. 重新运行测试，现在&#x200B;**`testGetName()`**&#x200B;通过！

   ![测试名称通过](assets/unit-testing/testgetname-pass.png)


## 测试getOccupations() {#testing-get-occupations}

太好了！ 第一个测试已经通过！ 让我们继续并测试`getOccupations()`。 由于模拟上下文的初始化已在`@Before setUp()`方法中完成，因此该测试用例中的所有`@Test`方法均可使用此方法，包括`getOccupations()`。

请记住，此方法必须返回按字母顺序排序的占有列表（降序）存储在occutions属性中。

1. 按如下方式更新&#x200B;**`testGetOccupations()`**：

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

   * **`List<String> expected`**&#x200B;定义预期的结果。
   * **`ctx.currentResource`**&#x200B;将当前资源设置为根据/content/byline处的模拟资源定义评估上下文。 这可确保在模拟资源的上下文中执行&#x200B;**BylineImpl.java**。
   * **`ctx.request().adaptTo(Byline.class)`**&#x200B;通过从模拟请求对象中调整来实例化署名Sling模型。
   * **`byline.getOccupations()`**&#x200B;在Byline Sling模型对象上调用我们正在测试的方法`getOccupations()`。
   * **`assertEquals(expected, actual)`**&#x200B;声明预期列表与实际列表相同。

1. 请记住，与上述&#x200B;**`getName()`**&#x200B;一样，**BylineImplTest.json**&#x200B;未定义占用，因此如果运行该测试，测试将失败，因为`byline.getOccupations()`将返回空列表。

   更新&#x200B;**BylineImplTest.json**&#x200B;以包含职业列表，这些职业按非字母顺序设置，以确保我们的测试验证职业是否按&#x200B;**`getOccupations()`**&#x200B;的字母顺序排序。

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

   ![获取占用阶段](assets/unit-testing/testgetoccupations-pass.png)

   *testGetOccupations()通过*

## 测试isEmpty() {#testing-is-empty}

上次测试&#x200B;**`isEmpty()`**&#x200B;的方法。

测试`isEmpty()`很有趣，因为它需要测试各种条件。 正在审核&#x200B;**BylineImpl.java**&#x200B;的`isEmpty()`方法，必须测试以下条件：

* 当名称为空时返回true
* 当占用为null或空时返回true
* 当图像为null或没有src URL时返回true
* 当名称、占用和图像（带有src URL）存在时，返回false

为此，我们需要创建测试方法，每个测试方法在`BylineImplTest.json`中测试特定条件和新的模拟资源结构以驱动这些测试。

此检查允许我们在`getName()`、`getOccupations()`和`getImage()`为空时跳过测试，因为该状态的预期行为是通过`isEmpty()`测试的。

1. 第一个测试将测试未设置属性的全新组件的状态。

   向`BylineImplTest.json`添加新的资源定义，赋予其语义名称“**empty**”

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

   **`"empty": {...}`**&#x200B;定义名为“empty”的新资源定义，该定义只具有`jcr:primaryType`和`sling:resourceType`。

   请记住，我们在`@setUp`中执行每个测试方法之前将`BylineImplTest.json`加载到`ctx`中，因此我们在&#x200B;**/content/empty.**&#x200B;的测试中立即可以使用此新资源定义。

1. 按如下方式更新`testIsEmpty()`，将当前资源设置为新的&quot;**empty**&quot;模拟资源定义。

   ```java
   @Test
   public void testIsEmpty() {
       ctx.currentResource("/content/empty");
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertTrue(byline.isEmpty());
   }
   ```

   运行测试并确保测试通过。

1. 接下来，创建一组方法，以确保如果任何所需的数据点（名称、占有情况或图像）为空，`isEmpty()`将返回true。

   对于使用离散模拟资源定义的每个测试，使用不带 — name **的**&#x200B;和&#x200B;**不带 — occupations**&#x200B;的其他资源定义更新&#x200B;**BylineImplTest.json**。

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

   **`testIsEmpty()`**&#x200B;针对空模拟资源定义进行测试，并断言`isEmpty()`为true。

   针对具有占用但没有名称的模拟资源定义进行&#x200B;**`testIsEmpty_WithoutName()`**&#x200B;测试。

   针对名称为但无占用空间的模拟资源定义进行&#x200B;**`testIsEmpty_WithoutOccupations()`**&#x200B;测试。

   **`testIsEmpty_WithoutImage()`**&#x200B;针对具有名称和占用情况的模拟资源定义进行测试，但将模拟图像设置为返回空值。 请注意，我们要覆盖`setUp()`中定义的`modelFactory.getModelFromWrappedRequest(..)`行为，以确保此调用返回的图像对象为null。 Mockito桩模块功能非常严格，并且不需要重复的代码。 因此，我们将模拟设置为&#x200B;**`lenient`**&#x200B;设置，以明确说明我们正在覆盖`setUp()`方法中的行为。

   **`testIsEmpty_WithoutImageSrc()`**&#x200B;针对具有名称和占用情况的模拟资源定义进行测试，但设置模拟图像以在调用`getSrc()`时返回空白字符串。

1. 最后，编写测试以确保&#x200B;**isEmpty()**&#x200B;在正确配置组件时返回false。 对于这种情况，我们可以重用&#x200B;**/content/byline**，它表示完全配置的Byline组件。

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

在[GitHub](https://github.com/adobe/aem-guides-wknd)上查看完成的代码，或在Git分支`tutorial/unit-testing-solution`上本地查看和部署代码。
