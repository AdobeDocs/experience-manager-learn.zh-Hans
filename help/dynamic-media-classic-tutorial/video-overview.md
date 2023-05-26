---
title: 视频概述
description: Dynamic Media Classic附带上传时视频的自动转换、将视频流传输到桌面和移动设备以及针对基于设备和带宽的播放进行优化的自适应视频集。 了解有关Dynamic Media Classic中视频的更多信息，并获取有关视频概念和术语的初级课程。 然后，深入了解如何上传和编码视频、选择要上传的视频预设、添加或编辑视频预设、在视频查看器中预览视频、将视频部署到Web和移动站点、向视频添加字幕和章节标记以及将视频查看器发布到桌面和移动用户。
feature: Dynamic Media Classic, Video Profiles, Viewer Presets
doc-type: tutorial
topics: development, authoring, configuring, videos, video-profiles
audience: all
activity: use
topic: Content Management
role: User
level: Beginner
exl-id: dfbf316f-3922-4bc7-b3f3-2a5bbdeb7063
source-git-commit: f0c6e6cd09c1a2944de667d9f14a2d87d3e2fe1d
workflow-type: tm+mt
source-wordcount: '6118'
ht-degree: 0%

---

# 视频概述 {#video-overview}

Dynamic Media Classic附带上传时视频的自动转换、将视频流传输到桌面和移动设备以及针对基于设备和带宽的播放进行优化的自适应视频集。 关于视频最重要的事情之一就是工作流程非常简单 — 它的设计让任何人都可以使用它，即使他们不熟悉视频技术。

在本教程的此部分结束时，您将知道如何：

- 上传视频并编码（转码）为不同的大小和格式
- 在可供上传的视频预设中进行选择
- 添加或编辑视频编码预设
- 在视频查看器中预览视频
- 将视频部署到Web和移动站点
- 向视频添加字幕和章节标记
- 自定义和发布适用于桌面和移动设备用户的视频查看器

>[!NOTE]
>
>本章中的所有URL仅用于说明目的；它们不是实时链接。

## Dynamic Media Classic视频概述

让我们首先更好地了解使用Dynamic Media Classic制作视频的可能性。

### 特性和功能

Dynamic Media Classic视频平台提供了视频解决方案的所有部分 — 视频的上传、转换和管理；向视频添加字幕和章节标记的功能；以及使用预设以便轻松播放的功能。

它可让您轻松发布高质量的自适应视频，以便在多个屏幕(包括台式机、iOS、Android™、BlackBerry®和Windows移动设备)上实现流式传输。 自适应视频集对使用不同比特率和格式（如400 kbps、800 kbps和1000 kbps）编码的相同视频的版本进行分组。 台式计算机或移动设备检测可用带宽。

此外，如果桌面或移动设备上的网络状况发生变化，则视频质量会自动进行动态切换。 此外，如果客户在桌面上进入全屏模式，自适应视频集将使用更好的分辨率进行响应，从而改善客户的观看体验。 通过使用自适应视频集，客户可以在多个屏幕和设备上播放Dynamic Media Classic视频，您可以获得最佳播放体验。

### 视频管理

处理视频可能比处理静止数字图像更复杂。 使用视频，您可以处理多种格式和标准，以及受众能否播放剪辑的不确定性。 Dynamic Media Classic使视频处理更容易，提供了“隐藏的”许多强大工具，但消除了使用这些工具的复杂性。

Dynamic Media Classic可以识别多种可用的源格式并且可以与多种不同的源格式一起使用。 但是，阅读视频只是工作的一部分，您还必须将视频转换为Web友好格式。 Dynamic Media Classic通过允许您将视频转换为H.264视频来解决此问题。

使用许多可用的专业和爱好者工具自行转换视频可能会变得复杂。 Dynamic Media Classic通过提供针对不同质量设置进行了优化的简单预设，使其保持简单。 但是，如果您希望进行更多自定义，也可以创建自己的预设。

如果您拥有大量视频，那么在Dynamic Media Classic中能够管理您的所有资源以及图像和其他媒体将非常感谢。 您可以通过强大的XMP元数据支持来组织、编录和搜索资源，包括视频资源。

### 视频播放

