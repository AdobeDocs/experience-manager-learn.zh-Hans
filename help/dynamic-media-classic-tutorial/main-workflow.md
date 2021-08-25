---
title: Dynamic Media Classic主工作流和预览资产
description: 了解Dynamic Media Classic中的主工作流，该工作流包括三个步骤：创建（和上传）、创作（和发布）和交付。 然后，了解如何在Dynamic Media Classic中预览资产。
sub-product: dynamic-media
feature: Dynamic Media Classic
doc-type: tutorial
topics: development, authoring, configuring, architecture, publishing
audience: all
activity: use
topic: Content Management
role: User
level: Beginner
source-git-commit: ea7d49985e69ecf9713e17e51587125b3fb400ee
workflow-type: tm+mt
source-wordcount: '2710'
ht-degree: 0%

---


# Dynamic Media Classic主工作流和预览资产 {#main-workflow}

Dynamic Media支持创建（和上传）、创作（和发布）和交付工作流程。 您首先需要上传资产，然后对这些资产执行一些操作，例如构建图像集，最后发布以使其生效。 对于某些工作流，“生成”步骤是可选的。 例如，如果您的目标是仅对图像进行动态大小调整和缩放，或者转换和发布视频以进行流播放，则无需执行必要的构建步骤。

![图像](assets/main-workflow/create-author-deliver.jpg)

Dynamic Media Classic解决方案中的工作流包含三个主要步骤：

1. 创建（和上传）SourceContent
2. 创作（和发布）资产
3. 交付资产

## 步骤1:创建（和上传）

这是工作流的开始。 在此步骤中，您收集或创建适合您所使用工作流的源内容，并将其上传到Dynamic Media Classic。 系统支持多种文件类型用于图像、视频和字体，也支持PDF、Adobe Illustrator和Adobe InDesign。

