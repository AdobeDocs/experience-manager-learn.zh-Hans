---
title: Dynamic Media经典主工作流和预览资产
description: 了解Dynamic Media Classic中的主工作流，该工作流包括三个步骤：创建（和上传）、创作（和发布）和交付。 然后了解如何在Dynamic Media Classic中预览资源。
sub-product: dynamic-media
feature: Dynamic Media Classic
doc-type: tutorial
topics: development, authoring, configuring, architecture, publishing
audience: all
activity: use
topic: Content Management
role: Business Practitioner
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '2737'
ht-degree: 1%

---


# Dynamic Media经典主工作流和预览资产{#main-workflow}

Dynamic Media支持创建（和上传）、创作（和发布）和传送工作流程。 您可以通过上传资产来进行开始，然后对这些资产执行一些操作，如构建图像集，最后进行发布以使其生效。 生成步骤对于某些工作流是可选的。 例如，如果您的目标是仅对图像进行动态缩放和缩放，或转换和发布视频以进行流播放，则无需执行构建步骤。

![图像](assets/main-workflow/create-author-deliver.jpg)

Dynamic Media Classic解决方案中的工作流包括三个主要步骤：

1. 创建（并上传）SourceContent
2. 创作（和发布）资产
3. 交付资产

## 第1步：创建（并上传）

这是工作流的开始。 在此步骤中，您将收集或创建适合您所使用工作流的源内容，并将其上传到Dynamic Media Classic。 该系统支持图像、视频和字体的多种文件类型，也支持PDF、Adobe Illustrator和Adobe InDesign。

