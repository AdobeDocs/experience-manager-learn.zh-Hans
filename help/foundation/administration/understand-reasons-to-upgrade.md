---
title: 了解升级理由
description: 对考虑升级到最新版Adobe Experience Manager的客户进行关键功能的高级细分。
version: 6.5
sub-product: 资产，云管理器，商务，内容服务，动态媒体，表单，基础，屏幕，站点
topics: best-practices, upgrade
audience: all
activity: understand
doc-type: article
translation-type: tm+mt
source-git-commit: 1519856731758ece2860615c06fc0d64edb104a5
workflow-type: tm+mt
source-wordcount: '3540'
ht-degree: 1%

---


# 了解升级理由

对考虑升级到最新版Adobe Experience Manager的客户进行关键功能的高级细分。

## 升级到AEM 6.5的主要功能

+ [Adobe Experience Manager6.5发行说明](https://helpx.adobe.com/cn/experience-manager/6-5/release-notes.html)

### 基础改进

Adobe Experience Manager6.5通过以下方式继续提高系统的稳定性、性能和支持性：

+ **Java 11** 支持（同时维护Java 8支持）。

### 网站创建和管理

AEM Sites推出了许多旨在加快网站创建和构建的功能：

+ **SPA** Editorsupport允许SPA（单页应用程序）在AEM中完全创作，支持丰富的、适合营销人员的创作体验。+_ **JavaScript SDK的**、SPA项目开始工具包和支持构建工具，允许前端开发人员独立于AEM开发与SPA Editor兼容的单页应用程序。
+ **核心** 组件提供大量新组件、组 **件** 库以及对现有核心组件的各种增强。
+ 进一步&#x200B;**翻译**&#x200B;增强简化了AEM Sites的翻译。

### 流畅的体验

AEM继续采用新的、经过改进的工具，方便在AEM之外使用内容，从而提供流畅的体验。

+ **内容** 片段支持版本比较／差异和注释。
+ **AEM Assets HTTP** API支 **持将** 内容片段以JSON形式直 **接在DAM中**。
   **体验** 片段支 **持全文** 搜索和AEM **调度程** 序缓存 **无效以引**&#x200B;用页面。

### 资产管理

AEM Assets继续利用其丰富的资产管理功能来改进DAM的使用、管理和了解。 AEM 6.5继续改进Adobe Creative Cloud与创意工作流之间的集成。

+ **Adobe资产链** 接将创意人员从Adobe Creative Cloud工具直接连接到AEM Assets。
+ **Adobe** Stock集成允许直接从AEM Assets体验直接访问Adobe Stock图像，从而创造无缝的内容发现体验。
+ **AEM桌** 面版本购买2.0版，并在改善性能和稳定性的同时重新规划自己。
+ **Connected** Assets支持离散的AEM Sites实例，无缝访问和使用来自不同AEM Assets实例的资产。
+ 更新了&#x200B;**Dynamic Media**&#x200B;中的视频支持，包括&#x200B;**360视频**&#x200B;和&#x200B;**自定义视频缩略图**。

### 内容智能

AEM继续构建与智能技术的集成，利用机器学习和人工智能改善所有体验。

+ **Adobe资** 产链 **接可添加视觉相似性搜索**，使类似图像能在Adobe Creative Cloud工具中轻松发现 **和使用**。

### 集成

AEM增强了与其他Adobe服务集成的能力：

+ **体验** 片段通过支持以JSON形式 **导出** 到Adobe Target以及 **从Adobe Target删除基于** 体验片段的产 **品的能力，加**  ****&#x200B;深了它们与Adobe目标的集成。

### AMS Cloud Manager

[Cloud Manager](https://adobe.ly/2HODmsv)(Adobe Managed Services(AMS)客户独有的)优惠了以下功能：

+ Cloud Manager支持将AEM部署支持从AEM Sites扩展到&#x200B;**AEM Assets**，包括资产处理的&#x200B;**自动性能测试**。
+ **以预定** 义的阈值自动缩放AEM发布层，确保获得最佳的最终用户体验。
+ **非生产渠** 道允许开发团队利用Cloud Manager持续检查代码质量并部署到较低的环境（开发和QA）。
+ **CI/CD管道API使** 客户能够有计划地与Cloud Manager互动，从而深化与预置开发基础架构的集成可能性。

## 基础功能

以下是AEM提供的主要基础功能的列表。 其中一些功能在早期版本中引入了在每个版本中添加的增量增强功能。

+ [AEM Foundation发行说明](https://helpx.adobe.com/experience-manager/6-5/release-notes/wcm-platform.html)

***✔并<sup>对此</sup> 版本中的功能进行了重要增强。***

***✔  SP表示该功能可通过Service Pack或Feature Pack获得。<sup></sup>***

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
                <strong><a href="https://jackrabbit.apache.org/oak/docs/index.html" target="_blank">Oak Content Repository</a>：与</strong> 前代Jackrabbit 2相比，它提供更出色的性能和可扩展性。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/indexing-via-the-oak-run-jar.html">oak-run.jar索引支持</a>:</strong> 改进了Oak索引的重建／索引、统计信息收集和一致性检查。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/queries-and-indexing.html" target="_blank">自定义搜索索引</a>: </strong>
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/revision-cleanup.html" target="_blank">在线修订清理</a>：在</strong>
                不停机的情况下执行存储库维护。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/storage-elements-in-aem-6.html" target="_blank">TarMK或MongoMK存储库存储</a>:</strong>
                <br> 使用TarMK（下一代版本的TarPM）的简单、基于性能文件的存储的选项，或在MongoDB支持的存
                <br> 储库中使用MongoMK水平缩放。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/aem-with-mongodb.html" target="_blank">MongoMK性能和稳</a>定</strong>
            性：自AEM 6.0推出以来，MongoMK不断得到增强。</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/data-store-config.html#AmazonS3DataStore">AmazonS3 DataStore</a>：利</strong>
            用可扩展的云存储解决方案存储二进制资产。</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>触屏UI功能奇偶校验：</strong>
                继续增强创作UI的功能，提高工作效率并与经典UI实现功能对等性。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Omnisearch：快</strong>
                速搜索和导航AEM。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/operations-dashboard.html" target="_blank">操作仪表板</a>:</strong>
 从AEM中执行维护、监视服务器运行状况并分析性能。</td>
            <td></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/upgrade.html" target="_blank">升级改进</a>:</strong>
            升级改进使AEM的就地升级更简单、更快捷。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/htl/using/overview.html" target="_blank">HTL模板语言</a>:</strong>
            一种将表示与逻辑分离的现代模板引擎。显着缩短组件开发时间。 随每个版本添加的增量功能。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://sling.apache.org/documentation/bundles/models.html" target="_blank">Sling Models</a>:</strong>
            一个灵活的框架，用于将JCR资源建模为业务对象和逻辑。随每个版本添加的增量功能。
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
            <td><strong><a href="https://adobe.ly/2HODmsv" target="_blank">云管理器</a>: </strong>
                Cloud Manager是Adobe Managed Services(AMS)客户独有的，它通过先进的CI/CD管道加快了开发和部署。</td>
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

以下是AEM提供的主要安全功能的列表。 其中一些功能在早期版本中引入了在每个版本中添加的增量增强功能。

+ [安全发行说明](https://helpx.adobe.com/experience-manager/6-5/release-notes/wcm-platform.html#Security)

***✔表示此版本中对该功能进行了重大增强。***

***✔+表<sup></sup> 示该功能可通过Service Pack或功能包使用。***

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
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-service-users.html" target="_blank">服务用</a></strong>
            <br> 户区分权限，避免不必要地使用管理员权限。</td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/saml-2-0-authenticationhandler.html" target="_blank">密钥存储</a></strong>
            <br> 管理全局信任存储、证书和密钥均在存储库中进行管理。</td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><a href="https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/csrf-protection.html" target="_blank"><strong>CSRF</strong> <strong></strong></a>
            <br> 保护跨站点请求伪造现成保护。</td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/saml-2-0-authenticationhandler.html" target="_blank"><strong>CORSsupport跨来源资源共享支持，提高应用程序的灵活性。</strong> <strong></strong></a>
            <br> </td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://docs.adobe.com/docs/en/aem/6-5/administer/security/saml-2-0-authenticationhandler.html" target="_blank">改进的SAML身份验证</a><br>
 </strong>支持改进的SAML重定向、优化的组信息和密钥加密问题已解决。 
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
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ldap-config.html" target="_blank">LDAP作为OSGi配</a><br>
 </strong>置简化LDAP身份验证的管理和更新。</td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong>OSGi对纯文本密码的加密支<br>
 </strong>持密码和其他敏感值可以以加密形式保存并自动解密。</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/user-group-ac-admin.html" target="_blank">CUG增</a><br>
 </strong>强为解决性能和可伸缩性问题，已重新编写关闭的用户组实施。</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔<sup>+</sup></td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/platform-repository/using/ssl-wizard-technical-video-use.html" target="_blank">SSL向</a></strong>
            <br> 导UI可简化SSL的设置和管理。</td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/encapsulated-token.html" target="_blank">封装的令</a></strong>
            <br> 牌支持“粘滞”会话不再需要支持发布实例之间的水平身份验证。</td>
        <td> </td>
        <td> </td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ims-config-and-admin-console.html" target="_blank">AdobeIMS身份验证</a><br>
 </strong>支持仅适用于Adobe Managed Services(AMS)，通过AdobeIMS(Identity Management系统)集中管理对AEM作者实例的访问。</td>
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

以下是AEM提供的主要站点功能的列表。 其中一些功能在早期版本中引入了在每个版本中添加的增量增强功能。

+ [AEM Sites发行说明](https://helpx.adobe.com/experience-manager/6-5/release-notes/sites.html)

***✔并<sup>对此</sup> 版本中的功能进行了重要增强。***

***✔  SP表示该功能可通过Service Pack或Feature Pack获得。<sup></sup>***

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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/page-editor-feature-video-use.html" target="_blank">触屏优化页面创作</a>:</strong>
            使编辑人员能够利用带触摸屏的平板电脑和计算机。</td>
            <td></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/responsive-layout.html" target="_blank">响应式网站创作</a>:</strong>
                布局模式允许编辑人员根据响应式网站的设备宽度调整组件大小。</td>
            <td></td>
            <td></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/template-editor-feature-video-use.html" target="_blank">可编辑模板</a>:</strong>
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/core-components/user-guide.html" target="_blank">核心组件</a>：加</strong>
            快站点开发。可在GitHub上获得，可频繁发布计划和灵活性。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/spa-overview.html" target="_blank">SPA Editor</a>:</strong>
            使用基于React或Angular的单页应用程序(SPA)框架创建可创作、引人入胜的Web体验。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/release-notes/style-system-fp.html" target="_blank">样式系统</a>:</strong>
            通过使用上下文样式系统定义AEM组件的可视外观，从而增加组件的重用。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>SP</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/msm.html" target="_blank">多站点管理器(MSM)</a>:</strong>
            管理共享通用内容（即多语言、多品牌）的多个网站。</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/translation.html" target="_blank">内容翻译</a>:</strong>
            即插即用框架与行业领先的第三方翻译服务集成。</td>
            <td></td>
            <td></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/contexthub.html" target="_blank">ContextHub</a>：下</strong>
            一代Client Context Framework，用于内容个性化。</td>
            <td></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/launches.html" target="_blank">启动项</a>:</strong>
            为未来版本开发内容，不中断日常创作。</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/content-fragments-feature-video-understand.html" target="_blank">内容片段</a>:</strong>
            创建和管理与演示文稿脱钩的编辑内容，以便轻松重复使用。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/experience-fragments-feature-video-use.html" target="_blank">体验片段</a>:</strong>
            创建针对桌面、移动和社交渠道优化的可重复使用体验和变体。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/release-notes/content-services-fragments-featurepack.html" target="_blank">内容服务</a>:</strong>
            将AEM中的内容导出为JSON，供跨设备和应用程序使用。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>SP</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Adobe Analytics集成和内容洞察：</strong>
                轻松集成Adobe Analytics和DTM。在创作环境中显示性能信息。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/content-targeting-touch.html" target="_blank">Adobe Target集</a></strong>
            成：分步向导，用于创建有针对性的体验、创建可重用的优惠库。</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/campaign.html" target="_blank">Adobe Campaign集成</a>:</strong>
            轻松与下一代电子邮件活动解决方案集成。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/using/aem_launch_adobeio_integration.html" target="_blank">Adobe启动集</a>成：</strong>
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/aem-screens-introduction.html" target="_blank">屏幕</a>:</strong>
            管理数字标牌和展台的体验。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ecommerce.html" target="_blank">电子商务</a>:</strong>
            跨网络、移动和社交接触点提供品牌化、个性化的购物体验。
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/overview.html" target="_blank">社区</a>：论</strong>
            坛、线程化评论、事件日历和许多其他功能允许与网站访客深入互动。</td>
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

## 资源功能

以下是AEM提供的主要资产功能列表。 其中一些功能在早期版本中引入了在每个版本中添加的增量增强功能。

+ [AEM Assets发行说明](https://helpx.adobe.com/experience-manager/6-5/release-notes/assets.html)

***✔表示此版本中对该功能进行了重大增强。***

***✔+表<sup></sup> 示该功能可通过Service Pack或功能包使用。***

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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/managing-assets-touch-ui.html" target="_blank">触屏优化UI</a>:</strong>
            在桌面计算机或触屏优化设备上管理资产。</td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/metadata.html" target="_blank">高级元数据管理</a>:</strong>
            元数据模板、元数据模式编辑器和批量元数据编辑。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/task-content.html" target="_blank">任</a> 务和 <a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/projects-with-workflows.html" target="_blank"></a> 工作流管</strong>
            理：预建工作流和任务，以利用AEM项目对数字资产进行审核和批准。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>可伸缩性和性能：</strong>
            增强的大规模摄取、上传和存储支持。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/mac-api-assets.html" target="_blank">资产HTTP API</a>:</strong>
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/link-sharing.html" target="_blank">链接共享</a>:</strong>
            无需登录即可轻松进行数字资产的临时共享。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/brand-portal/using/brand-portal.html" target="_blank">Brand Portal</a>:</strong>
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/use-assets-across-connected-assets-instances.html" target="_blank">连接的资产</a>:</strong>
            AEM Sites实例可以无缝访问和使用来自其他AEM Assets实例的资产。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/touch-ui-asset-insights.html" target="_blank">资产洞察</a>：利</strong>
            用Adobe Analytics，捕捉AEM数字资产和视图的客户互动。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/multilingual-assets.html" target="_blank">多语言资产</a>:</strong>
            资产元数据的翻译支持自动与语言根目录结合使用。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/enhanced-smart-tags.html" target="_blank">智能标记和协调</a>：利</strong>
            用Adobe Sensei自动使用有用的元数据标记图像。</td>
            <td> </td>
            <td></td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/assets/using/smart-translation-search-feature-video-use.html" target="_blank">智能翻译搜索</a>:</strong>
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/indesign.html" target="_blank">Adobe InDesign Server集成</a>:</strong>
            生成产品目录。根据InDesign模板制作小册子、广告传单和印制广告。</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/desktop-app/aem-desktop-app.html" target="_blank">AEM桌面应用程序</a>:</strong>
            将资源同步到本地桌面，以便使用Creative Suite产品进行编辑。
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/imaging-transcoding-library.html" target="_blank">Adobe成像库</a>:</strong>
                <br> 用于高质量文件处理的Photoshop和AcrobatPDF库。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://www.adobe.com/cn/creativecloud/business/enterprise/adobe-asset-link.html" target="_blank">Adobe资产链接</a>:</strong>
            直接从Adobe创建云应用程序访问AEM Assets。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/aem-assets-adobe-stock.html" target="_blank">Adobe Stock集成</a>:</strong>
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

### AEM Assets动态媒体

***✔并<sup>对此</sup> 版本中的功能进行了重要增强。***

***✔  SP表示该功能可通过Service Pack或Feature Pack获得。<sup></sup>***


<table>
    <thead>
        <tr>
            <td>动态媒体功能</td>
            <td>5.6.x</td>
            <td>6.0</td>
            <td>6.1</td>
            <td>6.2</td>
            <td>6.3 + FP</td>
            <td>6.4</td>
            <td>6.5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/managing-assets.html" target="_blank">成像</a>:</strong>
            动态传送不同大小和格式的图像，包括Smart Crop。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/video-profiles.html" target="_blank">视频</a>：高</strong>
            级视频编码和自适应视频流</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/interactive-images.html" target="_blank">交互式媒体</a>:</strong>
            创建交互式横幅、包含可点击内容的视频以展示关键优惠。
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
            <td><strong>设置(<a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/image-sets.html" target="_blank">图像</a>、旋 <a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/spin-sets.html" target="_blank">转</a>、混 <a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/mixed-media-sets.html" target="_blank">合媒体</a>):</strong>
            允许用户缩放、平移、旋转和模拟360度全方位查看体验。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://docs.adobe.com/docs/en/aem/6-5/administer/content/dynamic-media/viewer-presets.html" target="_blank">查看器</a>:</strong>
            支持不同屏幕／设备的自定义品牌富媒体播放器和预设。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/delivering-dynamic-media-assets.html" target="_blank">投放</a></strong>
            ：用于链接或嵌入Dynamic Media内容的灵活选项以及通过HTTP/2协议投放。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>从Scene7升级到Dynamic Media:</strong>
            能够迁移主控资产并继续使用现有S7 URL。</td>
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

以下是AEM提供的主要AEM Forms附加功能的列表。 其中一些功能在早期版本中引入了在每个版本中添加的增量增强功能。

+ [AEM Forms发行说明](https://helpx.adobe.com/experience-manager/6-5/release-notes/forms.html)

***✔并<sup>对此</sup> 版本中的功能进行了重要增强。***

***✔  SP表示该功能可通过Service Pack或Feature Pack获得。<sup></sup>***

<table>
    <thead>
        <tr>
            <td>Forms特征</td>
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/introduction-forms-authoring.html" target="_blank">自适应Forms编辑</a>:</strong>
            根据设备和浏览器设置创建引人入胜、响应迅速的自适应表单。</td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/generate-document-of-record-for-non-xfa-based-adaptive-forms.html" target="_blank">文档记录</a>:</strong>
            创建文档以确保长期存储数据捕获体验或打印就绪版本。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/themes.html" target="_blank">主题编辑器</a>:</strong>
            创建可重用的主题，为表单的组件和面板设计样式。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/template-editor.html" target="_blank">模板编辑器</a>:</strong>
            实现自适应表单的标准化并实施最佳实践。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#EnhancedintegrationwithAdobeSign" target="_blank">Adobe Sign集成</a>:</strong>
            允许部署基于Adobe Sign综合表单的签名方案。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/cm-overview.html" target="_blank">通信管理</a>:</strong>
            借助AEM Forms，您可以创建、管理和提供个性化的交互式客户通信。
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
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#AEMFormsdataintegration" target="_blank">第三方数据集成</a>:</strong>
            使用数据集成，根据表单中的用户输入从不同的数据源获取数据。提交表单时，捕获的数据将写回数据源。
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
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#FormscentricAEMWorkflowsforAEMFormsonOSGi" target="_blank">用于Forms处理的工作流(在OSGi上</a>):</strong>
            简化表单审批流程的部署。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/user-guide.html?topic=/experience-manager/6-5/forms/morehelp/integrations.ug.js" target="_blank">与Marketing Cloud集成</a>:</strong>
            与Adobe Analytics和Adobe Target集成以增强和衡量客户体验。</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/introduction-managing-forms.html" target="_blank">表单管理器</a>:</strong>
            单一位置，用于管理所有表单/文档/通信，如启用分析、翻译、A/B测试、审阅和发布。
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/aem-forms-app.html" target="_blank">AEM Forms应用</a>:</strong>
            允许在iOS、Android或Windows上的应用程序内处理联机／脱机表单。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/adaptive-document.html" target="_blank">交互式通信</a>:</strong>
            创建丰富的通信，如具有交互式元素（如图表）的目标语句(以前称为自适应文档)。</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/pdf/aem-forms/6-5/WorkbenchHelp.pdf" target="_blank">针对Forms处理的工作流程(J2EE)</a>:</strong>
            利用直观的IDE构建复杂的表单／以文档为中心的工作流。</td>
            <td></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#EnhancedDocumentSecurity" target="_blank">AEM Forms文档安全</a>:</strong>
            安全访问和授权PDF和Office文档。
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
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#Simplifiedauthoringexperience" target="_blank">测试框架</a>:</strong>
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

## 社区功能

以下是AEM提供的主要AEM Communities附加功能的列表。 其中一些功能在早期版本中引入了在每个版本中添加的增量增强功能。

+ [AEM Communities新功能摘要](https://helpx.adobe.com/experience-manager/6-5/communities/using/whats-new-aem-communities.html#main-pars_text)

***✔并<sup>对此</sup> 版本中的功能进行了重要增强。***

***✔  SP表示该功能可通过Service Pack或Feature Pack获得。<sup></sup>***

<table>
    <thead>
        <tr>
            <td> </td>
            <td>社区功能</td>
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
            <td rowspan="7">社区功能</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/forum.html" target="_blank">论坛</a>:</strong> （社交组件框架）创建新主题或视图、关注、搜索和移动现有主题。</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td>
                <p><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-qna.html" target="_blank">问题解答</a>:</strong>
                提问、视图和回答问题。</p>
            </td>
            <td></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td>
                <strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/blog-feature.html" target="_blank">博客</a>:</strong>
                在发布端创建博客文章和评论。
            </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td>
                <strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/ideation-feature.html" target="_blank">构思</a></strong>
                ：创建和与社区共享构思，或者视图，关注现有构思并对其进行评论。
            </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td>
                <strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/calendar.html" target="_blank">日历</a>:</strong>
                （社交组件框架）向站点访客提供社区事件信息。
            </td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td>
                <strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/file-library.html" target="_blank">文件库</a>:</strong>
                上传、管理和下载社区站点内的文件。</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/users.html#AboutCommunityGroups" target="_blank">用户组</a>:
            </strong>一组用户可以属于成员组，并可以集体分配角色。</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong> </strong></td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/resource.html" target="_blank">分配</a>：创</strong>
            建并向社区成员分配学习资源。</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td rowspan="5">启用</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/catalog.html" target="_blank">编</a> 录和资 <a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/resource.html" target="_blank">源管理</a>:</strong>
            从目录访问启用资源。</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/resources.html#CreateaLearningPath" target="_blank">学习路径管理</a>:</strong>
            管理课程或支持资源组。</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/reports.html#main-pars_text_1739724213" target="_blank">启用报告</a>:</strong>
            报告启用资源和学习路径。</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/resources.html#main-pars_text_899882038" target="_blank">Enablement on Enablement</a>:</strong>
            添加对Enablement Resource的评论。</td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/analytics.html" target="_blank">Enablement Analytics</a>:</strong>
            视频分析、进度报告和分配报告</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td rowspan="8">Commons</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/comments.html" target="_blank">评</a> 论和附件：</strong>
            （社交组件框架）作为社区成员共享有关社区站点上内容的意见和知识。</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>内容片段转换：</strong>
            将UGC贡献转换为内容片段。</td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/reviews.html" target="_blank">评论</a>:</strong>
                （社交组件框架）作为社区成员，使用评论和评级功能的组合来评论某段内容。</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/rating.html" target="_blank">评级</a>:/strong&gt;（社交组件框架）作为社区成员对某个内容进行评级。</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/voting.html" target="_blank">投票</a>:</strong>
                （社交组件框架）作为社区成员对某一内容进行上诉或下诉。</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/tag-ugc.html" target="_blank">标记</a>:</strong>
            将标记（关键字或标签）与内容相连，以便快速定位内容。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/search.html" target="_blank">搜索</a>：预</strong>
            测性和提示性搜索。</td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/translate-ugc.html" target="_blank">翻译</a>:</strong>
            用户生成内容的机器翻译。</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td rowspan="10">管理</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/create-site.html" target="_blank">站点管理</a>:</strong>
            创建具有社区功能的站点。</td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/sites.html" target="_blank">模板</a>:</strong>
                <a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/sites.html" target="_blank"></a> 站点 <a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/tools-groups.html" target="_blank"></a> 和组模板，用于基于向导创建功能完整的社区站点。</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>可编辑模板：</strong>
            使社区管理员能使用AEM可编辑模板构建丰富的体验。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/creating-groups.html" target="_blank">组或子社区</a>:</strong>
            在社区站点内动态创建子社区。
            </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/in-context.html" target="_blank">协调</a>:</strong>
            协调用户生成的内容。
            </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/moderation.html" target="_blank">批量审核</a>:</strong>
            审核控制台可批量管理用户生成的内容。</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/moderate-ugc.html#CommonModerationConcepts" target="_blank">垃圾邮件检测和不良过滤器</a>：自</strong>
            动垃圾邮件检测。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/members.html" target="_blank">成员管理</a>:</strong>
            从成员管理区域管理用户用户档案和用户组。</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/overview.html#main-pars_text_866731966" target="_blank">响应式设计</a>:</strong>
            AEM Communities站点是响应式。
            </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/analytics.html" target="_blank">分析</a>:</strong>
            与Adobe Analytics集成，获得社区站点使用情况的重要洞察。</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td rowspan="4">成员</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/advanced.html" target="_blank">评分和徽章</a>:</strong>
            (由Adobe Sensei提供高级评分)确定社区成员是专家并给予奖励。</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/activities.html" target="_blank">活</a> 动和 <a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/notifications.html" target="_blank">通知</a>:视图</strong>
            最近的活动流，并获得有关事件的通知。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/configure-messaging.html" target="_blank">消息</a>:</strong>
            直接向用户和用户组发送消息。</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/social-login.html" target="_blank">社交登录</a></strong>
            ：使用其Facebook或Twitter帐户登录。</td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td rowspan="5">平台</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-srp.html" target="_blank">MSRP(Mongo存储</a>):</strong>
            用户生成的内容(UGC)直接保留在本地MongoDB实例中</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-srp.html" target="_blank">DSRP(数据库存储</a>):</strong>
            用户生成的内容(UGC)直接保留在本地MySQL数据库实例中。</td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-srp.html" target="_blank">SRP(云存储)</a>:</strong>
                用户生成的内容(UGC)在由Adobe托管和管理的云服务中远程保留。</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-srp.html" target="_blank"><strong>JSRP</a>：社</strong>
                区内容存储在JCR中，UGC可从发布它的作者（或发布）实例访问。</td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/sync.html" target="_blank">用户和组同步</a>:</strong>
            在使用发布场拓扑时，在发布实例中同步用户和组。</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
    </tbody>
</table>

AEM Communities通过发行版添加[增强](https://helpx.adobe.com/experience-manager/6-5/communities/using/whats-new-aem-communities.html)，使组织能够吸引和使其用户参与，具体方法如下：

+ **@** mention支持用户生成的内容。
+ 通过&#x200B;**Enablement**&#x200B;组件中的&#x200B;**键盘导航**&#x200B;改进辅助功能。
+ 使用&#x200B;**自定义过滤器**&#x200B;改进了&#x200B;**批量审核**。
+ **可编** 辑的模板，使社区管理员能够在AEM中构建丰富的社区体验。
+ 用户现在可以批量&#x200B;**将**&#x200B;直接消息发送给组的所有成员。
