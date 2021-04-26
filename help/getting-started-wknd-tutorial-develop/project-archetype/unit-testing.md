---
title: 单元测试
description: 本教程涵盖单位测试的实现，该测试用于验证在自定义组件教程中创建的Byline组件的Sling模型的行为。
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 4089
mini-toc-levels: 1
thumbnail: 30207.jpg
feature: API、AEM Project Archetype
topic: 内容管理，开发
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: e8c36a85bc47adbf76e614f245c3f47d7a50826e
workflow-type: tm+mt
source-wordcount: '3016'
ht-degree: 0%

---


# 单元测试{#unit-testing}

本教程涵盖单位测试的实现，该测试用于验证在[自定义组件](./custom-component.md)教程中创建的Byline组件的Sling模型的行为。

## 前提条件 {#prerequisites}

查看设置[本地开发环境](overview.md#local-dev-environment)所需的工具和说明。

_如果系统上同时安装了Java 8和Java 11，则VS代码测试运行程序在执行测试时可能会选择较低的Java运行时，从而导致测试失败。如果发生这种情况，请卸载Java 8._

### 入门项目

>[!NOTE]
>
> 如果您成功完成了上一章，则可以重复使用项目并跳过签出入门项目的步骤。

查看教程构建的基行代码：

1. 查看[GitHub](https://github.com/adobe/aem-guides-wknd)中的`tutorial/unit-testing-start`分支

   ```shell
   $ cd aem-guides-wknd
   $ git checkout tutorial/unit-testing-start
   ```

1. 使用Maven技能将代码库部署到本地AEM实例：

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > 如果使用AEM 6.5或6.4，请将`classic`用户档案附加到任何Maven命令。

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

您始终可以在[GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/unit-testing-start)上视图完成的代码，或通过切换到分支`tutorial/unit-testing-start`在本地签出代码。

## 目标

1. 了解单元测试的基础知识。
1. 了解常用于测试AEM代码的框架和工具。
1. 了解编写单元测试时模仿或模拟AEM资源的选项。

## 背景 {#unit-testing-background}

在本教程中，我们将探索如何为Byline组件的[Sling Model](https://sling.apache.org/documentation/bundles/models.html)(在[创建自定义AEM组件](custom-component.md)中创建)编写[Unit Tests](https://en.wikipedia.org/wiki/Unit_testing)。 单元测试是使用Java编写的构建时测试，用于验证Java代码的预期行为。 每个单元测试通常较小，并根据预期结果验证方法（或工作单元）的输出。

我们将使用AEM最佳实践，并使用：

* [JUnit 5](https://junit.org/junit5/)
* [Mockito Testing Framework](https://site.mockito.org/)
* [wcm.io测试框架](https://wcm.io/testing/) (构建于Apache  [Sling Mocks上](https://sling.apache.org/documentation/development/sling-mock.html))

## 设备测试和Adobe云管理器{#unit-testing-and-adobe-cloud-manager}

[Adobe Cloud ](https://docs.adobe.com/content/help/zh-Hans/experience-manager-cloud-manager/using/introduction-to-cloud-manager.html) Manager将单元测试执行和代 [码覆](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/how-to-use/understand-your-test-results.html#code-quality-testing) 盖报告集成到其CI/CD渠道中，以帮助鼓励和推广单元测试AEM代码的最佳实践。

虽然单元测试代码是任何代码库的良好实践，但在使用Cloud Manager时，通过提供Cloud Manager运行的单元测试，充分利用其代码质量测试和报告功能非常重要。

## Inspect测试Maven依赖项{#inspect-the-test-maven-dependencies}

第一步是检查Maven依赖项以支持编写和运行测试。 需要四个依赖项：

1. JUnit5
1. Mockito测试框架
1. Apache Sling Mocks
1. AEM Mocks Test Framework（由io.wcm提供）

在使用[AEM Maven原型](project-setup.md)进行设置时，将&#x200B;**JUnit5**、**Mockito**&#x200B;和&#x200B;**AEM Mocks**&#x200B;测试依赖项自动添加到项目中。

1. 要视图这些依赖项，请在&#x200B;**aem-guides-wknd/pom.xml**&#x200B;打开父反应器POM，导航到`<dependencies>..</dependencies>`并确保定义了以下依赖项：

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

1. 打开&#x200B;**aem-guides-wknd/core/pom.xml**&#x200B;并视图相应的测试依赖关系可用：

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

   **core**&#x200B;项目中的并行源文件夹将包含单元测试和任何支持测试文件。 此&#x200B;**test**&#x200B;文件夹提供测试类与源代码的分离，但允许测试的作用就像它们与源代码位于相同的包中一样。

## 创建JUnit测试{#creating-the-junit-test}

单元测试通常使用Java类映射1到1。 在本章中，我们将为&#x200B;**BylineImpl.java**&#x200B;编写一个JUnit测试，Byline组件是支持Byline组件的Sling模型。

![设备测试src文件夹](assets/unit-testing/core-src-test-folder.png)

*Unit测试的存储位置。*

1. 通过在Java包文件夹结构中在`src/test/java`下创建新的Java类，为`BylineImpl.java`创建单元测试，该文件夹结构镜像要测试的Java类的位置。

   ![新建一个BylineImplTest.java文件](assets/unit-testing/new-bylineimpltest.png)

   因为我们在测试

   * `src/main/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImpl.java`

   在

   * `src/test/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.java`

   单元测试文件`Test`后缀`BylineImplTest.java`是一种约定，它允许我们

   1. 轻松将其标识为&#x200B;_`BylineImpl.java`的测试文件_
   1. 但是，将测试文件&#x200B;_与要测试的类_&#x200B;区分开， `BylineImpl.java`



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

2. 后续方法是测试方法，其名称按约定以`test`前缀，并用`@Test`注释标记。 请注意，默认情况下，我们的所有测试都设置为失败，因为我们尚未实施它们。

   首先，我们对测试的类中的每个公共方法使用单一测试方法进行开始，因此：

   | BylineImpl.java |  | BylineImplTest.java |
   | ------------------|--------------|---------------------|
   | getName() | 由 | testGetName() |
   | getSchropions() | 由 | testGetSchorpies() |
   | isEmpty() | 由 | testIsEmpty() |

   这些方法可以根据需要进行扩展，我们将在本章的稍后部分看到。

   当运行此JUnit测试类（也称为JUnit测试用例）时，标有`@Test`的每个方法都将作为可通过或失败的测试执行。

![生成的BylineImplTest](assets/unit-testing/bylineimpltest-stub-methods.png)

*`core/src/test/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.java`*

1. 右键单击`BylineImplTest.java`文件，然后点按&#x200B;**运行**，即可运行JUnit Test Case。
如预期，所有测试都会失败，因为尚未实施。

   ![作为junit测试运行](assets/unit-testing/run-junit-tests.png)

   *右键单击BylineImplTests.java >运行*

## 查看BylineImpl.java {#reviewing-bylineimpl-java}

编写单元测试时，主要有两种方法：

* [TDD或测试驱动开发](https://en.wikipedia.org/wiki/Test-driven_development)，包括在开发实施之前立即以增量方式编写单元测试；编写测试，编写实现使测试通过。
* 实施优先开发，包括先开发工作代码，然后编写验证该代码的测试。

在本教程中，使用了后一种方法（因为我们在上一章中已经创建了一个有效的&#x200B;**BylineImpl.java**）。 因此，我们必须审查和理解其公共方法的行为，并了解其实施细节。 这听起来可能相反，因为良好的测试只应关心输入和输出，然而，在AEM工作时，需要了解各种执行考虑因素，以便构建工作测试。

TDD在AEM环境中需要一定的专业知识，并且最能被精通AEM代码开发和单元测试的AEM开发人员所采用。

## 设置AEM test context {#setting-up-aem-test-context}

为AEM编写的大多数代码都依赖JCR、Sling或AEM API，而JCR、Sling或API又需要运行的AEM的上下文才能正确执行。

由于单元测试是在生成时执行的，因此在运行的AEM实例的上下文之外，不存在此类上下文。 为了促进这一点，[wcm.io的AEM Mocks](https://wcm.io/testing/aem-mock/usage.html)创建了模拟上下文，允许这些API对&#x200B;_的作用与在AEM中运行一样。_

1. 通过将AEM上下文添加为以&#x200B;**BylineImplTest.java**&#x200B;文件装饰的JUnit扩展，使用&#x200B;**wcm.io**`AemContext`在&#x200B;**中的**`@ExtendWith`创建上下文。 该扩展负责所有所需的初始化和清理任务。 为`AemContext`创建一个可用于所有测试方法的类变量。

   ```java
   import org.junit.jupiter.api.extension.ExtendWith;
   import io.wcm.testing.mock.aem.junit5.AemContext;
   import io.wcm.testing.mock.aem.junit5.AemContextExtension;
   ...
   
   @ExtendWith(AemContextExtension.class)
   class BylineImplTest {
   
       private final AemContext ctx = new AemContext();
   ```

   此变量`ctx`公开了一个提供许多AEM和Sling抽象的模拟AEM上下文：

   * BylineImpl Sling模型将注册到此上下文中
   * 在此上下文中创建了模拟JCR内容结构
   * 可在此上下文中注册自定义OSGi服务
   * 提供多种常见的所需模型对象和帮助，如SlingHttpServletRequest对象、各种模拟Sling和AEM OSGi服务，如ModelFactory、PageManager、Page、Template、ComponentManager、Component、TagManager、Tag等。
      * *请注意，并非这些对象的所有方法都已实现！*
   * [更多](https://wcm.io/testing/aem-mock/usage.html)!

   **`ctx`**&#x200B;对象将作为我们大多数模型上下文的入口点。

1. 在每个`@Test`方法之前执行的`setUp(..)`方法中，定义一个通用的模拟测试状态：

   ```java
   @BeforeEach
   public void setUp() throws Exception {
       ctx.addModelsForClasses(BylineImpl.class);
       ctx.load().json("/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.json", "/content");
   }
   ```

   * **`addModelsForClasses`** 将要测试的Sling模型注册到mok AEM Context中，以便在方法中实例 `@Test` 化。
   * **`load().json`** 将资源结构加载到模型上下文中，使代码能够与这些资源进行交互，就像它们是由真实存储库提供的一样。文件&#x200B;**`BylineImplTest.json`**&#x200B;中的资源定义将加载到&#x200B;**/content**&#x200B;下的模型JCR上下文中。
   * **`BylineImplTest.json`** 尚未存在，因此，我们创建它并定义测试所需的JCR资源结构。

1. 表示模拟资源结构的JSON文件存储在&#x200B;**core/src/test/resources**&#x200B;下，其路径与JUnit Java测试文件相同。

   在&#x200B;**core/test/resources/com/adobe/aem/guides/wknd/core/models/impl**&#x200B;创建名为&#x200B;**BylineImplTest.json**&#x200B;的新JSON文件，内容如下：

   ```json
   {
       "byline": {
       "jcr:primaryType": "nt:unstructured",
       "sling:resourceType": "wknd/components/content/byline"
       }
   }
   ```

   ![BylineImplTest.json](assets/unit-testing/bylineimpltest-json.png)

   此JSON为Byline组件单元测试定义一个模型资源（JCR节点）。 此时，JSON具有表示Byline组件内容资源、`jcr:primaryType`和`sling:resourceType`所需的最少属性集。

   处理单元测试时的一般规则是，创建满足每个测试所需的最少一组模拟内容、上下文和代码。 避免在编写测试之前构建完整的模拟上下文的诱惑，因为这往往会产生不需要的伪像。

   现在，由于存在&#x200B;**BylineImplTest.json**，当执行`ctx.json("/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.json", "/content")`时，模型资源定义将加载到路径&#x200B;**/content.**&#x200B;的上下文中。

## 正在测试getName(){#testing-get-name}

现在，我们有一个基本的模型上下文设置，让我们为&#x200B;**BylineImpl的getName()**&#x200B;编写第一个测试。 此测试必须确保方法&#x200B;**getName()**&#x200B;返回存储在资源的&quot;**name&quot;**&#x200B;属性中的正确创作名称。

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

   * **`String expected`** 设置预期值。我们将其设置为“**简·完成**”。
   * **`ctx.currentResource`** 设置要评估代码的模型资源的上下文，因此将其设置为/ **content/** bylineas，即加载模型byline内容资源的位置。
   * **`Byline byline`** 通过从mock Request对象中调整Byline Sling模型来实例化它。
   * **`String actual`** 调用我们正在测试的方 `getName()`法，在Byline Sling Model对象上。
   * **`assertEquals`** 断言预期值与署名Sling Model对象返回的值匹配。如果这些值不相等，则测试将失败。

1. 运行测试……，但`NullPointerException`失败。

   请注意，此测试不会失败，因为我们从未在模拟JSON中定义`name`属性，这会导致测试失败，但测试执行尚未达到该点！ 由于byline对象本身上的`NullPointerException`，此测试失败。

1. 在`BylineImpl.java`中，如果`@PostConstruct init()`引发异常，则会阻止Sling模型实例化，并导致该Sling Model对象为null。

   ```java
   @PostConstruct
   private void init() {
       image = modelFactory.getModelFromWrappedRequest(request, request.getResource(), Image.class);
   }
   ```

   事实证明，虽然ModelFactory OSGi服务是通过`AemContext`（通过Apache Sling Context）提供的，但并非所有方法都得到实现，包括在BylineImpl的`init()`方法中调用的`getModelFromWrappedRequest(...)`。 这会导致[AbstractMethodError](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/AbstractMethodError.html)，该函数在术语上导致`init()`失败，而由此产生的`ctx.request().adaptTo(Byline.class)`的适配是空对象。

   由于提供的嘲笑无法容纳我们的代码，我们必须自己实现模型上下文。为此，我们可以使用Mockito创建一个模拟ModelFactory对象，当调用`getModelFromWrappedRequest(...)`时，该对象将返回一个模拟Image对象。

   因为，为了甚至实例化Byline Sling模型，此模型上下文必须就位，我们可以将其添加到`@Before setUp()`方法中。 我们还需要将`MockitoExtension.class`添加到&#x200B;**BylineImplTest**&#x200B;类上方的`@ExtendWith`注释中。

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

   * **`@ExtendWith({AemContextExtension.class, MockitoExtension.class})`** 标记要与Mockito JUnit Jupiter扩展一起运 [行的Test ](https://www.javadoc.io/page/org.mockito/mockito-junit-jupiter/latest/org/mockito/junit/jupiter/MockitoExtension.html) Case类，该类允许使用@Mock批注在类级别定义模拟对象。
   * **`@Mock private Image`** 创建类型的模型对 `com.adobe.cq.wcm.core.components.models.Image`象请注意，这是在类级别定义的，这样，`@Test`方法可以根据需要更改其行为。
   * **`@Mock private ModelFactory`** 创建ModelFactory类型的模型对象。请注意，这是纯Mockito模型，没有在其上实现任何方法。 请注意，这是在类级别定义的，这样，`@Test`方法可以根据需要更改其行为。
   * **`when(modelFactory.getModelFromWrappedRequest(..)`** 在mockModelFactory对象上 `getModelFromWrappedRequest(..)` 调用时注册模型行为。在`thenReturn (..)`中定义的结果是返回模型Image对象。 请注意，仅在以下情况下才调用此行为：第1个参数等于`ctx`的request对象，第2个参数是任何Resource对象，第3个参数必须是Core Components Image类。 我们接受任何资源，因为在整个测试过程中，我们将将`ctx.currentResource(...)`设置为&#x200B;**BylineImplTest.json**&#x200B;中定义的各种模拟资源。 请注意，我们添加了&#x200B;**enlighte()**&#x200B;严格性，因为我们稍后将要覆盖ModelFactory的此行为。
   * **`ctx.registerService(..)`。** 将模型ModelFactory对象注册到AemContext中，其服务级别最高。这是必需的，因为BylineImpl的`init()`中使用的ModelFactory是通过`@OSGiService ModelFactory model`字段注入的。 为了使AemContext插入&#x200B;**我们的**&#x200B;模拟对象（处理对`getModelFromWrappedRequest(..)`的调用），我们必须将其注册为该类型(ModelFactory)的最高级别服务。

1. 重新运行测试，然后再次失败，但这一次，消息清晰地说明了测试失败的原因。

   ![测试名称失败声明](assets/unit-testing/testgetname-failure-assertion.png)

   *testGetName()失败，原因是断言*

   我们收到一个&#x200B;**AssertionError**，表示测试中的断言条件失败，它告诉我们&#x200B;**预期值为&quot;Jane Doe&quot;**，但&#x200B;**实际值为null**。 这很有意义，因为尚未将&quot;**name&quot;**&#x200B;属性添加到&#x200B;**BylineImplTest.json**&#x200B;中的mock **/content/byline**&#x200B;资源定义中，因此，我们添加它：

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

   ![测试名称](assets/unit-testing/testgetname-pass.png)


## 正在测试getSchorpies(){#testing-get-occupations}

很好！ 我们的第一次考试通过了！ 让我们继续测试`getOccupations()`。 由于模型上下文的初始化是在`@Before setUp()`方法中进行的，因此此测试案例中的所有`@Test`方法（包括`getOccupations()`）均可使用此模型上下文。

请记住，此方法必须返回存储在职业属性中的职业（降序）的按字母顺序排序的列表。

1. 更新&#x200B;**`testGetOccupations()`**，如下所示：

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
   * **`ctx.currentResource`** 将当前资源设置为根据/content/byline中的模型资源定义评估上下文。这可确保在模型资源的上下文中执行&#x200B;**BylineImpl.java**。
   * **`ctx.request().adaptTo(Byline.class)`** 通过从mock Request对象中调整Byline Sling模型来实例化它。
   * **`byline.getOccupations()`** 调用我们正在测试的方 `getOccupations()`法，在Byline Sling Model对象上。
   * **`assertEquals(expected, actual)`** 断言预期列表与实际列表相同。

1. 请记住，与上面的&#x200B;**`getName()`**&#x200B;一样，**BylineImplTest.json**&#x200B;不定义职业，因此如果运行它，此测试将失败，因为`byline.getOccupations()`将返回空列表。

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

1. 试一试，我们又通过了！ 看来分类的职业很管用！

   ![获得职业通过](assets/unit-testing/testgetoccupations-pass.png)

   *testGetSchorpils()通过*

## 测试isEmpty(){#testing-is-empty}

测试&#x200B;**`isEmpty()`**&#x200B;的最后一个方法。

测试`isEmpty()`很有趣，因为它需要测试各种情况。 查看&#x200B;**BylineImpl.java**&#x200B;的`isEmpty()`方法，必须测试以下条件：

* 名称为空时返回true
* 当职业为null或空时返回true
* 当图像为null或没有src URL时，返回true
* 如果存在名称、职业和图像（带有src URL），则返回false

为此，我们需要创建新的测试方法，每个方法都测试特定条件，并在`BylineImplTest.json`中创建新的模拟资源结构来驱动这些测试。

请注意，此检查允许我们在`getName()`、`getOccupations()`和`getImage()`为空时跳过测试，因为通过`isEmpty()`测试该状态的预期行为。

1. 第一个测试将测试没有设置任何属性的全新组件的状态。

   向`BylineImplTest.json`添加新的资源定义，为其提供语义名称&quot;**empty**&quot;

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

   **`"empty": {...}`** 定义名为“empty”的新资源定义，该定义只包含 `jcr:primaryType` 和 `sling:resourceType`。

   请记住，在`@setUp`中执行每个测试方法之前，我们会先将`BylineImplTest.json`加载到`ctx`中，因此，在&#x200B;**/content/empty的测试中，我们可以立即使用此新资源定义。**

1. 按如下方式更新`testIsEmpty()`，将当前资源设置为新的“**empty**”模拟资源定义。

   ```java
   @Test
   public void testIsEmpty() {
       ctx.currentResource("/content/empty");
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertTrue(byline.isEmpty());
   }
   ```

   运行测试并确保通过。

1. 接下来，创建一组方法，以确保任何所需的数据点（名称、职业或图像）为空，`isEmpty()`将返回true。

   对于每个测试，使用离散的模拟资源定义，使用&#x200B;**without-name**&#x200B;和&#x200B;**without-schorpies**&#x200B;的附加资源定义更新&#x200B;**BylineImplTest.json**。

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

   创建以下测试方法以测试这些状态中的每个状态。

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

   **`testIsEmpty()`** 针对空模型资源定义进行测试，并声 `isEmpty()` 明为true。

   **`testIsEmpty_WithoutName()`** 测试的是一个没有名字但有职业的模拟资源定义。

   **`testIsEmpty_WithoutOccupations()`** 测试所依据的是一个名字，但没有职业的模拟资源定义。

   **`testIsEmpty_WithoutImage()`** 使用名称和职位对模拟资源定义进行测试，但将模拟图像设置为返回null。请注意，我们希望覆盖在`setUp()`中定义的`modelFactory.getModelFromWrappedRequest(..)`行为，以确保此调用返回的Image对象为null。 莫基托小作品的特征很严格，不需要重复的代码。 因此，我们使用&#x200B;**`lenient`**&#x200B;设置设置模型以显式注意我们正在覆盖`setUp()`方法中的行为。

   **`testIsEmpty_WithoutImageSrc()`** 使用名称和职位对模拟资源定义进行测试，但将模拟图像设置为在调用时返回空 `getSrc()` 白字符串。

1. 最后，编写测试以确保在正确配置组件时&#x200B;**isEmpty()**&#x200B;返回false。 对于这种情况，我们可以重新使用&#x200B;**/content/byline**，它表示完全配置的Byline组件。

   ```java
   @Test
   public void testIsNotEmpty() {
       ctx.currentResource("/content/byline");
       when(image.getSrc()).thenReturn("/content/bio.png");
   
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertFalse(byline.isEmpty());
   }
   ```

1. 现在，在BylineImplTest.java文件中运行所有单元测试，并查看Java测试报告输出。

![所有测试均通过](./assets/unit-testing/all-tests-pass.png)

## 作为内部版本{#running-unit-tests-as-part-of-the-build}的一部分运行单元测试

执行单元测试是作为主生成的一部分进行传递的。 这可确保在部署应用程序之前成功通过所有测试。 执行包或安装等主要目标会自动调用并要求通过项目中的所有单元测试。

```shell
$ mvn package
```

![mvn包成功](assets/unit-testing/mvn-package-success.png)

```shell
$ mvn package
```

同样，如果我们将测试方法更改为失败，则生成会失败并报告测试失败的原因和原因。

![mvn包失败](assets/unit-testing/mvn-package-fail.png)

## 查看代码{#review-the-code}

视图[GitHub](https://github.com/adobe/aem-guides-wknd)上完成的代码，或在Git brach `tutorial/unit-testing-solution`上查看并本地部署代码。