请参见[支持的文件类型](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/upload-publish/uploading-files.html#supported-asset-file-formats)的完整列表。

您可以通过多种不同的方式上传源内容：

- 直接从桌面或本地网络访问。 [了解如何](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/upload-publish/uploading-files.html#upload-files-using-sps-desktop-application)。
- 从Dynamic Media Classic FTP服务器。 [了解如何](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/upload-publish/uploading-files.html#upload-files-using-via-ftp)。

默认模式为“从桌面”，您可以在该模式下浏览本地网络上的文件并开始上传。

![图像](assets/main-workflow/upload.jpg)

>[!TIP]
>
>请勿手动添加您的文件夹。 而应从FTP运行上传，然后使用&#x200B;**“包含子文件夹**”选项在Dynamic Media Classic中重新创建文件夹结构。

默认情况下，启用两个最重要的上载选项：**标记为发布**（我们前面讨论过）和&#x200B;**覆盖**。 “覆盖”表示，如果正在上载的文件与系统中已有的文件同名，则新文件将替换现有版本。 如果取消选中此选项，则可能无法上载文件。

### 上传图像时覆盖选项

可以为整个公司设置“覆盖图像”选项的四种变体，这些变体经常被误解。 简而言之，您可以设置规则，以便希望更频繁地覆盖具有相同名称的资产，或者希望更少地发生覆盖（在这种情况下，新图像将以“–1”或“–2”扩展名重命名）。

- **在当前文件夹中覆盖，基本图像名称/扩展名相同**。此选项是最严格的替换规则。 它要求您将替换图像上传到与原始图像相同的文件夹，并且替换图像的文件扩展名与原始图像的扩展名相同。 如果这些要求未得到满足，则会创建重复。

- **在当前文件夹中覆盖，无论扩展名如何，基本资源名称都相同**。要求将替换图像上传到与原始图像相同的文件夹，但文件扩展名可能与原始图像不同。 例如，chair.tif将替换chair.jpg。

- **在任意文件夹中覆盖，基本资产名称/扩展名相同**。要求替换图像的文件扩展名与原始图像相同（例如，chair.jpg必须替换chair.jpg，而不是chair.tif）。 但是，您可以将替换图像上传到与原始图像不同的文件夹。 更新后的图像驻留在新文件夹中；在文件的原始位置找不到该文件。

- **在任意文件夹中覆盖相同的基本资产名称，而不考虑扩展名**。此选项是最具包容性的替换规则。 您可以将替换图像上传到与原始图像不同的文件夹，以其他文件扩展名上传文件，并替换原始文件。 如果原始文件位于其他文件夹中，则替换图像将驻留在其上传到的新文件夹中。

了解有关[覆盖图像选项](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/setup/application-setup.html#using-the-overwrite-images-option)的更多信息。

虽然不需要，但是使用上述两种方法之一进行上传时，您可以为该特定上传指定作业选项 — 例如，计划重复上传、在上传时设置裁剪选项等。 这对于某些工作流来说很有价值，因此值得考虑是否适合您。

了解有关[作业选项](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/upload-publish/uploading-files.html#upload-options)的更多信息。

上传是任何工作流中的第一步，因为Dynamic Media Classic无法处理其系统中尚未包含的任何内容。 在上传期间，系统会在后台将每个上传的资产注册到集中的Dynamic Media Classic数据库，分配一个ID，并将其复制到存储。 此外，系统会将图像文件转换为允许动态调整大小和缩放的格式，并将视频文件转换为MP4 Web友好格式。

### 概念：以下是将图像上传到Dynamic Media Classic时的操作

当您将任何类型的图像上传到Dynamic Media Classic时，该图像将转换为称为Pyramid TIFF或P-TIFF的主控图像格式。 P-TIFF与分层TIFF位图图像的格式类似，不同之处在于，该文件包含同一图像的多种大小（分辨率），而不是不同的图层。

![图像](assets/main-workflow/pyramid-p-tiff.png)

在转换图像时，Dynamic Media Classic会拍摄图像的全部大小的“快照”，将图像缩放一半并保存，再缩放一半并保存，等等，直到填充的是原始大小的偶数倍。 例如，一个2000像素的P-TIFF在同一文件中将有1000、500、250和125像素大小（及更小）。 P-TIFF文件是Dynamic Media Classic中称为“主控图像”的格式。

当您请求特定大小的图像时，创建P-TIFF可让Dynamic Media Classic的图像服务器快速找到下一个较大的图像并将其缩小。 例如，如果您上传2000像素的图像并请求100像素的图像，Dynamic Media Classic将找到125像素版本并将其缩小到100像素，而不是从2000像素缩放到100像素。 这使得操作非常快。 此外，在放大图像时，这使缩放查看器能够仅请求缩放的图像拼贴，而不是请求整个全分辨率图像。 这就是主控图像格式P-TIFF文件支持动态调整大小和缩放的方式。

同样，您也可以将主控源视频上传到Dynamic Media Classic，上传Dynamic Media Classic后，可以自动调整视频大小并将其转换为适合Web的MP4格式。

### 确定上传图像的最佳大小的经验法则

**以您需要的最大大小上传图像。**

- 如果需要缩放，请上传最长尺寸范围为1500-2500像素的高分辨率图像。 请考虑您要提供多少细节、源图像的质量以及要显示的产品的大小。 例如，上传一个1000像素的小环图像，而上传整个会议室的一个3000像素图像。
- 如果您不需要缩放，请以可见的相同大小上传它。 例如，如果要将徽标或初始/横幅图像放置到页面上，请精确上传其大小为1:1的图像，然后完全按该大小调用它们。

**在上传到Dynamic Media Classic之前，切勿对图像进行上采样或放大。** 例如，不要向上取样小图像以使其成为2000像素的图像。看起来不妙。 在上传之前，使图像尽可能接近完美。

**缩放没有最小大小，但默认情况下，查看器不会超过100%。** 如果图像太小，它将不会缩放，或只会缩放很小的图像，以防图像看起来不好。

**虽然图像大小没有最低要求，但我们不建议上传巨型图像。** 一幅巨图可以视为4000+像素。上传此大小的图像可能会显示图像中的灰尘颗粒或毛发等潜在缺陷。 此类图像还将占用Dynamic Media Classic服务器上的更多空间，这可能会导致您超出合同规定的存储限制。

了解有关[上载文件](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/upload-publish/uploading-files.html#uploading-your-files)的更多信息。

## 第2步：创作（和发布）

在创建和上传内容后，您将通过执行一个或多个子工作流，从上传的资产创作新的富媒体资产。 这包括所有不同类型的集合 — 图像、色板、旋转和混合媒体集以及模板。 它还包含视频。 稍后，我们将详细介绍每种类型的图像集合和视频富媒体。 但是，在几乎所有情况下，您都可以通过选择一个或多个资产（或未选择任何资产）并选择要构建的资产类型来进行开始。 例如，您可以选择一个主图像和该图像的几视图，然后选择构建一个图像集，它是同一产品的一组替代视图。

>[!IMPORTANT]
>
>确保您的所有资产都已标记为发布。 默认情况下，上传时会自动将所有资产标记为发布，但根据您的已上传内容新创作的资产也需要标记为发布。

构建新资产后，您将运行发布作业。 您可以手动完成此操作，也可以计划自动运行的发布作业。 发布会将所有内容从专用的Dynamic Media Classic球体复制到公共，发布等式的服务器球体。 Dynamic Media发布作业的产品是每个已发布资产的唯一URL。

您发布到的服务器取决于内容和工作流的类型。 例如，所有图像都将转到图像服务器，并将视频流化到FMS服务器。 为方便起见，我们将将“发布”作为单一事件发布到单台服务器。

发布时会发布所有标记为发布的内容，而不仅仅是您的内容。 单个管理员通常代表所有人而不是运行发布的个人用户进行发布。 管理员可以根据需要进行发布，或设置一个每日、每周、甚至每10分钟循环发布的作业，该作业将自动发布。 在对您的业务有意义的计划上发布。

>[!TIP]
>
>自动完成发布作业并计划完整发布，以便每天在半夜12:00或晚上任何时间运行。

### 概念：了解Dynamic Media Classic URL

Dynamic Media Classic工作流的最终产品是指向资产（无论是图像集还是自适应视频集）的URL。 这些URL可以非常预测，并且遵循相同的模式。 对于图像，每幅图像都由P-TIFF主控图像生成。

以下是图像URL的语法，其中包含几个示例：

![图像](assets/main-workflow/dmc-url.jpg)

在URL中，问号左侧的所有内容都是指向特定图像的虚拟路径。 问号右侧的所有内容都是图像服务器修饰符，它说明了如何处理图像。 当您有多个修饰符时，它们会用“和号”分隔。

在第一个示例中，图像“Backpack_A”的虚拟路径为`http://sample.scene7.com/is/image/s7train/BackpackA`。 图像服务器修饰符将图像的大小调整为250像素（从wid=250）的宽度，并使用Lanczos插值算法对图像重新采样，该算法在调整图像大小时（从resMode=sharp2）进行锐化。

第二个示例将称为“图像预设”的内容应用到同一Backpack_A图像，如$！所示_template300$。 表达式两侧的$符号表示图像预设（一组打包的图像修饰符）正应用于图像。

了解Dynamic Media Classic URL的组合方式后，您便会了解如何通过编程方式更改它们，以及如何将它们更深地集成到站点和后端系统中。

### 概念：了解缓存延迟

新上传和发布的资产将立即显示，而更新的资产可能会延迟10小时缓存。 默认情况下，所有已发布的资产在过期前至少要有10小时。 我们认为最低值，因为每次查看图像时，它都会开始一个时钟，直到10小时过后，没有人查看过该图像时钟才会过期。 此10小时时段是资产的“生存时间”。 该资产的缓存到期后，即可交付更新的版本。

除非出现错误，并且图像/资产的名称与之前发布的版本相同，否则通常不会出现此问题，但是图像有问题。 例如，您意外上传了低分辨率版本，或者您的艺术总监未批准图像。 在这种情况下，您需要重新调用原始图像，并使用相同的资产ID将其替换为新版本。

了解如何[手动清除需要更新的URL的缓存](https://docs.adobe.com/content/help/en/experience-manager-65/assets/dynamic/invalidate-cdn-cached-content.html)。

>[!TIP]
>
>要避免缓存延迟的问题，请始终继续工作 — 一个晚上、一天、两周等。 在向公众发布之前，及时为内部各方提供QA/接受，以验证您的工作。 甚至在前一晚工作也允许您更改和重新发布当天晚上的内容。 到早上，10小时已过，缓存会使用正确的映像进行更新。

- 了解有关[创建发布作业](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/upload-publish/publishing-files.html#creating-a-publish-job)的更多信息。
- 了解有关[发布](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/upload-publish/publishing-files.html)的更多信息。

## 第3步：交付

请记住，Dynamic Media Classic工作流程的最终产品是指向资产的URL。 该URL可能指向单个图像、图像集、旋转集或其他图像集集合或视频。 您需要使用该URL并执行相应操作，例如编辑您的HTML，以使`<IMG>`标签指向Dynamic Media经典图像，而不是指向来自当前站点的图像。

在“传送”步骤中，您必须将这些URL集成到您的网站、移动应用程序、电子邮件活动或要在其上显示资产的任何其他数字接触点中。

将图像的Dynamic Media经典URL集成到网站的示例：

![图像](assets/main-workflow/example-url-1.png)

红色的URL是特定于Dynamic Media Classic的唯一元素。

您的IT团队或集成合作伙伴可以带头编写和更改代码，以将Dynamic Media Classic URL集成到您的站点中。 Adobe有一个咨询团队，可以通过提供技术、创意或一般指导来帮助您完成这一工作。

对于缩放查看器或将缩放与替代视图相结合的查看器等更复杂的解决方案，通常URL会指向由Dynamic Media Classic托管的查看器，并且该URL中也会引用资产ID。

在新弹出窗口中在查看器中打开图像集的链接示例（以红色表示）：

![图像](assets/main-workflow/example-url-2.png)

>[!IMPORTANT]
>
>您需要将Dynamic Media Classic URL集成到您的网站、移动应用程序、电子邮件和其他数字接触点中 — Dynamic Media Classic无法帮您实现！

## 预览资产

您可能希望预览您上传或正在创建或编辑的资产，以确保在客户视图资产时，资产按您需要的方式显示。 您可以通过单击资产缩略图上的&#x200B;**预览**&#x200B;按钮、**浏览/构建面板**&#x200B;顶部的按钮或转到&#x200B;**文件>预览**&#x200B;来访问预览窗口。 在浏览器窗口中，它将预览面板中当前位于哪个资源，无论是图像、视频还是像图像集一样构建的资源。

### 动态大小预览（图像预设）

可以使用&#x200B;**Sizes**&#x200B;预览以多种大小预览图像。 这会载入您可用图像预设的列表。 我们稍后将讨论图像预设，但会将其视为以指定大小加载图像的“方法”，同时具有特定锐化和图像质量。

### 缩放预览

您还可以使用&#x200B;**缩放**&#x200B;选项，将图像预览到许多预置缩放预设中的一个，这些预设基于包含的不同缩放查看器。

了解有关[预览资产](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/managing-assets/previewing-asset.html)的更多信息。
