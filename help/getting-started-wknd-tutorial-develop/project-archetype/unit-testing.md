---
title: 单元测试
description: 本教程涵盖单元测试的实施，该测试将验证在自定义组件教程中创建的署名组件Sling模型的行为。
sub-product: 站点
version: 6.4, 6.5, Cloud Service
type: Tutorial
feature: API、AEM项目原型
topic: 内容管理、开发
role: Developer
level: Beginner
kt: 4089
mini-toc-levels: 1
thumbnail: 30207.jpg
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '3013'
ht-degree: 0%

---


# 单元测试 {#unit-testing}

本教程涵盖单元测试的实施，该测试将验证在[自定义组件](./custom-component.md)教程中创建的署名组件Sling模型的行为。

## 前提条件 {#prerequisites}

查看设置[本地开发环境](overview.md#local-dev-environment)所需的工具和说明。

_如果系统上同时安装了Java 8和Java 11，则VS Code测试运行程序在执行测试时可能会选择较低的Java运行时，从而导致测试失败。如果发生这种情况，请卸载Java 8._

### 入门项目

>[!NOTE]
>
> 如果您成功完成了上一章，则可以重复使用该项目并跳过签出起始项目的步骤。

查看本教程构建的基行代码：

1. 查看[GitHub](https://github.com/adobe/aem-guides-wknd)中的`tutorial/unit-testing-start`分支

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
1. 了解常用于测试AEM代码的框架和工具。
1. 了解在编写单元测试时用于模拟或模拟AEM资源的选项。

## 背景 {#unit-testing-background}

在本教程中，我们将探讨如何为我们的Byline组件的[Sling模型](https://sling.apache.org/documentation/bundles/models.html)(在[创建自定义AEM组件](custom-component.md)中创建)编写[设备测试](https://en.wikipedia.org/wiki/Unit_testing)。 单元测试是使用Java编写的内部版本时测试，用于验证Java代码的预期行为。 每个单元测试通常都较小，并根据预期结果验证方法（或工作单元）的输出。

我们将使用AEM最佳实践，并使用：

* [JUnit 5](https://junit.org/junit5/)
* [Mockito测试框架](https://site.mockito.org/)
* [wcm.io测试框架](https://wcm.io/testing/) (以Apache Sling吊 [床为基础](https://sling.apache.org/documentation/development/sling-mock.html))

## 单元测试和AdobeCloud Manager {#unit-testing-and-adobe-cloud-manager}

[Adobe云管](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/introduction-to-cloud-manager.html?lang=zh-Hans) 理器将单元测试执行和 [代码覆](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/how-to-use/understand-your-test-results.html#code-quality-testing) 盖报告集成到其CI/CD管道中，以帮助鼓励和推广单元测试AEM代码的最佳实践。

虽然单元测试代码是任何代码库的最佳实践，但在使用Cloud Manager时，务必要通过为Cloud Manager运行单元测试来利用其代码质量测试和报告功能。

## Inspect测试Maven依赖项 {#inspect-the-test-maven-dependencies}

第一步是检查Maven依赖项，以支持编写和运行测试。 需要四个依赖项：

1. JUnit5
1. Mockito测试框架
1. Apache Sling Mocks
1. AEM Mocks测试框架（由io.wcm提供）

在使用[AEM Maarcheven类型](project-setup.md)进行设置期间，会将&#x200B;**JUnit5**、**Mockito**&#x200B;和&#x200B;**AEM Mocks**&#x200B;测试依赖项自动添加到项目中。

1. 要查看这些依赖关系，请在&#x200B;**aem-guides-wknd/pom.xml**&#x200B;处打开父Reactor POM，导航到`<dependencies>..</dependencies>`，并确保定义了以下依赖关系：

   ```xml
   <dependencies>
       ...       
       <!-- Testing -->
       <dependency>
           <groupId>org.junit</groupId>
           <artifactId>junit-bom</artifactId>
           <version>5.6.2</version>
           <type>pom</type>
           <scope>import</scope>
       </dependency>
       <dependency>
           <groupId>org.mockito</groupId>
           <artifactId>mockito-core</artifactId>
           <version>3.3.3</version>
           <scope>test</scope>
       </dependency>
       <dependency>
           <groupId>org.mockito</groupId>
           <artifactId>mockito-junit-jupiter</artifactId>
           <version>3.3.3</version>
           <scope>test</scope>
       </dependency>
       <dependency>
           <groupId>junit-addons</groupId>
           <artifactId>junit-addons</artifactId>
           <version>1.4</version>
           <scope>test</scope>
       </dependency>
       <dependency>
           <groupId>io.wcm</groupId>
           <artifactId>io.wcm.testing.aem-mock.junit5</artifactId>
           <!-- Prefer the latest version of AEM Mock Junit5 dependency -->
           <version>3.0.2</version>
           <scope>test</scope>
       </dependency>        
       ...
   </dependencies>
   ```

1. 打开&#x200B;**aem-guides-wknd/core/pom.xml**&#x200B;并查看相应的测试依赖项是否可用：

   ```xml
   ...
   <!-- Testing -->
   <dependency>
       <groupId>org.junit.jupiter</groupId>
       <artifactId>junit-jupiter</artifactId>
       <scope>test</scope>
   </dependency>
   <dependency>
       <groupId>org.mockito</groupId>
       <artifactId>mockito-core</artifactId>
       <scope>test</scope>
   </dependency>
   <dependency>
       <groupId>org.mockito</groupId>
       <artifactId>mockito-junit-jupiter</artifactId>
       <scope>test</scope>
   </dependency>
   <dependency>
       <groupId>junit-addons</groupId>
       <artifactId>junit-addons</artifactId>
       <scope>test</scope>
   </dependency>
   <dependency>
       <groupId>io.wcm</groupId>
       <artifactId>io.wcm.testing.aem-mock.junit5</artifactId>
       <exclusions>
           <exclusion>
               <groupId>org.apache.sling</groupId>
               <artifactId>org.apache.sling.models.impl</artifactId>
           </exclusion>
           <exclusion>
               <groupId>org.slf4j</groupId>
               <artifactId>slf4j-simple</artifactId>
           </exclusion>
       </exclusions>
       <scope>test</scope>
   </dependency>
   <!-- Required to be able to support injection with @Self and @Via -->
   <dependency>
       <groupId>org.apache.sling</groupId>
       <artifactId>org.apache.sling.models.impl</artifactId>
       <version>1.4.4</version>
       <scope>test</scope>
   </dependency>
   ...
   ```

   **core**&#x200B;项目中的并行源文件夹将包含单元测试和任何支持的测试文件。 此&#x200B;**test**&#x200B;文件夹提供了与源代码分离的测试类，但允许测试像与源代码位于同一包中一样进行操作。

## 创建JUnit测试 {#creating-the-junit-test}

单元测试通常使用Java类将1对1映射。 在本章中，我们将为&#x200B;**BylineImpl.java**&#x200B;编写一个JUnit测试，该测试是支持Byline组件的Sling模型。

![单元测试src文件夹](assets/unit-testing/core-src-test-folder.png)

*存储单元测试的位置。*

1. 通过在Java包文件夹结构（用于镜像要测试的Java类的位置）中的`src/test/java`下创建新Java类，为`BylineImpl.java`创建单元测试。

   ![创建新的BylineImplTest.java文件](assets/unit-testing/new-bylineimpltest.png)

   因为我们在测试

   * `src/main/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImpl.java`

   在

   * `src/test/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.java`

   设备测试文件`Test`后缀`BylineImplTest.java`是一种惯例，它允许我们

   1. 轻松将其标识为&#x200B;_`BylineImpl.java`的测试文件_
   1. 但是，也应将测试文件&#x200B;_与要测试的类_&#x200B;区分开来，`BylineImpl.java`



## 查看BylineImplTest.java {#reviewing-bylineimpltest-java}

此时，JUnit测试文件是空Java类。 使用以下代码更新文件：

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

1. 第一个方法`public void setUp() { .. }`用JUnit的`@BeforeEach`进行注释，指示JUnit测试运行器在运行此类中的每个测试方法之前执行此方法。 这为初始化所有测试所需的常见测试状态提供了方便的位置。

2. 后续方法是测试方法，其名称按惯例以`test`为前缀，并标有`@Test`注释。 请注意，默认情况下，由于我们尚未实施这些测试，因此所有测试都将设置为失败。

   首先，我们从一个测试方法开始，针对我们测试的类中的每个公共方法，这样：

   | BylineImpl.java |  | BylineImplTest.java |
   | ------------------|--------------|---------------------|
   | getName() | 测试方式 | testGetName() |
   | getScriptions() | 测试方式 | testGetSchories() |
   | isEmpty() | 测试方式 | testIsEmpty() |

   这些方法可以根据需要进行扩展，我们将在本章的后面部分看到。

   运行此JUnit测试类（也称为JUnit测试案例）时，标有`@Test`的每个方法都将作为测试执行，该测试可以通过或失败。

![生成的BylineImplTest](assets/unit-testing/bylineimpltest-stub-methods.png)

*`core/src/test/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.java`*

1. 右键单击`BylineImplTest.java`文件，然后点按&#x200B;**运行**，以运行JUnit测试案例。
如预期，所有测试都会失败，因为尚未实施。

   ![作为junit测试运行](assets/unit-testing/run-junit-tests.png)

   *右键单击BylineImplTests.java >运行*

## 查看BylineImpl.java {#reviewing-bylineimpl-java}

编写单元测试时，主要采用以下两种方法：

* [TDD或测试驱动开发](https://en.wikipedia.org/wiki/Test-driven_development)，即在开发实施之前，以增量方式编写单元测试；编写测试，编写实现以使测试通过。
* 实施优先开发，包括先开发工作代码，然后编写验证该代码的测试。

在本教程中，使用了后一种方法（因为我们在上一章中已经创建了有效的&#x200B;**BylineImpl.java**）。 因此，我们必须对其公共方法的行为进行审查和理解，并了解其实施细节。 这听上去可能相反，因为良好的测试应该只关心输入和输出，但在AEM中工作时，需要了解各种实施考虑因素，才能构建工作测试。

对于AEM,TDD需要一定的专业技能，最能被精通AEM代码开发和单元测试的AEM开发人员采用。

## 设置AEM测试上下文  {#setting-up-aem-test-context}

为AEM编写的大多数代码都依赖于JCR、Sling或AEM API，而JCR、Sling或API反过来又需要运行AEM的上下文才能正确执行。

由于单元测试是在生成时执行的，因此在运行AEM实例的上下文之外，不存在此类上下文。 为了实现此目的，[wcm.io的AEM Mocks](https://wcm.io/testing/aem-mock/usage.html)会创建模拟上下文，以允许这些API在&#x200B;_中大部分_&#x200B;起作用，就像它们在AEM中运行一样。

1. 在&#x200B;**BylineImplTest.java**&#x200B;中使用&#x200B;**wcm.io的** `AemContext`创建AEM上下文，方法是将其作为使用`@ExtendWith`修饰的JUnit扩展添加到&#x200B;**BylineImplTest.java**&#x200B;文件中。 扩展负责处理所有所需的初始化和清理任务。 为`AemContext`创建一个类变量，该类变量可用于所有测试方法。

   ```java
   import org.junit.jupiter.api.extension.ExtendWith;
   import io.wcm.testing.mock.aem.junit5.AemContext;
   import io.wcm.testing.mock.aem.junit5.AemContextExtension;
   ...
   
   @ExtendWith(AemContextExtension.class)
   class BylineImplTest {
   
       private final AemContext ctx = new AemContext();
   ```

   此变量`ctx`公开了一个模拟AEM上下文，该上下文提供了许多AEM和Sling抽象概念：

   * BylineImpl Sling模型将注册到此上下文中
   * 在此上下文中创建模拟JCR内容结构
   * 可在此上下文中注册自定义OSGi服务
   * 提供各种常见的必需模拟对象和帮助程序，如SlingHttpServletRequest对象、各种模拟Sling和AEM OSGi服务，如ModelFactory、PageManager、Page、Template、ComponentManager、Component、TagManager、Tag等。
      * *请注意，并非这些对象的所有方法都已实施！*
   * 和[更多的](https://wcm.io/testing/aem-mock/usage.html)!

   **`ctx`**&#x200B;对象将用作我们大多数模拟上下文的入口点。

1. 在每个`@Test`方法之前执行的`setUp(..)`方法中，定义一个通用的模拟测试状态：

   ```java
   @BeforeEach
   public void setUp() throws Exception {
       ctx.addModelsForClasses(BylineImpl.class);
       ctx.load().json("/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.json", "/content");
   }
   ```

   * **`addModelsForClasses`** 将要测试的Sling模型注册到模拟AEM上下文中，以便在方法中对其进行实 `@Test` 例化。
   * **`load().json`** 将资源结构加载到模拟上下文中，从而允许代码与这些资源进行交互，就像这些资源是由真实存储库提供的一样。文件&#x200B;**`BylineImplTest.json`**&#x200B;中的资源定义将加载到&#x200B;**/content**&#x200B;下的模拟JCR上下文中。
   * **`BylineImplTest.json`** 尚不存在，因此我们创建它并定义测试所需的JCR资源结构。

1. 表示模拟资源结构的JSON文件存储在&#x200B;**core/src/test/resources**&#x200B;下，其路径与JUnit Java测试文件相同。

   在名为&#x200B;**BylineImplTest.json**&#x200B;的&#x200B;**core/test/resources/com/adobe/aem/guides/wknd/core/models/impl**&#x200B;的新JSON文件中创建，并包含以下内容：

   ```json
   {
       "byline": {
       "jcr:primaryType": "nt:unstructured",
       "sling:resourceType": "wknd/components/content/byline"
       }
   }
   ```

   ![BylineImplTest.json](assets/unit-testing/bylineimpltest-json.png)

   此JSON为Byline组件单元测试定义了一个模拟资源（JCR节点）。 此时，JSON具有表示Byline组件内容资源、 `jcr:primaryType`和`sling:resourceType`所需的最小属性集。

   使用单元测试时，一般规则是创建满足每个测试所需的最小模拟内容、上下文和代码集。 避免在编写测试之前构建完整的模拟上下文的诱惑，因为这通常会产生不需要的工件。

   现在，存在&#x200B;**BylineImplTest.json**&#x200B;时，当执行`ctx.json("/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.json", "/content")`时，模拟资源定义将加载到路径&#x200B;**/content.**&#x200B;的上下文中。

## 测试getName() {#testing-get-name}

既然我们有了基本的模拟上下文设置，让我们编写我们针对&#x200B;**BylineImpl&#39;s getName()**&#x200B;的第一个测试。 此测试必须确保方法&#x200B;**getName()**&#x200B;返回存储在资源“**name&quot;**&#x200B;属性中的正确创作名称。

1. 按如下方式更新&#x200B;**BylineImplTest.java**&#x200B;中的&#x200B;**testGetName**()方法：

   ```java
   import com.adobe.aem.guides.wknd.core.components.Byline;
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

   * **`String expected`** 设置预期值。我们将此参数设置为“**Jane Done**”。
   * **`ctx.currentResource`** 设置模拟资源的上下文以评估其代码，因此将其设置为加载 **模拟署名内容资源的位置/content/** bylineas。
   * **`Byline byline`** 通过从模拟请求对象中对其进行改编，来实例化Byline Sling模型。
   * **`String actual`** 在Byline Sling模型对象上调 `getName()`用我们正在测试的方法。
   * **`assertEquals`** 声明预期值与署名Sling模型对象返回的值匹配。如果这些值不相等，测试将失败。

1. 运行测试……并且失败，出现`NullPointerException`。

   请注意，此测试不会失败，因为我们从未在模拟JSON中定义`name`属性，该属性将导致测试失败，但测试执行尚未达到该点！ 此测试因署名对象本身上的`NullPointerException`而失败。

1. 在`BylineImpl.java`中，如果`@PostConstruct init()`引发异常，它会阻止Sling模型实例化，并导致该Sling模型对象为空。

   ```java
   @PostConstruct
   private void init() {
       image = modelFactory.getModelFromWrappedRequest(request, request.getResource(), Image.class);
   }
   ```

   结果显示，虽然ModelFactory OSGi服务是通过`AemContext`（通过Apache Sling Context）提供的，但并非所有方法都得到实施，包括`getModelFromWrappedRequest(...)`，该在BylineImpl的`init()`方法中调用。 这会导致[AbstractMethodError](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/AbstractMethodError.html)，该函数在术语中导致`init()`失败，因此`ctx.request().adaptTo(Byline.class)`的适配是空对象。

   由于提供的吊床无法容纳我们的代码，因此我们必须自己实施模拟上下文。为此，我们可以使用Mockito创建一个模拟ModelFactory对象，当`getModelFromWrappedRequest(...)`被调用时，该对象将返回一个模拟Image对象。

   由于为了甚至实例化Byline Sling模型，此模拟上下文必须就位，因此我们可以将其添加到`@Before setUp()`方法中。 我们还需要将`MockitoExtension.class`添加到&#x200B;**BylineImplTest**&#x200B;类上方的`@ExtendWith`注释中。

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

   * **`@ExtendWith({AemContextExtension.class, MockitoExtension.class})`** 标记要使用Mockito JUnit Jupiter扩展运 [行的测](https://www.javadoc.io/page/org.mockito/mockito-junit-jupiter/latest/org/mockito/junit/jupiter/MockitoExtension.html) 试案例类，该扩展允许使用@Mock批注在类级别定义模拟对象。
   * **`@Mock private Image`** 创建类型的模拟对象 `com.adobe.cq.wcm.core.components.models.Image`。请注意，这是在类级别定义的，以便`@Test`方法可以根据需要更改其行为。
   * **`@Mock private ModelFactory`** 创建ModelFactory类型的模拟对象。请注意，这是纯Mockito模型，没有在其上实施任何方法。 请注意，这是在类级别定义的，以便`@Test`方法可以根据需要更改其行为。
   * **`when(modelFactory.getModelFromWrappedRequest(..)`** 在模拟ModelFactory对象 `getModelFromWrappedRequest(..)` 上调用时，注册模拟行为。在`thenReturn (..)`中定义的结果是返回模拟图像对象。 请注意，仅在以下情况下才会调用此行为：第1个参数等于`ctx`的请求对象，第2个参数是任何资源对象，第3个参数必须是核心组件图像类。 我们接受任何资源，因为在整个测试过程中，我们将将`ctx.currentResource(...)`设置为&#x200B;**BylineImplTest.json**&#x200B;中定义的各种模拟资源。 请注意，我们添加了&#x200B;**enlidge()**&#x200B;严格，因为我们以后将要覆盖ModelFactory的此行为。
   * **`ctx.registerService(..)`。** 在AemContext中注册模拟ModelFactory对象，其服务排名最高。这是必需的，因为BylineImpl的`init()`中使用的ModelFactory是通过`@OSGiService ModelFactory model`字段插入的。 为了使AemContext插入&#x200B;**我们的**&#x200B;模拟对象（用于处理`getModelFromWrappedRequest(..)`的调用），我们必须将其注册为该类型(ModelFactory)的最高级别服务。

1. 重新运行测试，然后再次失败，但这一次，消息明确了测试失败的原因。

   ![测试名称失败断言](assets/unit-testing/testgetname-failure-assertion.png)

   *testGetName()由于断言失败*

   我们收到&#x200B;**AssertionError**，这表示测试中的断言条件失败，它告诉我们&#x200B;**预期值为“Jane Doe”**，但&#x200B;**实际值为null**。 这很有意义，因为尚未添加“**name&quot;**&#x200B;属性来模拟&#x200B;**BylineImplTest.json**&#x200B;中的&#x200B;**/content/byline**&#x200B;资源定义，因此，我们添加该属性：

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

1. 重新运行测试，**`testGetName()`**&#x200B;现在通过！

   ![测试名称通过](assets/unit-testing/testgetname-pass.png)


## 测试getSchories() {#testing-get-occupations}

很好！ 我们的第一个测试通过了！ 让我们继续测试`getOccupations()`。 由于模拟上下文的初始化是在`@Before setUp()`方法中进行的，因此此测试案例中的所有`@Test`方法（包括`getOccupations()`）均可使用此模拟上下文。

请记住，此方法必须返回按字母顺序排序的职业列表（降序），这些职业列表存储在职业属性中。

1. 按如下方式更新&#x200B;**`testGetOccupations()`**:

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
   * **`ctx.currentResource`** 设置当前资源以根据/content/byline中的模拟资源定义评估上下文。这可确保在模拟资源的上下文中执行&#x200B;**BylineImpl.java**。
   * **`ctx.request().adaptTo(Byline.class)`** 通过从模拟请求对象中对其进行改编，来实例化Byline Sling模型。
   * **`byline.getOccupations()`** 在Byline Sling模型对象上调 `getOccupations()`用我们正在测试的方法。
   * **`assertEquals(expected, actual)`** 断言预期列表与实际列表相同。

1. 请记住，就像上面的&#x200B;**`getName()`**&#x200B;一样，**BylineImplTest.json**&#x200B;不定义职业，因此如果运行此测试，则此测试将失败，因为`byline.getOccupations()`将返回空列表。

   更新&#x200B;**BylineImplTest.json**&#x200B;以包含职业列表，这些职业将按非字母顺序设置，以确保我们的测试验证职业是否按&#x200B;**`getOccupations()`**&#x200B;的字母顺序排序。

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

测试&#x200B;**`isEmpty()`**&#x200B;的最后一个方法。

测试`isEmpty()`很有意思，因为它需要测试各种情况。 查看&#x200B;**BylineImpl.java**&#x200B;的`isEmpty()`方法，必须测试以下条件：

* 名称为空时返回true
* 当职业为空时返回true
* 当图像为null或没有src URL时，返回true
* 当名称、职业和图像（使用src URL）存在时，返回false

为此，我们需要创建新的测试方法，每个测试都测试特定条件以及`BylineImplTest.json`中的新模拟资源结构，以驱动这些测试。

请注意，当`getName()`、`getOccupations()`和`getImage()`为空时，此检查允许我们跳过测试，因为该状态的预期行为是通过`isEmpty()`进行测试的。

1. 第一个测试将测试没有设置属性的全新组件的条件。

   向`BylineImplTest.json`添加新的资源定义，为其提供语义名称“**empty**”

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

   **`"empty": {...}`** 定义名为“empty”的新资源定义，该定义只具有 `jcr:primaryType` 和 `sling:resourceType`。

   请记住，在`@setUp`中执行每个测试方法之前，我们会先将`BylineImplTest.json`加载到`ctx`中，因此，在&#x200B;**/content/empty的测试中，我们可以立即使用此新的资源定义。**

1. 按如下方式更新`testIsEmpty()`，将当前资源设置为新的“**empty**”模拟资源定义。

   ```java
   @Test
   public void testIsEmpty() {
       ctx.currentResource("/content/empty");
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertTrue(byline.isEmpty());
   }
   ```

   运行测试并确保测试通过。

1. 接下来，创建一组方法，以确保如果任何必需的数据点（名称、职位或图像）为空，则`isEmpty()`会返回true。

   对于每个测试，都使用离散的模拟资源定义，使用&#x200B;**without-name**&#x200B;和&#x200B;**without-shorgies**&#x200B;的附加资源定义更新&#x200B;**BylineImplTest.json**。

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

   **`testIsEmpty()`** 针对空的模拟资源定义进行测试，并声 `isEmpty()` 明为true。

   **`testIsEmpty_WithoutName()`** 针对有职业但没有名字的模拟资源定义进行测试。

   **`testIsEmpty_WithoutOccupations()`** 针对名为但没有职业的模拟资源定义进行测试。

   **`testIsEmpty_WithoutImage()`** 对名称和职业的模拟资源定义进行测试，但将模拟图像设置为返回空值。请注意，我们要覆盖在`setUp()`中定义的`modelFactory.getModelFromWrappedRequest(..)`行为，以确保此调用返回的Image对象为null。 模仿作品的小作品功能很严格，不需要重复的代码。 因此，我们使用&#x200B;**`lenient`**&#x200B;设置来设置模型，以明确地注意我们正在覆盖`setUp()`方法中的行为。

   **`testIsEmpty_WithoutImageSrc()`** 针对名称和职业的模拟资源定义进行测试，但会将模拟图像设置为在调用时返回空 `getSrc()` 字符串。

1. 最后，编写一个测试，以确保在正确配置组件后，**isEmpty()**&#x200B;返回false。 对于此条件，我们可以重新使用&#x200B;**/content/byline**，它表示完全配置的Byline组件。

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

在[GitHub](https://github.com/adobe/aem-guides-wknd)上查看完成的代码，或在Git浏览器`tutorial/unit-testing-solution`上的本地查看并部署代码。