与将视频转换为对Web友好且易于访问的问题相似，实施视频并将其部署到网站也存在问题。 选择购买播放器还是构建自己的播放器，使其兼容各种设备和屏幕，然后维护您的播放器可能是一项全职工作。

同样地，Dynamic Media Classic的方法是允许您选择符合您需求的预设和查看器。 您有许多不同的查看器选择，并且有许多可用的预设库。

您可以轻松地将视频交付到Web和移动设备，因为Dynamic Media Classic支持HTML5视频，这意味着您可以定位运行各种浏览器的用户以及Android™和iOS平台用户。 流式视频允许流畅播放较长内容或高清内容，而渐进式HTML5视频则具有针对小屏幕而优化的预设。

视频的查看器预设可部分配置，具体取决于查看器类型。

与所有查看器一样，集成是通过每个查看器或视频的一个Dynamic Media Classic URL实现的。

>[!NOTE]
>
>作为最佳实践，请使用Dynamic Media ClassicHTML5视频查看器。 HTML5视频查看器中使用的预设是可靠的视频播放器。 通过将使用HTML5和CSS设计播放组件的功能整合到单个播放器中、嵌入播放器并根据浏览器的功能使用自适应和渐进式流，您可以将富媒体内容的范围扩展到桌面、平板电脑和移动设备用户，并确保简化的视频体验。

最后一条关于Dynamic Media Classic视频的注释可能适用于某些客户：并非所有公司都为其帐户启用了自动转换、流或视频预设。 如果由于某种原因无法访问流视频的URL，则这可能是原因。 您可以上传和发布逐步下载的视频，并访问所有视频查看器。 但是，要利用完整的Dynamic Media Classic视频功能，请联系您的客户经理或销售经理以启用这些功能。

详细了解 [Dynamic Media Classic中的视频](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/video/quick-start-video.html).

## Video 101

### 基本视频概念和术语

在开始之前，让我们讨论一些您应该熟悉的术语以使用视频。 这些概念并不是特定于Dynamic Media Classic的，如果您要管理专业网站的视频，最好进一步了解该主题。 我们建议在本节末尾提供一些资源。

- **编码/转码。** 编码是应用视频压缩以将原始未压缩视频数据转换为更易于使用的格式的过程。 转码与此类似，是指从一种编码方法转换为另一种编码方法。

   - 使用视频编辑软件创建的主控视频文件通常太大，并且格式不正确，无法传送到在线目标。 它们通常经过编码，用于在桌面上快速播放和进行编辑，但不用于通过Web交付。
   - 为了将数字视频转换为在不同屏幕上播放的正确格式和规范，视频文件被转码为更小、更高效的文件大小，最适合于传送到Web和移动设备。

- **视频压缩。** 减少用于表示数字视频图像的数据量，是空间图像压缩和时间运动补偿的组合。

   - 大多数压缩技术都是有损的，这意味着它们会丢弃数据以实现更小的大小。
   - 例如，DV视频压缩得相对较少，可让您轻松编辑源素材，但它的体积太大而无法在Web上使用，甚至无法放在DVD上。

- **文件格式。** 格式是一个与ZIP文件类似的容器，它确定文件在视频文件中的组织方式，但通常不确定文件的编码方式。

   - 源视频的常见文件格式包括Windows Media (WMV)、QuickTime (MOV)、Microsoft®AVI和MPEG等。 Dynamic Media Classic发布的格式为MP4。
   - 视频文件通常包含多个相互关联并同步的轨道 — 一个视频轨道（没有音频）和一个或多个音频轨道（没有视频）。
   - 视频文件格式确定如何组织这些不同的数据跟踪和元数据。

- **编解码.** 视频编解码器描述了使用压缩对视频进行编码的算法。 音频也通过音频编解码器编码。

   - 编解码器可最大限度地减少播放视频所需的信息量。 存储的信息不是关于每个单独帧的信息，而是关于一个帧与下一个帧之间的差异的信息。
   - 由于大多数视频在帧间变化不大，因此编解码器允许较高的压缩率，从而缩小文件大小。
   - 视频播放器根据其编解码器解码视频，然后在屏幕上显示一系列图像或帧。
   - 常见的视频编解码器包括H.264、On2 VP6和H.263。

![图像](assets/video-overview/bird-video.png)

