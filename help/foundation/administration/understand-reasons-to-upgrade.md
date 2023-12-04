---
title: 了解升级的原因
description: 面向考虑升级到最新版本Adobe Experience Manager 6的客户的重要功能高级细目。
version: 6.5
topic: Upgrade
feature: Release Information
role: Leader, Architect, Developer, Admin, User
level: Beginner
doc-type: Article
exl-id: bf4030b0-67c4-4b00-af95-f63e6f79e995
duration: 820
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '2586'
ht-degree: 1%

---

# 了解升级原因

面向考虑升级到最新版本Adobe Experience Manager 6的客户的重要功能高级细目。

## 升级到AEM 6.5的主要功能

+ [Adobe Experience Manager 6.5发行说明](https://helpx.adobe.com/cn/experience-manager/6-5/release-notes.html)

### 基础改进

Adobe Experience Manager 6.5继续通过以下方式增强系统的稳定性、性能和可支持性：

+ **Java 11** 支持（同时保持Java 8支持）。

### 网站创建和管理

AEM Sites引入了一系列旨在加速网站创建和构建的功能：

+ **SPA编辑器** 这种支持使得可在AEM中完全创作SPA（单页应用程序），从而支持丰富、对营销人员友好的创作体验。
+_ **JavaScript SDK的**&#x200B;是一个SPA项目启动套件和支持的构建工具，它允许前端开发人员独立于AEM开发与SPA编辑器兼容的单页应用程序。
+ **核心组件** 添加大量新组件， **组件库** 以及现有核心组件的各种增强功能。
+ 其他 **翻译** 增强了AEM Sites的简化翻译。

### 流畅的体验

AEM通过新的和改进的工具继续拥抱流畅的体验，这些工具便于在AEM之外使用内容。

+ **内容片段** 支持版本比较/差异和注释。
+ **AEM Assets HTTP API** 支持公开 **内容片段** 直接在DAM中作为 **JSON**.
  **体验片段** 支持 **全文搜索** 和 **AEM Dispatcher缓存失效** 以供参考 **页面**.

### 资产管理

AEM Assets继续以其丰富的资产管理功能集为基础，改进对DAM的使用、管理和了解。 AEM 6.5继续改进Adobe Creative Cloud与创意工作流程之间的集成。

+ **Adobe资源链接** 将创意人员从Adobe Creative Cloud工具直接连接到AEM Assets。
+ **Adobe Stock** 集成允许直接从AEM Assets Experience访问Adobe Stock图像，从而创造无缝的内容发现体验。
+ **AEM桌面应用程序** 版本2.0，并在改进性能和稳定性的同时对其自身进行了重新设想。
+ **连接的资产** 支持独立的AEM Sites实例，以便无缝访问和使用其他AEM Assets实例中的资源。
+ 更新了中的视频支持 **Dynamic Media**，包括 **360视频** 和 **自定义视频缩略图**.

### 内容智能

AEM继续构建与智能技术的集成，利用机器学习和人工智能改善所有体验。

+ **Adobe资源链接** 添加 **视觉相似性搜索**，从而可以轻松地在中发现和使用类似的图像 **Adobe Creative Cloud工具**.

### 集成

AEM提高了与其他Adobe服务集成的能力：

+ **体验片段** 深化与的集成 **Adobe Target** 通过支持 **导出为JSON** 到Adobe Target并能够 **删除基于体验片段的选件** 从 **Adobe Target**.

### AMS Cloud Manager

[Cloud Manager](https://adobe.ly/2HODmsv)是Managed Services (AMS)客户专有Adobe的一项功能，可提供以下功能：

+ Cloud Manager支持将AEM部署支持从AEM Sites扩展到 **AEM Assets**，包括 **资产处理的自动性能测试**.
+ **自动缩放** 达到预定义阈值的AEM发布层，确保获得最佳最终用户体验。
+ **非生产管道** 允许开发团队利用Cloud Manager不断检查代码质量并部署到较低环境（开发和QA）。
+ **CI/CD管道API** 允许客户以编程方式与Cloud Manager交互，深化与内部部署开发基础架构的集成可能性。

## 基础功能

以下是AEM提供的关键基础功能列表。 其中一些功能已在早期版本中引入，并在每个版本中添加了增量增强功能。

+ [AEM Foundation发行说明](https://helpx.adobe.com/experience-manager/6-5/release-notes/wcm-platform.html)

***✔<sup>+</sup> 此版本中的功能得到了显着增强。***

***✔<sup>SP</sup> 表示该功能可通过Service Pack或Feature Pack使用。***

<table>
    <thead>
        <tr>
            <td>基础功能</td>
            <td>5.6.x</td>
            <td>6.0</td>
            <td>6.1</td>
            <td>6.2</td>
            <td>6.3</td>
            <td>6.4</td>
            <td>6.5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>
                <strong>Java 11支持：</strong> AEM支持Java 11（以及Java 8）。
            </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
        </tr>
        <tr>
            <td>
                <strong><a href="https://jackrabbit.apache.org/oak/docs/index.html" target="_blank">Oak内容存储库</a>：</strong> 比前身Jackrabbit 2提供更出色的性能和可扩展性。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/indexing-via-the-oak-run-jar.html">oak-run.jar索引支持</a>：</strong> 改进了Oak索引的重新索引、统计信息收集和一致性检查。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/queries-and-indexing.html" target="_blank">自定义搜索索引</a>： </strong>
                能够添加自定义索引定义以优化查询性能和搜索相关性。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/revision-cleanup.html" target="_blank">在线修订版清理</a>：</strong>
                执行存储库维护，无需服务器停机。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/storage-elements-in-aem-6.html" target="_blank">TarMK或MongoMK存储库存储</a>：</strong>
                <br> 使用TarMK（新一代版本的TarPM）的简单、高性能文件存储选项
                <br> 或者使用MongoDB支持的带有MongoMK的存储库水平扩展。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/aem-with-mongodb.html" target="_blank">MongoMK性能和稳定性</a>：</strong>
            自MongoMK引入AEM 6.0以来，它一直得到增强。</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/data-store-config.html#AmazonS3DataStore">Amazon S3数据存储</a>：</strong>
            利用可扩展的云存储解决方案来存储二进制资产。</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>触屏UI对等功能：</strong>
                持续增强创作UI以提高工作效率，并增强与经典UI的功能对等性。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Omnisearch：</strong>
                快速搜索和导航AEM。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/operations-dashboard.html" target="_blank">操作功能板</a>：</strong>
 从AEM内执行维护、监控服务器运行状况并分析性能。</td>
            <td></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/upgrade.html" target="_blank">升级改进</a>：</strong>
            升级改进可让您更轻松、更快速地就地升级AEM。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/cn/experience-manager/htl/using/overview.html" target="_blank">HTL模板语言</a>：</strong>
            一种将表示与逻辑分离的现代模板化引擎。 显着缩短组件开发时间。 每个版本中添加了增量功能。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://sling.apache.org/documentation/bundles/models.html" target="_blank">Sling模型</a>：</strong>
            将JCR资源建模为业务对象和逻辑的灵活框架。 每个版本中添加了增量功能。
            </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://adobe.ly/2HODmsv" target="_blank">Cloud Manager</a>： </strong>
                除了AdobeManaged Services (AMS)客户之外，Cloud Manager还通过最先进的CI/CD管道加快了开发和部署。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
    </tbody>
</table>

## 安全功能

以下是AEM提供的主要安全功能列表。 其中一些功能已在早期版本中引入，并在每个版本中添加了增量增强功能。

+ [安全性发行说明](https://helpx.adobe.com/experience-manager/6-5/release-notes/wcm-platform.html#Security)

***✔表示此版本中的功能得到了显着增强。***

***✔<sup>+</sup> 表示该功能可通过Service Pack或Feature Pack使用。***

<table>
    <thead>
        <tr>
            <td>安全功能</td>
            <td>5.6.x</td>
            <td>6.0</td>
            <td>6.1</td>
            <td>6.2</td>
            <td>6.3</td>
            <td>6.4</td>
            <td>6.5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-service-users.html" target="_blank">服务用户</a></strong>
            <br> 将权限划分为多个区段，以避免不必要地使用管理员权限。</td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/saml-2-0-authenticationhandler.html" target="_blank">密钥存储管理</a></strong>
            <br> 全局信任存储区、证书和密钥都在存储库中管理。</td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><a href="https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/csrf-protection.html" target="_blank"><strong>CSRF</strong> <strong>保护</strong></a>
            <br> 开箱即用的跨站点请求伪造保护。</td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/saml-2-0-authenticationhandler.html" target="_blank"><strong>CORS</strong> <strong>支持</strong></a>
            <br> 跨源资源共享支持提高应用程序灵活性。</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://experienceleague.adobe.com/docs/" target="_blank">改进了SAML身份验证支持</a><br>
 </strong>改进了SAML重定向，优化了组信息和解决了密钥加密问题。
            <br>
        </td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ldap-config.html" target="_blank">LDAP作为OSGi配置</a><br>
 </strong>简化了LDAP身份验证的管理和更新。</td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong>对纯文本密码的OSGi加密支持<br>
 </strong>密码和其他敏感值可以加密形式保存并自动解密。</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/user-group-ac-admin.html" target="_blank">CUG增强功能</a><br>
 </strong>封闭用户组实施已重新编写，以解决性能和可扩展性问题。</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔<sup>+</sup></td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/platform-repository/using/ssl-wizard-technical-video-use.html" target="_blank">SSL向导</a></strong>
            <br> 用于简化SSL设置和管理的UI。</td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/encapsulated-token.html" target="_blank">封装令牌支持</a></strong>
            <br> 不再需要“粘性”会话来支持跨发布实例的水平身份验证。</td>
        <td> </td>
        <td> </td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ims-config-and-admin-console.html" target="_blank">Adobe IMS身份验证支持</a><br>
 </strong>专门AdobeManaged Services (AMS)，可通过Adobe IMS (Identity Management System)集中管理对AEM创作实例的访问。</td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td>✔</td>
        <td>✔</td>
    </tr>
</tbody>
</table>

## 站点功能

以下是AEM提供的关键Sites功能列表。 其中一些功能已在早期版本中引入，并在每个版本中添加了增量增强功能。

+ [AEM Sites发行说明](https://helpx.adobe.com/experience-manager/6-5/release-notes/sites.html)

***✔<sup>+</sup> 此版本中的功能得到了显着增强。***

***✔<sup>SP</sup> 表示该功能可通过Service Pack或Feature Pack使用。***

<table>
    <thead>
        <tr>
            <td><strong>站点功能</strong></td>
            <td>5.6.x</td>
            <td>6.0</td>
            <td>6.1</td>
            <td>6.2</td>
            <td>6.3</td>
            <td>6.4</td>
            <td>6.5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/page-editor-feature-video-use.html" target="_blank">触控优化页面创作</a>：</strong>
            允许编辑人员使用带有触摸屏的平板电脑和计算机。</td>
            <td></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/responsive-layout.html" target="_blank">响应式网站创作</a>：</strong>
                布局模式允许编辑器根据响应式站点的设备宽度调整组件大小。</td>
            <td></td>
            <td></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/template-editor-feature-video-use.html" target="_blank">可编辑的模板</a>：</strong>
            允许专业作者创建和编辑页面模板。</td>
            <td></td>
            <td></td>
            <td></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/core-components/user-guide.html" target="_blank">核心组件</a>：</strong>
            加快站点开发。 在GitHub上提供，可提供频繁的发布计划和灵活性。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/spa-overview.html" target="_blank">SPA编辑器</a>：</strong>
            使用基于React构建的单页应用程序(SPA)框架，创建可创作、可触发的Web体验。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong>样式系统：</strong>
            通过使用上下文内样式系统定义其可视化外观，提高AEM组件的重用率。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>SP</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/msm.html" target="_blank">多站点管理器(MSM)</a>：</strong>
            管理共享相同内容的多个网站（即多语言、多个品牌）。</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/translation.html" target="_blank">内容翻译</a>：</strong>
            即插即用框架与业界领先的第三方翻译服务集成。</td>
            <td></td>
            <td></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/contexthub.html" target="_blank">ContextHub</a>：</strong>
            用于内容个性化的下一代客户端上下文框架。</td>
            <td></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/launches.html" target="_blank">启动次数</a>：</strong>
            在不中断日常创作的情况下为未来版本开发内容。</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>内容片段：</strong>
            创建和管理与演示文稿脱钩的编辑内容，以方便重用。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/experience-fragments-feature-video-use.html" target="_blank">体验片段</a>：</strong>
            创建针对桌面、移动和社交渠道优化的可重用体验和变体。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong>内容服务：</strong>
            将内容从AEM导出为JSON，以便跨设备和应用程序使用。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>SP</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Adobe Analytics集成和内容分析：</strong>
                Adobe Analytics与DTM的轻松集成。 在创作环境中显示性能信息。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/content-targeting-touch.html" target="_blank">Adobe Target集成</a>：</strong>
            分步向导用于创建目标体验、创建可重复使用的选件库。</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/campaign.html" target="_blank">Adobe Campaign集成</a>：</strong>
            与新一代电子邮件促销活动解决方案轻松集成。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/cn/experience-manager/using/aem_launch_adobeio_integration.html" target="_blank">AdobeLaunch集成</a>：</strong>
            与Adobe的下一代标签管理云服务集成。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>屏幕：</strong>
            管理数字标牌和网亭的体验。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ecommerce.html" target="_blank">电子商务</a>：</strong>
            跨Web、移动和社交接触点交付品牌化、个性化的购物体验。
            </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/overview.html" target="_blank">Communities</a>：</strong>
            论坛、主题评论、活动日历和许多其他功能允许与网站访客进行深入互动。</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
    </tbody>
</table>

## Assets功能

以下是AEM提供的关键Assets功能列表。 其中一些功能已在早期版本中引入，并在每个版本中添加了增量增强功能。

+ [AEM Assets发行说明](https://helpx.adobe.com/experience-manager/6-5/release-notes/assets.html)

***✔表示此版本中的功能得到了显着增强。***

***✔<sup>+</sup> 表示该功能可通过Service Pack或Feature Pack使用。***

<table>
    <thead>
        <tr>
            <td>资源功能</td>
            <td>5.6.x</td>
            <td>6.0</td>
            <td>6.1</td>
            <td>6.2</td>
            <td>6.3</td>
            <td>6.4</td>
            <td>6.5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/managing-assets-touch-ui.html" target="_blank">触控优化的UI</a>：</strong>
            在台式计算机上或触屏设备上管理资产。</td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/metadata.html" target="_blank">高级元数据管理</a>：</strong>
            元数据模板、元数据架构编辑器和批量元数据编辑。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/task-content.html" target="_blank">任务</a> 和 <a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/projects-with-workflows.html" target="_blank">工作流</a> 管理：</strong>
            利用AEM Projects审阅和批准数字资产的预建工作流和任务。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>可扩展性和性能：</strong>
            增强了对大规模摄取、上传和存储的支持。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/mac-api-assets.html" target="_blank">资源HTTP API</a>：</strong>
            通过HTTP和JSON以编程方式与资产交互。</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/link-sharing.html" target="_blank">链接共享</a>：</strong>
            无需登录即可简单地临时共享数字资产。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/brand-portal/using/brand-portal.html" target="_blank">Brand Portal</a>：</strong>
            用于无缝共享和分发数字资产的云服务SAAS解决方案。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/use-assets-across-connected-assets-instances.html" target="_blank">连接的资产</a>：</strong>
            AEM Sites实例可以无缝地访问和使用其他AEM Assets实例中的资源。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/touch-ui-asset-insights.html" target="_blank">资产分析</a>：</strong>
            利用Adobe Analytics捕获客户对数字资源的交互并在AEM中查看。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/multilingual-assets.html" target="_blank">多语言资产</a>：</strong>
            使用语言根自动翻译支持资源元数据。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/enhanced-smart-tags.html" target="_blank">智能标记和审核</a>：</strong>
            利用Adobe Sensei自动使用有用的元数据标记图像。</td>
            <td> </td>
            <td></td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/assets/using/smart-translation-search-feature-video-use.html" target="_blank">智能翻译搜索</a>：</strong>
            在搜索AEM Assets时自动翻译搜索词。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/indesign.html" target="_blank">Adobe InDesign Server集成</a>：</strong>
            生成产品目录。 根据InDesign模板创建宣传册、传单和印刷广告。</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/cn/experience-manager/desktop-app/aem-desktop-app.html" target="_blank">AEM桌面应用程序</a>：</strong>
            将资源同步到本地桌面以使用Creative Suite产品进行编辑。
            </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/imaging-transcoding-library.html" target="_blank">Adobe Imaging Library</a>：</strong>
                <br> Photoshop和AcrobatPDF库用于高质量文件操作。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://www.adobe.com/cn/creativecloud/business/enterprise/adobe-asset-link.html" target="_blank">Adobe资源链接</a>：</strong>
            直接从Adobe创建云应用程序中访问AEM Assets。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/aem-assets-adobe-stock.html" target="_blank">Adobe Stock集成</a>：</strong>
            直接从AEM无缝访问和使用Adobe Stock图像。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>SP</sup></td>
            <td>✔</td>
        </tr>
    </tbody>
</table>

### AEM Assets Dynamic Media

***✔<sup>+</sup> 此版本中的功能得到了显着增强。***

***✔<sup>SP</sup> 表示该功能可通过Service Pack或Feature Pack使用。***


<table>
    <thead>
        <tr>
            <td>Dynamic Media功能</td>
            <td>5.6.x</td>
            <td>6.0</td>
            <td>6.1</td>
            <td>6.2</td>
            <td>6.3 +FP</td>
            <td>6.4</td>
            <td>6.5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/managing-assets.html" target="_blank">成像</a>：</strong>
            动态交付不同大小和格式的图像，包括智能裁切。</td>
            <td> </td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/video-profiles.html" target="_blank">视频</a>：</strong>
            高级视频编码和自适应视频流</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/interactive-images.html" target="_blank">交互式媒体</a>：</strong>
            创建交互式横幅、包含可点击内容的视频以显示关键选件。
            </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>集(<a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/image-sets.html" target="_blank">图像</a>， <a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/spin-sets.html" target="_blank">旋转</a>， <a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/mixed-media-sets.html" target="_blank">混合媒体</a>)：</strong>
            允许用户缩放、平移、旋转和模拟360度的观看体验。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://experienceleague.adobe.com/docs/" target="_blank">查看器</a>：</strong>
            自定义品牌富媒体播放器和预设，支持不同的屏幕/设备。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/delivering-dynamic-media-assets.html" target="_blank">投放</a>：</strong>
            通过HTTP/2协议链接或嵌入Dynamic Media内容和交付的灵活选项。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>从Scene7升级到Dynamic Media：</strong>
            能够迁移主资产并继续使用现有S7 URL。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
    </tbody>
</table>

## Forms功能

以下是AEM提供的关键AEM Forms附加功能列表。 其中一些功能已在早期版本中引入，并在每个版本中添加了增量增强功能。

***✔<sup>+</sup> 此版本中的功能得到了显着增强。***

***✔<sup>SP</sup> 表示该功能可通过Service Pack或Feature Pack使用。***

<table>
    <thead>
        <tr>
            <td>Forms功能</td>
            <td>5.6.x</td>
            <td>6.0</td>
            <td>6.1</td>
            <td>6.2</td>
            <td>6.3</td>
            <td>6.4</td>
            <td>6.5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/introduction-forms-authoring.html" target="_blank">自适应Forms编辑器</a>：</strong>
            根据设备和浏览器设置创建引人入胜、响应式且自适应化的表单。</td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/generate-document-of-record-for-non-xfa-based-adaptive-forms.html" target="_blank">记录文档</a>：</strong>
            创建文档以确保数据捕获体验或打印就绪版本的长期存储。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/themes.html" target="_blank">主题编辑器</a>：</strong>
            创建可重用主题以设置表单组件和面板的样式。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/template-editor.html" target="_blank">模板编辑器</a>：</strong>
            实现自适应表单最佳实践的标准化和实施。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#EnhancedintegrationwithAdobeSign" target="_blank">Acrobat Sign集成</a>：</strong>
            允许部署基于Acrobat Sign集成表单的签名方案。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/cm-overview.html" target="_blank">通信管理</a>：</strong>
            借助AEM Forms，您可以创建、管理和提供个性化的交互式客户信函。
            </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#AEMFormsdataintegration" target="_blank">第三方数据集成</a>：</strong>
            使用数据集成，根据表单中的用户输入从不同的数据源获取数据。 在提交表单时，捕获的数据将写回数据源。
            </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#FormscentricAEMWorkflowsforAEMFormsonOSGi" target="_blank">用于Forms处理的工作流（在OSGi上）</a>：</strong>
            简化了表单批准流程的部署。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/user-guide.html?topic=/experience-manager/6-5/forms/morehelp/integrations.ug.js" target="_blank">与Marketing Cloud集成</a>：</strong>
            与Adobe Analytics和Adobe Target集成，以增强和衡量客户体验。</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/introduction-managing-forms.html" target="_blank">表单管理器</a>：</strong>
            单个位置管理所有表单/文档/通信，如启用分析、翻译、A/B测试、审阅和发布。
            </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/aem-forms-app.html" target="_blank">AEM Forms应用程序</a>：</strong>
            在iOS、Android或Windows上的应用程序中允许进行在线/离线表单处理。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/adaptive-document.html" target="_blank">交互式通信</a>：</strong>
            使用交互式元素（如图表，以前称为自适应文档）创建丰富的通信（如目标语句）。</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong>Forms处理工作流(J2EE)：</strong>
            使用直观的IDE构建复杂的表单/以文档为中心的工作流。</td>
            <td></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#EnhancedDocumentSecurity" target="_blank">AEM Forms Document Security</a>：</strong>
            对PDF和Office文档的安全访问和授权。
            </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#Simplifiedauthoringexperience" target="_blank">测试框架</a>：</strong>
            使用Calvin框架和Chrome插件支持和调试自适应表单。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
    </tbody>
</table>

