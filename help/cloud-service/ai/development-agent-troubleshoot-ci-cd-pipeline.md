---
title: 使用AEM Development Agent对CI/CD管道进行故障诊断
description: 了解如何使用AEM开发代理对失败的CI/CD管道进行故障排除和修复。
version: Experience Manager as a Cloud Service
role: Developer, Admin
level: Beginner
doc-type: tutorial
duration: null
jira: KT-20279
thumbnail: KT-20279.png
last-substantial-update: null
source-git-commit: 6fa0f88c231f7b68392a77a60491d4f741140a5a
workflow-type: tm+mt
source-wordcount: '1227'
ht-degree: 1%

---


# 使用AEM Development Agent对CI/CD管道进行故障诊断

了解如何使用AEM开发代理对失败的CI/CD管道进行故障排除和修复。

AEM开发代理通过提供&#x200B;**AI支持的指导和操作**，帮助技术团队（包括开发人员、DevOps工程师和管理员）加快其工作流&#x200B;_。_

>[!TIP]
>
> 另请参阅[AEM中的代理概述](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/ai-in-aem/agents/overview)，获取AEM as a Cloud Service中可用代理的完整列表、它们的功能以及如何访问它们。


## 概述

AEM Development Agent提供了多项功能，包括列出、排除和修复失败的CI/CD管道的功能。 您可以通过AI Assistant调用AEM开发代理，以解决您的特定用例。