- **解决方法.** 视频的高度和宽度（像素）。

   - 源视频的大小由摄像头和编辑软件的输出决定。 高清摄像头可创建高分辨率1920 x 1080视频，但为了在Web上顺利回放，您可以缩减像素取样（调整大小）至更小的分辨率，例如1280 x 720、640 x 480或更小。
   - 分辨率直接影响播放该视频所需的文件大小和带宽。

- **显示宽高比。** 视频宽度与视频高度的比率。 当视频的长宽比与播放器的长宽比不匹配时，您可能会看到“黑条”或空格。 用于显示视频的两个常见宽高比是：

   - 4:3 (1.33:1). 用于几乎所有标准清晰度电视广播内容。
   - 16:9 (1.78:1). 用于几乎所有宽屏、高清电视内容(HDTV)和电影。

- **比特率/数据速率。** 经过编码以构成视频播放一秒的数据量（以千位/秒为单位）。

   - 通常，比特率越低，Web所需的比特率就越高，因为它可以更快地下载。 但是，这也意味着由于压缩损失，质量较低。
   - 一个好的编解码器应该平衡低比特率和好的质量。

- **帧速率（每秒帧数或FPS）。** 视频的每秒帧数，即静态图像数。 通常，北美电视(NTSC)以29.97 FPS播放；欧洲和亚洲电视(PAL)以25 FPS播放；电影（模拟和数字）通常以24 (23.976) FPS播放。

   - 为了让事情更加混乱，还有渐进和交错的画面。 每个渐进式帧包含整个图像帧，而交错式帧则包含图像帧中每隔一行的像素。 然后，这些帧会快速回放，看起来混合在一起。 胶片使用渐进式扫描方法，而数字视频通常是交错的。
   - 通常，无论源视频是否交错，Dynamic Media Classic都会在转换后的视频中保留扫描方法。
   - 流式/渐进式投放。 视频流是指以连续流的形式发送媒体，在媒体到达时可以播放，而渐进式下载的视频则与从服务器下载的任何其他文件一样下载并在浏览器中缓存到本地。

希望本入门课程有助于您了解使用Dynamic Media Classic视频时涉及的各个选项。

## 视频工作流

在Dynamic Media Classic中使用视频时，遵循与处理图像类似的基本工作流程。

![图像](assets/video-overview/video-overview-2.png)