请参阅[支持的文件类型](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/upload-publish/uploading-files.html#supported-asset-file-formats)的完整列表。

您可以通过多种不同方式上传源内容：

- 直接从桌面或本地网络访问。 [了解如何](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/upload-publish/uploading-files.html#upload-files-using-sps-desktop-application)。
- 从Dynamic Media Classic FTP服务器。 [了解如何](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/upload-publish/uploading-files.html#upload-files-using-via-ftp)。

默认模式为“从桌面”，您可以在此处浏览本地网络上的文件，然后开始上传。

![图像](assets/main-workflow/upload.jpg)

>[!TIP]
>
>请勿手动添加文件夹。 请改为从FTP运行上传，然后使用&#x200B;**Include Suffilders**&#x200B;选项在Dynamic Media Classic中重新创建文件夹结构。

默认情况下，启用了两个最重要的上载选项： **标记为发布**（我们之前已讨论过）和&#x200B;**覆盖**。 覆盖表示如果正在上传的文件与系统中已有的文件同名，则新文件将替换现有版本。 如果取消勾选此选项，则文件可能无法上传。

### 上传图像时覆盖选项

您可以为整个公司设置覆盖图像选项的四个变体，这些变体经常会被误解。 简而言之，您可以设置规则，以便更频繁地覆盖具有相同名称的资产，或者希望更少地频繁地覆盖资产（在这种情况下，新图像将被重命名为“–1”或“–2”扩展）。

- **在当前文件夹中覆盖，基本图像名称/扩展名相同**。此选项是最严格的替换规则。 它要求您将替换图像上传到与原始图像相同的文件夹，并且替换图像的文件扩展名与原始图像相同。 如果不满足这些要求，则会创建重复项。

- **覆盖当前文件夹中的相同基本资产名称，而不考虑扩展名**。要求您将替换图像上传到与原始图像相同的文件夹，但文件扩展名可能与原始图像不同。 例如， chair.tif将取代chair.jpg。

- **在任何文件夹中，覆盖相同的基本资产名称/扩展名**。要求替换图像的文件扩展名与原始图像相同（例如， chair.jpg必须替换chair.jpg，而不是chair.tif）。 但是，您可以将替换图像上传到与原始图像不同的文件夹。 更新后的图像位于新文件夹中；在文件的原始位置中无法再找到该文件。

- **在任意文件夹中覆盖相同的基本资产名称，而不考虑扩展名**。此选项是包含最广的替换规则。 您可以将替换图像上传到与原始图像不同的文件夹，上传文件扩展名不同的文件，然后替换原始文件。 如果原始文件位于其他文件夹中，则替换图像将位于上传到的新文件夹中。

了解有关[覆盖图像选项](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/setup/application-setup.html#using-the-overwrite-images-option)的更多信息。

虽然这不是必需的，但是在使用上述两种方法之一上传时，您可以为该特定上传指定作业选项 — 例如，计划定期上传、在上传时设置裁剪选项等。 这些功能对于某些工作流可能很有价值，因此考虑是否适合您的工作流。

了解有关[作业选项](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/upload-publish/uploading-files.html#upload-options)的更多信息。

上传是任何工作流中第一个必需的步骤，因为Dynamic Media Classic无法处理其系统中尚未包含的任何内容。 在上传期间，系统会在后台向集中的Dynamic Media Classic数据库注册每个上传的资产，分配一个ID，并将其复制到存储中。 此外，系统还会将图像文件转换为允许动态调整大小和缩放的格式，并将视频文件转换为MP4 Web友好格式。

### 概念：以下是将图像上传到Dynamic Media Classic时图像会发生的情况

将任何类型的图像上传到Dynamic Media Classic时，该图像会转换为称为Pyramid TIFF或P-TIFF的主控图像格式。 P-TIFF与分层TIFF位图图像的格式类似，不同之处在于，文件包含同一图像的多种大小（分辨率），而不是不同的图层。

![图像](assets/main-workflow/pyramid-p-tiff.png)

转换图像后，Dynamic Media Classic会为整幅图像拍摄“快照”，将其缩放一半并保存，再次缩放一半并保存，等等，直到图像填充到原始大小的偶数倍为止。 例如，一个2000像素的P-TIFF在同一文件中将具有1000、500、250和125像素大小（及更小）。 P-TIFF文件是Dynamic Media Classic中称为“主控图像”的格式。

当您请求特定大小的图像时，创建P-TIFF可让Dynamic Media Classic的图像服务器快速找到下一个较大的图像并缩小其大小。 例如，如果您上传2000像素的图像并请求100像素的图像，Dynamic Media Classic将找到125像素的版本并将其缩小到100像素，而不是从2000像素缩放到100像素。 这使得手术很快。 此外，在缩放图像时，这允许缩放查看器仅请求缩放的图像的图块，而不是整个全分辨率图像。 这就是主控图像格式（P-TIFF文件）支持动态大小调整和缩放的方式。

同样，您也可以将主控源视频上传到Dynamic Media Classic，上传Dynamic Media Classic后，可以自动调整其大小并将其转换为MP4 Web友好格式。

### 确定您上传图像的最佳大小的经验规则

**以您需要的最大大小上传图像。**

- 如果需要缩放，请上传最长尺寸为1500-2500像素的高分辨率图像。 考虑您想要提供多少详细信息、源图像的质量以及所显示产品的大小。 例如，上传一个1000像素的小环图像，而上传一个3000像素的整个房间图像。
- 如果您不需要缩放，请以可看到的确切大小上传它。 例如，如果要在页面上放置徽标或闪屏/横幅图像，请完全按1:1大小上传这些图像，然后完全按该大小调用它们。

**在将图像上传到Dynamic Media Classic之前，切勿对图像进行上采样或吹扫。** 例如，不要对较小的图像进行上采样，使其变为2000像素的图像。看起来不妙。 在上传之前，使图像尽可能接近完美。

**缩放没有最小大小，但默认情况下，查看器不会缩放到100%以上。** 如果图像太小，它将不会完全缩放，或者只会缩放很小的量，以防止图像看起来不好。

**虽然图像大小没有最低限制，但我们不建议上传巨型图像。** 一幅巨图像可以视为4000+像素。上传此大小的图像可能会显示潜在的缺陷，如图像中的灰尘颗粒或毛发颗粒。 此类图像在Dynamic Media Classic服务器上还会占用更多空间，这可能会导致您超出合同规定的存储限制。

了解有关[上传文件](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/upload-publish/uploading-files.html#uploading-your-files)的更多信息。

## 步骤2:创作（和发布）

创建和上传内容后，您将通过执行一个或多个子工作流从上传的资产创作新的富媒体资产。 这包括所有不同类型的集合 — 图像、色板、旋转和混合媒体集以及模板。 它还包括视频。 我们稍后将详细介绍每种类型的图像收集集和视频富媒体。 但是，在几乎所有情况下，您都需要首先选择一个或多个资产（或未选择任何资产），然后选择要构建的资产类型。 例如，您可以选择一个主图像和该图像的几个视图，然后选择生成一个图像集，该图像集是同一产品的替代视图集合。

>[!IMPORTANT]
>
>确保所有资产都标记为发布。 默认情况下，所有资产都会在上传时自动标记为发布，但根据您上传的内容新创作的任何资产也需要标记为发布。

构建新资产后，您将运行发布作业。 您可以手动执行该操作，或计划自动运行的发布作业。 发布会将所有内容从专用的Dynamic Media Classic球体复制到公共的发布服务器球体。 Dynamic Media发布作业的产品是每个已发布资产的唯一URL。

您发布到的服务器取决于内容类型和工作流。 例如，所有图像都转到图像服务器，然后流视频到FMS服务器。 为方便起见，我们将将“发布”称为单个服务器的单个事件。

发布时会发布所有标记为发布的内容，而不仅仅是您的内容。 单个管理员通常代表每个人发布，而不是代表运行发布的个人用户发布。 管理员可以根据需要发布，或设置每日、每周甚至每10分钟一次的定期作业，该作业将自动发布。 按适合您业务的计划发布。

>[!TIP]
>
>自动执行发布作业，并计划“完整发布”以在每天凌晨12:00或深夜的任何时间运行。

### 概念：了解Dynamic Media Classic URL

Dynamic Media Classic工作流的最终产品是指向资产（无论是图像集还是自适应视频集）的URL。 这些URL是可预测的，并且遵循相同的模式。 对于图像，每个图像都从P-TIFF主控图像中生成。

以下是图像URL的语法，其中包含几个示例：

![图像](assets/main-workflow/dmc-url.jpg)

在URL中，问号左侧的所有内容都是指向特定图像的虚拟路径。 问号右侧的所有内容都是图像服务器修饰符，它指明了如何处理图像。 当您有多个修饰符时，它们会用与号分隔。

在第一个示例中，图像“Backpack_A”的虚拟路径为`http://sample.scene7.com/is/image/s7train/BackpackA`。 图像服务器修饰符将图像的大小调整为250像素（从wid=250）的宽度，并使用Lanczos插值算法对图像进行重新采样，该算法会随着图像大小的调整而锐化（从resMode=sharp2）。

第二个示例将所谓的“图像预设”应用到同一Backpack_A图像，如$!_template300$。 表达式两侧的$符号表示图像预设（一组打包的图像修饰符）正被应用到图像。

了解Dynamic Media Classic URL如何组合在一起后，您便可以了解如何以编程方式更改它们，以及如何将它们更深入地集成到您的网站和后端系统中。

### 概念：了解缓存延迟

新上传和发布的资产将立即显示，而更新的资产可能会受到10小时缓存延迟的影响。 默认情况下，所有已发布的资产在过期前至少有10小时。 我们说的最少，因为每次查看图像时，它都会启动一个时钟，该时钟直到没有人查看该图像的10小时后才会过期。 这10小时的时段是资产的“生存时间”。 该资产的缓存过期后，即可交付更新版本。

除非发生错误，并且图像/资产与之前发布的版本同名，否则通常不会出现此问题，但图像存在问题。 例如，您意外上传了低分辨率版本，或者您的艺术指导未批准该图像。 在这种情况下，您需要重新调用原始图像，并使用相同的资产ID将其替换为新版本。

了解如何[手动清除需要更新的URL的缓存](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/dynamicmedia/invalidate-cdn-cache-dynamic-media.html?lang=en)。

>[!TIP]
>
>为避免缓存延迟问题，请始终继续工作 — 晚上、一天、两周等。 在向公众发布之前，应及时为内部各方提供QA/接受以证明您的工作。 即使在前一天晚上工作，您也可以在当天晚上进行更改并重新发布。 到早上，10小时已过，缓存会更新为正确的图像。

- 了解有关[创建发布作业](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/upload-publish/publishing-files.html#creating-a-publish-job)的更多信息。
- 了解有关[Publishing](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/upload-publish/publishing-files.html)的更多信息。

## 步骤3:交付

请记住，Dynamic Media Classic工作流的最终产品是指向资产的URL。 该URL可能指向单个图像、图像集、旋转集或其他一些图像集集合或视频。 您需要获取该URL并对其执行一些操作，例如编辑HTML，以便`<IMG>`标记指向Dynamic Media Classic图像，而不是指向来自您当前网站的图像。

在“交付”步骤中，您必须将这些URL集成到您的网站、移动设备应用程序、电子邮件促销活动，或您要在其上显示资产的任何其他数字接触点中。

将图像的Dynamic Media Classic URL集成到网站的示例：

![图像](assets/main-workflow/example-url-1.png)

红色中的URL是唯一特定于Dynamic Media Classic的元素。

您的IT团队或集成合作伙伴可以牵头编写和更改代码，以将Dynamic Media Classic URL集成到您的网站中。 Adobe有一个咨询团队，可以通过提供技术、创意或一般指导来帮助您完成这项工作。

对于诸如缩放查看器或将缩放与替代视图相结合的查看器等更复杂的解决方案，URL通常指向由Dynamic Media Classic托管的查看器，而且该URL中也是对资产ID的引用。

将在新弹出窗口中的查看器中打开图像集的链接示例（以红色表示）：

![图像](assets/main-workflow/example-url-2.png)

>[!IMPORTANT]
>
>您需要将Dynamic Media Classic URL集成到您的网站、移动设备应用程序、电子邮件和其他数字接触点中 — Dynamic Media Classic无法为您执行此操作！

## 预览资产

您可能希望预览您上传或正在创建或编辑的资产，以确保在客户查看资产时这些资产会按您希望的方式显示。 您可以通过以下方式访问“预览”窗口：单击任意&#x200B;**预览**&#x200B;按钮（位于资产缩略图上、**浏览/构建面板**&#x200B;顶部），或转到&#x200B;**文件>预览**。 在浏览器窗口中，它将预览面板中当前位于哪个资产，无论该资产是图像、视频还是像图像集一样构建的资产。

### 动态大小预览（图像预设）

使用&#x200B;**大小**&#x200B;预览，可以以多种大小预览图像。 这会加载可用图像预设的列表。 我们稍后将讨论图像预设，但会将它们视为“方法”，用于以指定大小加载图像，并具有特定锐化和图像质量。

### 缩放预览

您还可以使用&#x200B;**缩放**&#x200B;选项在许多预建缩放预设中的某个预设中预览图像，这些预设基于包含的不同缩放查看器。

了解有关[预览资产](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/managing-assets/previewing-asset.html)的更多信息。