本教程使用[WKND Sites项目](https://github.com/adobe/aem-guides-wknd)来演示如何使用AEM开发代理对失败的CI/CD管道进行故障排除和修复。 同样的原则适用于任何AEM项目。

为简单起见，本教程在`BylineImpl.java`文件中引入了一个单元测试失败，以展示AEM开发代理的管道故障排除功能。

## 先决条件

要学习本教程，您需要：

- AEM中的AI助手和代理已启用。 有关详细信息，请参阅[在AEM中设置AI](./setup.md)，并请注意，该文章中提到的游乐场将不具有AEM开发代理功能。
- 使用开发人员或项目经理角色访问Adobe [Cloud Manager](https://my.cloudmanager.adobe.com/)。 有关详细信息，请参阅[角色定义](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-manager/content/requirements/users-and-roles#role-definitions)。
- AEM as a Cloud Service环境
- 通过[Beta程序](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/release-notes/release-notes/release-notes-current#aem-beta-programs)访问AEM中的代理
- [WKND站点项目](https://github.com/adobe/aem-guides-wknd)已克隆到您的本地计算机

### AEM Development Agent的当前功能

在深入研究本教程之前，让我们先查看AEM Development Agent的当前功能：

- 列出CI/CD管道及其状态
- 排除和修复失败的&#x200B;**全栈**&#x200B;管道，包括&#x200B;_代码质量_&#x200B;和&#x200B;_部署_&#x200B;类型。
- 支持&#x200B;_全栈_&#x200B;管道的&#x200B;_生成_（生成可部署项目的代码复杂化）和&#x200B;**代码质量**（通过SonarQube规则进行静态代码分析）步骤。

AEM Development Agent的功能会不断扩展和定期更新。 若要获取反馈和建议，请发送电子邮件至[aem-devagent@adobe.com](mailto:aem-devagent@adobe.com)。

## 设置

请按照以下高级步骤完成本教程：

1. 克隆[WKND站点项目](https://github.com/adobe/aem-guides-wknd)并将其推送到Cloud Manager Git存储库
2. 创建和配置代码质量管道
3. 运行管道并观察失败的执行
4. 使用AEM开发代理对失败的管道进行故障排除和修复

下面我们将详细介绍每个步骤。

### 使用WKND Sites项目作为演示项目

本教程使用WKND Sites项目的`tutorial/dev-agent/unit-test-failure`分支来演示如何使用AEM开发代理。 同样的原则可以应用于任何AEM项目。

- `BylineImpl.java`文件中引入了单元测试失败，如下所示。 如果您使用自己的AEM项目，则可以引入类似的单元测试失败。

  ```java
  ...
  @Override
  public String getName() {
      if (name != null) {
          return "Author: " + name; // This line is intentionally incorrect to introduce a unit test failure.
      }
      return name;
  }
  ...
  ```

- 将[WKND站点项目](https://github.com/adobe/aem-guides-wknd)克隆到本地计算机，导航到项目目录，然后切换到`tutorial/dev-agent/unit-test-failure`分支。

  ```shell
  git clone https://github.com/adobe/aem-guides-wknd.git
  cd aem-guides-wknd
  git checkout tutorial/dev-agent/unit-test-failure
  ```

- 为WKND Sites项目创建新的Cloud Manager Git存储库，并将其作为远程存储库添加到本地Git存储库：

   - 导航到Adobe [Cloud Manager](https://my.cloudmanager.adobe.com/)并选择您的项目。
   - 单击左侧边栏中的&#x200B;**存储库**。
   - 单击右上角的&#x200B;**添加存储库**。
   - 输入&#x200B;**存储库名称**（例如，“wknd-site-tutorial”），然后单击&#x200B;**保存**。 等待创建存储库。

     ![添加存储库](./assets/dev-agent/add-repository.png)

   - 单击右上角的&#x200B;**访问存储库信息**&#x200B;并复制存储库URL。

     ![访问存储库信息](./assets/dev-agent/access-repo-info.png)

   - 将新创建的Cloud Manager Git存储库作为远程存储库添加到本地Git存储库：

     ```shell
     git remote add adobe https://git.cloudmanager.adobe.com/<your-adobe-organization>/wknd-site-tutorial/
     ```

- 将本地Git存储库推送到Cloud Manager Git存储库：

  ```shell
  git push adobe
  ```

  在系统提示输入凭据时，从Cloud Manager的&#x200B;**存储库信息**&#x200B;模式中提供&#x200B;**用户名**&#x200B;和&#x200B;**密码**。

### 创建和配置代码质量管道

本教程使用代码质量管道（非生产）触发管道故障以进行故障排除。 有关代码质量管道的详细信息，请参阅[&#x200B; CI/CD管道简介](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines#introduction)。

- 在Cloud Manager中，导航到&#x200B;**管道**&#x200B;部分，然后选择&#x200B;**添加** > **添加非生产管道**。
- 在&#x200B;**添加非生产管道**&#x200B;对话框中，配置以下内容：

   - **配置**&#x200B;步骤：
      - 将诸如&#x200B;**管道类型**&#x200B;之类的默认值保留为`Code Quality Pipeline`，**部署触发器**&#x200B;保留为`Manual`。
      - 对于&#x200B;**非生产管道名称**，输入`Code Quality::Fullstack`

     ![添加非生产管道配置](./assets/dev-agent/add-non-production-pipeline-configuration.png)

   - **Source代码**&#x200B;步骤：
      - 选择&#x200B;**全栈代码**
      - 对于&#x200B;**存储库**，选择新创建的Cloud Manager Git存储库
      - 对于&#x200B;**Git分支**，请选择`tutorial/dev-agent/unit-test-failure`
      - 单击&#x200B;**保存**

     ![添加非生产管道Source代码](./assets/dev-agent/add-non-production-pipeline-source-code.png)

- 在管道条目的三点式菜单中单击&#x200B;**运行**，运行新创建的代码质量管道。

  ![运行代码质量管道](./assets/dev-agent/run-code-quality-pipeline.png)


>[!IMPORTANT]
>
> 本教程中未介绍部署管道。 但是，您可以按照相同的原则对失败的部署管道进行故障排除和修复。


### 观察失败的管道执行

代码质量管道在&#x200B;**工件准备**&#x200B;步骤中失败，并返回错误：

![失败的管道执行](./assets/dev-agent/failed-pipeline-execution.png)

如果没有AEM开发代理，此管道故障需要手动疑难解答。 开发人员需要检查日志并查看代码，这是一个繁琐且耗时的过程。

接下来，您将了解代理人工智能如何对失败的管道执行进行故障排除和修复。

## 使用AEM开发代理对失败的管道进行故障排除和修复

您可以通过以自然语言描述管道故障，从而使用AEM中的AI Assistant调用AEM开发代理。

- 单击右上角的&#x200B;**AI助手**&#x200B;图标。

- 以自然语言（即&#x200B;**提示**）输入管道失败详细信息。 例如：

  ```text
  I have a failed pipeline execution on %PROGRAM-NAME% program, help me to troubleshoot and fix it.
  ```

  ![调用AEM开发代理](./assets/dev-agent/invoke-aem-development-agent.png)

  调用&#x200B;**AEM开发代理**&#x200B;对失败的管道执行进行故障排除和修复。

  >[!NOTE]
  >
  > 如果输入的提示不清晰，AI助手会要求您进行说明，并提供信息以帮助您优化提示。

- 推理完成后，单击&#x200B;**全屏打开**&#x200B;图标以查看详细的故障排除过程。

  ![全屏打开](./assets/dev-agent/open-in-full-screen.png)

  结果包含有用的见解，包括错误详细信息、源文件、行号以及&#x200B;**如何修复**&#x200B;部分，其中包含解决问题的明确步骤。

- 在这种情况下，代理正确地建议更改实现（`getName()`方法）或更新单元测试（`getNameTest()`方法）以解决此问题。 它避免了幻觉，并采用了human-in-the-loop方法，同时为开发人员提供了可操作的代码更改。

  ![复制代码更改](./assets/dev-agent/copy-code-changes.png)

- 使用建议的代码更改更新`BylineImpl.java`文件，然后提交更改并将其推送到Cloud Manager Git存储库。

  ```java
  ...
  @Override
  public String getName() {
      return name;
  }
  ...
  ```

- 再次运行管道并观察是否成功执行。

## 其他示例

WKND Sites项目包括损坏的代码和配置问题的其他示例，例如缺少依赖项和不正确的配置。 您可以通过查看以[`tutorial/dev-agent/`开头的](https://github.com/adobe/aem-guides-wknd/branches/all?query=tutorial%2Fdev-agent&lastTab=overview)分支来探索这些示例。 若要查看重大更改，您可以通过单击“`tutorial/dev-agent/unit-test-failure`比较`main`”按钮将&#x200B;**分支与**&#x200B;分支进行比较。 然后查找&#x200B;_更改的文件_&#x200B;部分。

![比较分支](./assets/dev-agent/compare-branches.png)

另请参阅[示例提示](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/ai-in-aem/agents/development/overview#sample-prompts)，获取有关如何使用AEM开发代理的更多想法。

## 摘要

在本教程中，您已了解如何使用AEM开发代理通过AI助手对失败的CI/CD管道进行故障排除和修复。 您还了解了代理人工智能如何通过提供可操作见解和代码更改来加速技术工作流。

开始使用AEM中的AEM开发代理和其他代理来加速您的工作流，有关详细信息，请参阅[AEM中的代理概述](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/ai-in-aem/agents/overview)。

## 其他资源

- [Experience Manager的人工智能](./overview.md)
- [AEM代理概述](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/ai-in-aem/agents/overview)
- [开发代理概述](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/ai-in-aem/agents/development/overview)
- [AEM代理概述](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/ai-in-aem/agents/overview)