1. 首先，将视频文件上传到Dynamic Media Classic。 为此，请打开 **“工具”菜单** 位于Dynamic Media Classic扩展面板底部，然后选择 **上传至Dynamic Media Classic >文件到文件夹名称**，或 **上传至Dynamic Media Classic >文件夹至文件夹名称**. “文件夹名称”是指您当前使用该扩展浏览的任何文件夹。 视频文件可能很大，因此我们建议使用FTP上传大文件。 在上传过程中，选择一个或多个用于编码视频的视频预设。 上传视频时可以将视频转码为MP4视频。 有关使用和创建编码预设的更多信息，请参阅下面的视频预设主题。 了解 [上传和编码视频](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/video/uploading-encoding-videos.html).
2. 选择或修改视频查看器预设并预览视频。 您可以选择预建的查看器预设，也可以自定义您自己的查看器预设。 如果您定位的是移动用户，则无需在此处执行任何操作，因为移动平台不需要查看器或预设。 详细了解 [在视频查看器中预览视频](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/video/previewing-videos-video-viewer.html) 和 [添加或编辑视频查看器预设](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/video/previewing-videos-video-viewer.html#adding-or-editing-a-video-viewer-preset).
3. 运行视频发布、获取URL并集成。 视频工作流的此步骤与图像工作流的主要区别在于，您运行的是特殊的视频发布，而不是（或可能和）标准的图像服务发布。 桌面上的视频查看器集成的工作方式与图像查看器集成完全相同，不过对于移动设备而言，它甚至更简单 — 您只需要视频本身的URL。

### 关于转码

转码之前被定义为从一种编码方法转换为另一种编码方法的过程。 对于Dynamic Media Classic，此过程会将源视频从当前格式转换为MP4格式。 在桌面浏览器或移动设备上显示视频之前，必须执行此操作。

Dynamic Media Classic可以为您处理所有转码工作，这是一个巨大的优势。 您可以自行转码视频并上传已转换为MP4的文件，但这是一个复杂的过程，需要复杂的软件。 除非您知道自己在做什么，否则通常在第一次尝试时不会获得良好的结果。

Dynamic Media Classic不仅可以为您转换文件，而且通过提供易于使用的预设来简化转换过程。 您确实不需要了解此流程的技术方面 — 您应知道的只是您希望从系统中获得的最终大小以及最终用户拥有的带宽情况。

虽然预置预设很方便且可满足大多数需求，但有时您需要一些更自定义的内容。 在这种情况下，您可以创建自己的编码预设。 在Dynamic Media Classic中，编码预设称为视频预设。 本章稍后将对此进行说明。

### 关于流

另一个值得注意的主要功能是视频流，这是Dynamic Media Classic视频平台的标准功能。 流媒体由最终用户持续接收并在交付时呈现给最终用户。 这很重要，而且出于以下几个原因，它是可取的。

与渐进式下载相比，流媒体通常需要更少的带宽，因为仅交付观看的视频部分。 Dynamic Media Classic视频流服务器和查看器使用自动带宽检测，为用户的Internet连接提供尽可能最佳的流。

使用流式传输时，视频开始播放的时间比使用其他方法时早。 它还提高了网络资源的使用效率，因为只有观看过的视频部分会发送到客户端。

另一种交付方法是渐进式下载。 与流视频相比，渐进式下载只有一个持续优势，即无需流服务器即可交付视频。 这当然是Dynamic Media Classic的优势所在 — Dynamic Media Classic在平台中内置了流服务器，因此您无需维护此专用硬件带来的麻烦或额外成本。

渐进式下载视频可以从任何普通Web服务器提供。 虽然这既方便又可能经济实惠，但请记住，渐进式下载的搜寻和导航功能有限，用户可以访问您的内容并重新调整其用途。 在某些情况下（例如在严格的网络防火墙后播放），可以阻止流式传输；在这些情况下，可以希望回滚到渐进式传输。

渐进式下载非常适合流量要求较低的业余爱好者或网站；如果他们不介意将内容缓存在用户计算机上；如果他们只需要交付较短的视频（少于10分钟）；或者他们的访客由于某种原因无法接收流视频。

如果您需要高级功能和控制视频交付，并且/或者您需要向更大的受众显示视频（例如，多个100位同时观看者）、跟踪和报告使用情况或查看统计数据，或者希望为观看者提供最佳交互式播放体验，则需要流式传输视频。

最后，如果您担心保护媒体的知识产权或版权管理问题，流式传输可提供更安全的视频交付，因为媒体在流式传输时不会保存到客户端的缓存中。

## 视频预设

上传视频时，您可以从一个或多个预设中进行选择，这些预设包含用于通过编码将主控视频转换为Web友好格式的设置。 视频预设有两种风格：自适应视频预设和单个编码预设。

参见 [可用的视频预设](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/setup/application-setup.html#video-presets-for-encoding-video-files).

自适应视频预设默认处于激活状态，这意味着它们可用于编码。 如果您希望使用单个编码预设，管理员需要激活该预设，它才能显示在视频预设列表中。

了解如何 [激活或停用视频预设](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/video/uploading-encoding-videos.html#activating-or-deactivating-video-encoding-presets).

您可以从Dynamic Media Classic附带的许多预建预设中选择一个，也可以创建自己的预设；但是，默认情况下不会选择任何预设进行上传。 换句话说， **如果您在上传时未选择视频预设，则您的视频将不会被转换，并且可能会无法发布**. 不过，您可以自己离线转换视频，也可以上传并发布。 仅当您希望Dynamic Media Classic为您进行转换时，才需要视频预设。

上传时，您可以通过选择来选择视频预设 **视频选项** 在“作业选项”面板中。 然后，选择您要为“计算机”、“移动设备”还是“平板电脑”编码。

- 计算机用于桌面。 在这里，您通常会发现使用更多带宽的较大预设（例如高清）。
- 移动设备和平板电脑为iPhone和Android™智能手机等设备创建MP4视频。 移动设备与平板电脑的唯一区别在于，平板电脑预设通常具有较大的带宽，因为它们基于WiFi使用。 移动预设针对较慢的3G使用率进行了优化。

### 选择预设之前需要问自己的问题

在选择预设时，您应该了解受众和源素材。 您对客户了解多少？ 他们怎么看视频 — 用电脑显示器还是移动设备？

您的视频的分辨率是多少？ 如果您选择的预设大于原始预设，则可能会获得模糊/像素化的视频。 如果您的视频大于预设，但不要选择大于源视频的预设，这没有关系。

其宽高比是多少？ 如果您在转换后的视频周围看到黑色条，则表明您选择了错误的纵横比。 Dynamic Media Classic无法自动检测这些设置，因为它必须先检查文件，然后才能上传。

### 视频选项划分

视频预设通过指定这些设置来确定视频的编码方式。 如果您不熟悉这些术语，请查看上面的“基本视频概念和术语”主题。

![图像](assets/video-overview/video-overview-3.jpg)

- **宽高比.** 标准4:3或宽屏16:9。
- **大小.** 这与显示分辨率相同，并以像素为单位测量。 这与长宽比有关。 在16:9的比率下，视频为432 x 240像素，而在4:3时，视频为320 x 240像素。
- **FPS.** 标准帧速率为每秒30帧、每秒25帧或每秒24帧(fps)，具体取决于视频标准 — NTSC、PAL或Film。 此设置无关紧要，因为Dynamic Media Classic始终使用与源视频相同的帧速率。
- **格式.** 这是MP4。
- **带宽。** 这是目标用户所需的连接速度。 它们连接互联网的速度是快还是慢？ 他们通常使用台式计算机还是移动设备？ 这也与分辨率（大小）有关，因为视频越大，它需要的带宽就越多。

### 确定视频的数据速率或“比特率”

计算视频的比特率是向Web提供视频服务时了解最少的一个因素，但可能是最重要的因素，因为它直接影响用户体验。 如果比特率设置得太高，则视频质量会很高，但性能会很差。 Internet连接速度较慢的用户被迫等待视频在播放时不断暂停。 但是，如果设置过低，质量就会受损。 在视频预设中，Dynamic Media Classic会根据您的目标带宽建议一系列数据。 这是一个很好的起点。

但是，如果您想自己算出来，则需要比特率计算器。 视频专业人员和爱好者通常使用此工具来估计给定流或媒体（如DVD）中的数据大小。

## 创建自定义视频预设

有时，您可能会发现需要与内置编码视频预设的设置不匹配的特殊视频预设。 如果您有特定大小的自定义视频，例如从3D动画软件创建的视频或已从原始大小裁剪的视频，则可能会发生这种情况。 您可能希望尝试使用不同的带宽设置，以提供更高或更低质量的视频。 无论哪种情况，请创建自定义的单一编码视频预设。

### 视频预设工作流程

1. 视频预设位于 **设置>应用程序设置>视频预设**. 您将在此处找到公司可用的所有编码预设的列表。

   - 每个流视频帐户都有数十个开箱即用的预设，如果您创建自己的自定义预设，您也会在此处看到它们。
   - 您可以使用下拉菜单按类型进行筛选。 预设分为“计算机”、“移动设备”和“平板电脑”。
      ![图像](assets/video-overview/video-overview-4.jpg)

2. “活动”列允许您选择是要在上载时显示所有预设，还是只显示您选择的预设。 如果您在美国，则可能需要取消选中European PAL预设；如果您在英国/欧洲、中东和非洲地区，则可能需要取消选中NTSC预设。
3. 单击 **添加** 按钮以创建自定义预设。 这将打开添加视频预设面板。 此处的过程与创建图像预设类似。
4. 首先，给它一个 **预设名称** 显示在预设列表中。 在上面的示例中，此预设用于屏幕捕获教程视频。
5. 此 **描述** 是可选的，但它会向您的用户提供描述此预设用途的工具提示。
6. 此 **编码文件后缀** 会附加到您在此处创建的视频的名称末尾。 请记住，您将拥有主控视频以及此编码视频(主控的派生项)，并且Dynamic Media Classic中的任意两个资源不能具有相同的资源ID。
7. **播放设备** 您可以选择所需的视频文件格式（计算机、移动设备或Tablet）。 请记住，Mobile和Tablet生成相同的MP4格式。 Dynamic Media Classic只需要知道预设的放置类别；但是，理论上的区别在于，平板电脑预设通常用于更快的互联网连接，因为所有预设都支持WiFi。
8. **目标数据速率** 是您必须自行解决的问题，但您可以在下图中看到建议的范围。 或者，也可以将滑块拖动到近似的目标带宽。 要获得更精确的图形，请使用比特率计算器。 这其中涉及一些试错。

   ![图像](assets/video-overview/video-overview-5.jpg)

9. 设置源文件的 **宽高比**. 此设置直接绑定到下面的大小。 如果您选择 _自定义_，您必须手动输入宽度和高度。
10. 如果选择纵横比，则为设置一个值 **分辨率大小** 和Dynamic Media Classic会自动填充其他值。 但是，对于自定义纵横比，请填充这两个值。 您的规模应与数据速率一致。 如果设置低数据率和大数据，则质量可能会很差。
11. 单击 **保存** 以保存预设。 与其他所有预设不同，您现在不需要发布，因为预设仅用于上传文件。 稍后，您必须发布经过编码的视频，但预设仅供Dynamic Media Classic内部使用。
12. 要验证您的视频预设是否在上传列表中，请转到 **上传**. 选择 **作业选项** 并展开 **视频选项**. 您的预设会列在您选择的播放设备（“计算机”、“移动设备”或“平板电脑”）的类别中。

详细了解 [添加或编辑视频预设](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/video/uploading-encoding-videos.html#adding-or-editing-a-video-encoding-preset).

## 在视频中添加字幕

有时候，向视频添加字幕会很有用 — 例如，当您需要以多种语言向查看者提供视频，但不希望以另一种语言对音频进行重复或者以单独的语言再次录制视频时。 此外，为听力受损且使用隐藏式字幕的用户提供更好的辅助功能。 通过Dynamic Media Classic，可以轻松地将字幕添加到视频中。

了解如何 [向视频添加字幕](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/video/adding-captions-video.html).

## 向视频添加章节标记

对于长格式视频，观看者可能会喜欢使用章节标记导航视频所提供的能力和便利。 Dynamic Media Classic提供向视频轻松添加章节标记的功能。

了解如何 [向视频添加章节标记](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/video/adding-chapter-markers-video.html).

## 视频实施主题

### 发布并复制URL

Dynamic Media Classic工作流中的最后一个步骤是发布视频内容。 但是，视频有其自己的发布作业，称为“视频服务器发布”，位于“高级”下。

![图像](assets/video-overview/video-overview-6.jpg)

了解如何 [发布您的视频](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/video/deploying-video-websites-mobile-sites.html#publishing-video).

运行视频发布后，您便能够获取URL以在Web浏览器中访问视频和任何现成的Dynamic Media Classic Viewer预设。 但是，如果您自定义或创建自己的视频查看器预设，则需要运行单独的图像服务器发布。

- 了解如何 [将URL链接到移动设备网站或网站](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/video/deploying-video-websites-mobile-sites.html#linking-a-video-url-to-a-mobile-site-or-a-website).
- 了解如何 [在网页上嵌入视频查看器](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/video/deploying-video-websites-mobile-sites.html#embedding-the-video-viewer-on-a-web-page).

您也可以使用第三方或自定义的视频播放器来部署视频。

了解如何 [使用第三方视频播放器部署视频](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/video/deploying-video-websites-mobile-sites.html#deploying-video-using-a-third-party-video-player).

此外，如果您还想使用视频缩略图（从视频中提取的图像），则需要运行图像服务器发布。 这是因为视频的缩略图图像位于图像服务器上，而视频本身位于视频服务器上。 视频缩略图可用于视频搜索结果、视频播放列表，并可用作视频播放之前显示在视频查看器中的初始“海报帧”。

详细了解 [使用视频缩略图](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/video/deploying-video-websites-mobile-sites.html#working-with-video-thumbnails).

### 选择和自定义查看器预设

选择和自定义查看器预设的过程与图像的过程相同。 您可以创建预设或修改现有预设，然后使用新名称保存、进行编辑并运行图像服务发布。 所有查看器预设都发布到图像服务器，而不仅仅是图像的预设，因此您必须运行图像发布才能查看新预设或修改的预设。

>[!TIP]
>
>在视频服务器发布后运行图像服务发布，以发布与视频关联的任何缩略图。

## 视频搜索引擎优化

搜索引擎优化(SEO)是指提高网站或网页在搜索引擎中的可见性的过程。 尽管搜索引擎擅长收集有关文本内容的信息，但除非向搜索引擎提供视频信息，否则搜索引擎无法充分获取有关视频的信息。 通过使用Dynamic Media Classic视频SEO，您可以使用元数据为搜索引擎提供视频描述。 视频SEO功能允许您创建视频站点地图和媒体RSS (mRSS)源。

- **视频站点地图**. 告知Google视频内容在网站上的确切位置和内容。 因此，可以在Google上完全搜索视频。 例如，视频站点地图可以指定视频的运行时间和类别。
- **mRSS馈送**. 内容发布者用来将媒体文件馈送到Yahoo！ 视频搜索。 Google支持视频站点地图和媒体RSS (mRSS)馈送协议，以便向搜索引擎提交信息。

创建视频站点地图和mRSS馈送时，需要决定视频文件中要包含的元数据字段。 这样，您就可以将视频描述给搜索引擎，以便搜索引擎可以更准确地将流量引导至您网站上的视频。

创建站点地图或信息源后，您可以让Dynamic Media Classic自动发布站点地图或信息源，手动发布站点地图，或仅生成一个可在以后编辑的文件。 此外，Dynamic Media Classic可以每天自动生成并发布此文件。

在该过程结束时，您将向搜索引擎提交文件或URL。 这项任务是在Dynamic Media Classic之外完成的；但我们会在下面简要讨论。

### Sitemap/mRSS文件的要求

要让Google和其他搜索引擎不拒绝您的文件，它们必须采用正确的格式并包含特定信息。 Dynamic Media Classic会生成一个格式正确的文件；但是，如果某些视频没有相应的信息，则不会包含在该文件中。

必填字段包括登陆页面（提供视频的页面的URL，而不是视频本身的URL）、标题和描述。 每个视频都必须有一个这些项目对应的条目，否则不会包含在生成的文件中。 可选字段包括标记和类别。

还有其他两个必填字段 — 内容URL（视频资源本身的URL）和缩略图（视频缩略图图像的URL），但Dynamic Media Classic会自动为您填充这些值。

推荐的工作流程是在使用XMP元数据上传之前将此数据嵌入到视频中，Dynamic Media Classic将在上传时提取此数据。 您将使用诸如Adobe Bridge之类的应用程序(此应用程序包含在所有Adobe Creative Cloud应用程序中)将数据填充到标准元数据字段中。

通过使用此方法，您无需使用Dynamic Media Classic手动输入此数据。 但是，您还可以在Dynamic Media Classic中使用元数据预设，作为每次输入相同数据的快速方法。

有关该主题的详细信息，请参阅 [查看、添加和导出元数据](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/managing-assets/viewing-adding-exporting-metadata.html).

![图像](assets/video-overview/video-overview-7.jpg)

填充元数据后，您便能够在该视频资产的详细信息视图中看到该元数据。 关键字也可能存在，但这些关键字位于“关键字”选项卡下。

- 详细了解 [添加关键字](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/managing-assets/viewing-adding-exporting-metadata.html#add-or-edit-keywords).
- 详细了解 [视频SEO](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/setup/video-seo-search-engine-optimization.html).
- 了解 [视频SEO设置](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/setup/video-seo-search-engine-optimization.html#choosing-video-seo-settings).

#### 设置视频SEO

设置视频SEO时，首先需要选择所需的格式类型、生成方法以及文件中应包含的元数据字段。

1. 转到 **设置>应用程序设置>视频SEO >设置**.
2. 在 **生成模式** 菜单，选择文件格式。 默认值为“关闭”，因此要启用它，请选择“视频站点地图”、“mRSS”或“两者”。
3. 选择是自动生成还是手动生成。 为了简化，我们建议您将它设置为 **自动模式**. 如果选择“自动”，则还应设置 **标记为发布** 选项，否则文件将无法上线。 Sitemap和RSS文件是XML文档的类型，必须像任何其他资源一样发布。 如果现在还没有准备好所有信息，或者只想进行一次性生成，请使用手动模式之一。
4. 填充文件中使用的元数据标记。 此步骤不是可选的。 您至少必须包含三个标有星号(\*)的字段： **登陆页面** ， **标题**、和 **描述**. 要将您的元数据用于这些任务，请将右侧元数据面板中的字段拖放到表单上的相应字段中。 Dynamic Media Classic将自动使用每个视频中的实际数据填充占位符字段。 您不必使用元数据字段。 您可以在此处键入一些静态文本，但每个视频将显示相同的文本。
5. 在三个必填字段中输入信息后，Dynamic Media Classic将启用 **保存** 和 **保存并生成** 按钮。 单击一个以保存您的设置。 使用 **保存** 如果您处于自动模式，并且希望让Dynamic Media Classic稍后生成文件。 使用 **保存并生成** 立即创建文件。

### 测试和发布视频站点地图、mRSS馈送或同时发布这两个文件

生成的文件将显示在帐户的根（基本）目录中。

![图像](assets/video-overview/video-overview-8.jpg)

必须发布这些文件，因为视频SEO工具无法自行运行发布。 只要它们被标记为发布，它们就会在下次运行发布时发送到发布服务器。

发布后，您的文件可以使用此URL格式。

![图像](assets/video-overview/video-url-format.png)

示例:

![图像](assets/video-overview/video-format-example.png)

### 提交到搜索引擎

该过程的最后一步是将您的文件和/或URL提交到搜索引擎。 Dynamic Media Classic不能为您执行此步骤；但是，假定您提交的是URL而不是XML文件本身，则下次生成文件并发布时，应更新信息源。

提交到搜索引擎的方法会有所不同，但是对于Google，您使用的是Google Webmaster工具。 到那里后，转到 **站点配置>站点地图** ，然后单击 **提交站点地图** 按钮。 您可以在此处将Dynamic Media Classic URL放置到SEO文件。

### 视频SEO报告

Dynamic Media Classic提供了一个报表，用于显示成功在文件中包括的视频数量，更重要的是，由于错误未包括的视频。 要访问报告，请转到 **设置>应用程序设置>视频SEO >报表**.

![图像](assets/video-overview/video-overview-9.jpg)

## MP4视频的移动实施

Dynamic Media Classic不包含适用于移动设备的查看器预设，因为查看器不需要在支持的移动设备上播放视频。 只要您编码为H.264 MP4格式（在上传时转换或在桌面上预先编码），受支持的平板电脑和智能手机就可以在无需查看器的情况下播放您的视频。 Android和iOS(iPhone和iPad)设备支持此功能。

不需要查看器是因为两个平台都支持本机H.264。 您可以将视频嵌入到HTML5网页中，也可以将视频嵌入到应用程序本身中，Android和iOS操作系统将提供一个控制器来播放视频。

因此，Dynamic Media Classic不会向您提供移动设备查看器的URL，而是为您直接提供视频的URL。 在MP4视频的“预览”窗口中，有“桌面”和“移动设备”链接。 移动URL指向已发布的视频。

关于已发布的视频，需要注意的一点是，URL列出了视频的完整路径，而不仅仅是资产ID。 在处理图像时，无论文件夹结构如何，您都按其资产ID调用图像。 但是，对于视频，还必须指定文件夹结构。 在上面的URL中，视频存储在路径中：

![图像](assets/video-overview/mobile-implement-1.png)

这也可以表示为视频的公司名称/文件夹路径/名称。

### 方法#1：浏览器播放 — HTML5代码

要将您的MP4视频嵌入到网页中，请使用HTML5视频标签。

![图像](assets/video-overview/browser-playback.png)

此方法同样适用于桌面Web，但您可能会遇到浏览器支持问题 — 并非所有桌面Web浏览器都本机支持H.264视频，包括Firefox。

### 方法#2：iOS上的应用程序播放 — Media Player框架

或者，您可以将Dynamic Media Classic MP4视频嵌入到移动设备应用程序代码中。 以下是使用Media Player框架的iOS的通用示例，仅供说明之用：

![图像](assets/video-overview/app-playback.png)

## 其他资源

观看 [Dynamic Media Skill Builder： Dynamic Media Classic中的视频](https://seminars.adobeconnect.com/p2ueiaswkuze) 按需网络研讨会，了解如何使用Dynamic Media Classic中的视频功能。
