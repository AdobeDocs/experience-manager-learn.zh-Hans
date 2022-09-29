---
title: 视频概述
description: Dynamic Media Classic附带了上传视频时的自动转换、到桌面和移动设备的视频流，以及针对基于设备和带宽的播放而优化的自适应视频集。 进一步了解Dynamic Media Classic中的视频，并获取有关视频概念和术语的入门知识。 然后，深入了解如何上传和编码视频，选择用于上传的视频预设，添加或编辑视频预设，在视频查看器中预览视频，将视频部署到Web和移动设备网站，向视频添加字幕和章节标记，以及将视频查看器发布到桌面和移动设备用户。
sub-product: dynamic-media
feature: Dynamic Media Classic, Video Profiles, Viewer Presets
doc-type: tutorial
topics: development, authoring, configuring, videos, video-profiles
audience: all
activity: use
topic: Content Management
role: User
level: Beginner
exl-id: dfbf316f-3922-4bc7-b3f3-2a5bbdeb7063
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '6118'
ht-degree: 1%

---

# 视频概述 {#video-overview}

Dynamic Media Classic附带了上传视频时的自动转换、到桌面和移动设备的视频流，以及针对基于设备和带宽的播放而优化的自适应视频集。 视频最重要的一点是工作流程很简单 — 设计好让任何人都能使用它，即使他们不熟悉视频技术。

在本教程的本节结束时，您将了解如何：

- 将视频上传和编码（转码）为不同的大小和格式
- 从可用的视频预设中进行选择以进行上传
- 添加或编辑视频编码预设
- 在视频查看器中预览视频
- 将视频部署到Web和移动设备网站
- 向视频中添加字幕和章节标记
- 为桌面和移动设备用户自定义和发布视频查看器

>[!NOTE]
>
>本章中的所有URL仅供说明性用途；它们不是实时链接。

## Dynamic Media Classic视频概述

让我们首先更好地了解使用Dynamic Media Classic进行视频的可能性。

### 特性和功能

Dynamic Media Classic视频平台提供视频解决方案的所有部分 — 视频的上传、转换和管理；能够向视频中添加字幕和章节标记；以及使用预设进行轻松播放的功能。

它可以轻松发布高质量自适应视频，以便在多个屏幕(包括桌面、iOS、Android™、BlackBerry®和Windows移动设备)上进行流播放。 自适应视频集是同一个视频的一组版本，这些版本以不同的比特率和格式进行编码，例如 400 kbps、800 kbps 和 1000 kbps。台式计算机或移动设备会检测可用带宽。

此外，如果桌面或移动设备上的网络条件发生更改，则会自动动态切换视频质量。此外，如果客户在桌面上进入全屏模式，则自适应视频集将使用更好的分辨率做出响应，从而改善客户的观看体验。 对于在多个屏幕和设备上播放Dynamic Media Classic视频的客户，使用自适应视频集可以为您提供最佳的播放方式。

### 视频管理

处理视频比处理静态数字图像更复杂。 通过视频，您可以处理多种格式和标准，以及受众能否播放剪辑的不确定性。 Dynamic Media Classic让视频的使用变得轻松，“在机罩下”提供了许多功能强大的工具，但消除了与它们一起使用的复杂性。

Dynamic Media Classic可识别并可以处理多种不同的可用源格式。 但是，阅读视频只是工作的一部分，您还必须将视频转换为Web友好格式。 Dynamic Media Classic通过允许您将视频转换为H.264视频来解决此问题。

使用许多专业和发烧友工具，自行转换视频可能会变得复杂。 Dynamic Media Classic通过提供针对不同质量设置进行了优化的简单预设，从而保持了操作的简单性。 但是，如果您希望获得更多自定义内容，则还可以创建自己的预设。

如果您有大量视频，您将非常感激地能够在Dynamic Media Classic中管理所有资产以及图像和其他媒体。 您可以使用强大的XMP元数据支持来组织、编录和搜索资产（包括视频资产）。

### 视频播放

与将视频转换为易于访问Web的问题类似，也是实施视频并将其部署到您的网站的问题。 选择是购买播放器还是自行构建，使其能够兼容各种设备和屏幕，然后维护您的播放器可能是一项全职工作。

同样，Dynamic Media Classic的方法是允许您选择符合您需求的预设和查看器。 您有许多不同的查看器选项，并且有大量预设库可供使用。

由于Dynamic Media Classic支持HTML5视频，因此您可以轻松地将视频交付到Web和移动设备，这意味着您可以定位运行各种浏览器的用户，以及Android™和iOS平台用户。 流视频允许平滑播放较长或高清内容，而渐进式HTML5视频的预设已针对小屏幕进行了优化。

视频的查看器预设部分可根据查看器类型进行配置。

与所有查看者一样，集成也是通过每个查看者或视频使用一个Dynamic Media Classic URL。

>[!NOTE]
>
>最佳做法是使用Dynamic Media Classic HTML5视频查看器。 HTML5视频查看器中使用的预设是强大的视频播放器。 通过将使用HTML5和CSS设计播放组件、实现嵌入式播放以及根据浏览器功能使用自适应和渐进式流播放的功能整合到单个播放器中，可将富媒体内容的覆盖范围扩展到桌面、平板电脑和移动设备用户，并确保简化视频体验。

关于Dynamic Media Classic视频的最后一条说明，可能适用于某些客户：并非所有公司都为其帐户启用了自动转化、流播放或视频预设。 如果由于某些原因您无法访问流视频的URL，则可能是因此。 您可以上传和发布逐步下载的视频，并有权访问所有视频查看器。 但是，要利用Dynamic Media Classic的完整视频功能，请联系您的客户经理或销售经理以启用这些功能。

详细了解 [视频在Dynamic Media Classic](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/video/quick-start-video.html).

## Video 101

### 基本视频概念和术语

在开始之前，我们先讨论一些您应该熟悉的术语，以便使用视频。 这些概念并非Dynamic Media Classic特有的，如果您要管理专业网站的视频，那么最好就该学习一些有关该主题的进一步教育。 在本节末尾，我们建议使用一些资源。

- **编码/转码。** 编码是应用视频压缩将原始未压缩视频数据转换为更易于处理的格式的过程。 转码（虽然类似）是指从一种编码方法转换为另一种编码方法。

   - 使用视频编辑软件创建的主控视频文件通常太大，且格式不正确，无法交付到在线目标。 通常，会对它们进行编码，以便在桌面上快速播放并进行编辑，但不能通过Web进行交付。
   - 为了将数字视频转换为在不同屏幕上播放的适当格式和规范，视频文件会被转码为更小、更高效的文件大小，以便最好地传送到Web和移动设备。

- **视频压缩。** 减少用于表示数字视频图像的数据量，是空间图像压缩和时间运动补偿的组合。

   - 大多数压缩技术都有损，这意味着它们会丢弃数据以实现较小的大小。
   - 例如，DV视频压缩相对较少，可以轻松编辑源素材，但是它太大，无法通过Web使用，甚至无法放入DVD。

- **文件格式。** 格式是一个容器，与ZIP文件类似，它确定了文件在视频文件中的组织方式，但通常不确定它们的编码方式。

   - 源视频的常见文件格式包括Windows Media(WMV)、QuickTime(MOV)、Microsoft® AVI和MPEG等。 由Dynamic Media Classic发布的格式为MP4。
   - 视频文件通常包含多个轨道 — 一个视频轨道（无音频）和一个或多个音频轨道（无视频） — 这些轨道是相互关联和同步的。
   - 视频文件格式可确定这些不同数据跟踪和元数据的组织方式。

- **编解码.** 视频编解码器描述使用压缩对视频进行编码的算法。 音频也通过音频编解码器进行编码。

   - 编解码器可最大程度地减少播放视频所需的信息量。 与每个单独帧的信息不同，只存储关于一个帧与下一个帧之间差异的信息。
   - 由于大多数视频从一个帧到另一个帧的变化很小，因此编解码器允许高压缩率，这会导致较小的文件大小。
   - 视频播放器根据其编解码器对视频进行解码，然后在屏幕上显示一系列图像或帧。
   - 常见视频编解码器包括H.264、On2 VP6和H.263。

![图像](assets/video-overview/bird-video.png)

- **解决方法.** 视频的高度和宽度（以像素为单位）。

   - 源视频的大小由相机和编辑软件的输出决定。 高清相机可创建高分辨率1920 x 1080视频，但为了在Web上顺利播放，您可以将其缩减像素采样（调整大小）以缩小分辨率，如1280 x 720、640 x 480或更小。
   - 分辨率会直接影响播放该视频所需的文件大小和带宽。

- **显示宽高比。** 视频的宽度与视频高度的比率。 当视频的宽高比与播放器的比例不匹配时，您可能会看到“黑条”或空白。 用于显示视频的两种常见宽高比是：

   - 4:3(1.33:1)。 用于几乎所有标准定义电视广播内容。
   - 16:9(1.78:1)。 用于几乎所有宽屏、高清电视内容(HDTV)和电影。

- **比特率/数据率。** 经过编码以构成视频播放一秒的数据量（千比特/秒）。

   - 通常，比特率越低，对Web就越有意义，因为它的下载速度会更快。 但是，这也意味着由于压缩损失，质量很低。
   - 良好的编解码器应该在低比特率与高质量之间实现平衡。

- **帧速率（每秒帧数，或FPS）。** 每秒视频的帧数或静止图像数。 通常，北美电视(NTSC)以29.97 FPS的速率广播；欧洲和亚洲电视(PAL)以25 FPS的速率广播；和电影（模拟和数字）通常以24(23.976)FPS的速率显示。

   - 为了让事情更加混乱，还有逐行和交错的帧。 每个逐行帧包含整个图像帧，而隔行帧包含图像帧中每隔一行像素。 然后，这些帧会快速播放，并且看起来会混合在一起。 电影使用逐行扫描方法，而数字视频通常是隔行扫描的。
   - 通常，源素材是否是隔行扫描无关紧要 — Dynamic Media Classic将保留转换后视频中的扫描方法。
   - 流式/渐进式交付。 视频流是指在连续的流中发送媒体，在媒体到达时可以播放，而逐步下载的视频会像从服务器下载的任何其他文件一样下载，并缓存在浏览器的本地。

希望本入门读物有助于您了解使用Dynamic Media Classic视频时涉及的各种选项。

## 视频工作流

在Dynamic Media Classic中处理视频时，您会遵循与处理图像类似的基本工作流程。

![图像](assets/video-overview/video-overview-2.png)

1. 首先，将视频文件上传到Dynamic Media Classic。 为此，请打开 **“工具”菜单** 在Dynamic Media Classic扩展面板的底部，然后选择 **上传到Dynamic Media Classic >文件到文件夹名称**&#x200B;或 **上传到Dynamic Media Classic >文件夹到文件夹名称**. “文件夹名称”是您当前正在浏览的使用该扩展名的任何文件夹。 视频文件可能较大，因此我们建议使用FTP上传大文件。 在上传过程中，请选择一个或多个视频预设，以对您的视频进行编码。 视频在上传时可转码为MP4视频。 有关使用和创建编码预设的更多信息，请参阅下面的视频预设主题。 了解 [上传和编码视频](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/video/uploading-encoding-videos.html).
2. 选择或选择并修改视频查看器预设，然后预览您的视频。 您可以选择预建的查看器预设，也可以自定义您自己的查看器预设。 如果您定位的是移动设备用户，则无需在此处执行任何操作，因为移动平台不需要查看器或预设。 详细了解 [在视频查看器中预览视频](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/video/previewing-videos-video-viewer.html) 和 [添加或编辑视频查看器预设](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/video/previewing-videos-video-viewer.html#adding-or-editing-a-video-viewer-preset).
3. 运行视频发布、获取URL并进行集成。 视频工作流与图像工作流的此步骤的主要区别在于，您运行的是特殊的视频发布，而不是（或可能是）标准的图像服务发布。 桌面上的视频查看器集成的工作方式与图像查看器集成完全相同，但对于移动设备而言，集成更简单 — 您只需要视频本身的URL。

### 关于转码

转码在之前定义为从一种编码方法转换为另一种编码方法的过程。 对于Dynamic Media Classic，它是将源视频从其当前格式转换为MP4的过程。 在桌面浏览器或移动设备上显示您的视频之前，需要执行此操作。

Dynamic Media Classic可以为您处理所有转码，这是一项巨大的优势。 您可以自行转码视频并上传已转换为MP4的文件，但这可能是一个需要复杂软件的复杂过程。 除非您知道自己在做什么，否则您通常不会在首次尝试时获得良好的结果。

Dynamic Media Classic不仅会为您转换文件，而且还会提供易于使用的预设，从而简化转换过程。 您确实不需要详细了解此过程的技术方面 — 您应该知道的只是您希望从系统中移出的最终大小以及最终用户拥有的带宽。

虽然预建的预设非常方便，而且可满足大多数需求，但有时您还是希望自定义一些内容。 在这种情况下，您可以创建自己的编码预设。 在Dynamic Media Classic中，编码预设称为视频预设。 本章稍后将对此进行说明。

### 关于流

另一个值得注意的主要功能是视频流，这是Dynamic Media Classic视频平台的标准功能。 流媒体在交付时会不断地被接收并呈现给最终用户。 这是重要的，也是可取的，原因有几点。

与渐进式下载相比，流播放通常需要的带宽更少，因为只会交付观看的视频部分。 Dynamic Media Classic视频流服务器和查看者使用自动带宽检测来提供用户互联网连接的最佳流。

在流播放中，视频开始播放的时间早于使用其他方法的时间。 它还可以更有效地利用网络资源，因为只有被观看的视频部分才会被发送到客户端。

另一种投放方法是渐进式下载。 与流视频相比，渐进式下载只有一个一致的好处 — 您不需要流服务器即可交付视频。 这当然是Dynamic Media Classic的用处 — Dynamic Media Classic在平台中内置了一个流服务器，因此您无需费心维护这块专用硬件。

可从任何普通Web服务器提供渐进式下载视频。 虽然这可以很方便而且可能具有成本效益，但请记住，渐进式下载具有有限的搜寻和导航功能，用户可以访问您的内容并重新设定其用途。 在某些情况下（例如严格网络防火墙后的播放），可能会阻止流式传输；在这些情况下，可以要求回退到渐进式交付。

对于流量要求较低的爱好者或网站而言，渐进式下载是一个不错的选择；如果用户不介意将其内容缓存在用户计算机上；如果他们只需要传送长度较短的视频（少于10分钟）；或者，如果访客由于某些原因无法接收流视频。

如果您需要高级功能和控制视频交付，并且/或者您需要向较大的受众（例如，多个100个同时查看者）显示视频，跟踪和报告使用情况或查看统计信息，或者希望为查看者提供最佳的交互式播放体验，则需要流式传输您的视频。

最后，如果您担心在知识产权或权限管理问题上保护媒体的安全，流媒体将提供更加安全的视频交付，因为流媒体在流媒体播放时不会保存到客户端的缓存中。

## 视频预设

在上传视频时，您可以从一个或多个预设中进行选择，这些预设包含通过编码将主控视频转换为Web友好格式的设置。 视频预设有两种类型：自适应视频预设和单个编码预设。

请参阅 [可用的视频预设](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/setup/application-setup.html#video-presets-for-encoding-video-files).

自适应视频预设默认处于激活状态，这意味着它们可用于编码。 如果您希望使用单个编码预设，则管理员需要激活该预设，以使其显示在视频预设列表中。

了解如何 [激活或取消激活视频预设](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/video/uploading-encoding-videos.html#activating-or-deactivating-video-encoding-presets).

您可以选择Dynamic Media Classic附带的许多预建预设中的任一预设，也可以自行创建；但是，默认情况下不会选择任何预设进行上传。 换句话说， **如果您在上传时未选择视频预设，则您的视频将不会进行转换，并且可能无法发布**. 但是，您可以自行离线转换视频，并上传和发布视频，这一切正常。 只有当您希望Dynamic Media Classic为您进行转换时，才需要使用视频预设。

上传时，您可以通过选择 **视频选项** （在“作业选项”面板中）。 然后，选择是要对“计算机”、“移动设备”还是“平板电脑”进行编码。

- 计算机用于桌面。 在此，您通常会发现占用更多带宽的较大预设（如HD）。
- 移动设备和平板电脑可为iPhone和Android™智能手机等设备创建MP4视频。 移动设备和平板电脑之间的唯一区别在于，平板电脑预设通常具有更高的带宽，因为它们基于WiFi使用情况。 移动设备预设已针对3G使用速度较慢进行了优化。

### 在选择预设之前要自问的问题

在选择预设时，您应该了解受众和源素材。 您对客户了解多少？ 他们如何用电脑显示器或移动设备观看视频？

您的视频分辨率是多少？ 如果您选择的预设大于原始预设，则可能会获得一个模糊/像素化的视频。 如果视频大于预设，则无需选择大于源视频的预设。

它的宽高比是多少？ 如果您在转换后的视频周围看到黑条，则选择了错误的宽高比。 Dynamic Media Classic无法自动检测这些设置，因为它必须首先在上传之前检查文件。

### 视频选项划分

视频预设通过指定这些设置来确定视频的编码方式。 如果您不熟悉这些术语，请查阅上面的视频基本概念和术语主题。

![图像](assets/video-overview/video-overview-3.jpg)

- **宽高比.** 标准4:3或宽屏16:9。
- **大小.** 此值与显示分辨率相同，以像素为单位。 这与宽高比有关。 以16:9的比率计算，视频为432 x 240像素，而在4:3的比率为320 x 240像素。
- **FPS.** 标准帧速率为30帧/秒、25帧/秒或24帧/秒(fps)，具体取决于视频标准 — NTSC、PAL或Film。 此设置无关紧要，因为Dynamic Media Classic始终使用与源视频相同的帧率。
- **格式.** 这是MP4。
- **带宽。** 这是目标用户的所需连接速度。 他们的互联网连接是快还是慢？ 他们通常使用台式计算机还是移动设备？ 这也与分辨率（大小）有关，因为视频越大，它需要的带宽就越多。

### 确定视频的数据速率或“比特率”

计算视频的比特率是向Web提供视频的最不为人所理解的因素之一，但可能最为重要，因为它会直接影响用户体验。 如果您设置的比特率过高，则视频质量会很高，但性能会很差。 网络连接速度较慢的用户在视频播放时会被迫等待视频不断暂停。 但是，如果设置得太低，质量就会受到影响。 在“视频预设”中，Dynamic Media Classic会根据您的目标带宽提供一系列数据。 这是个好开始。

但是，如果你想自己弄明白，你需要一个比特计算器。 这是视频专业人士和发烧友通常使用的工具，用于估算特定流或介质（如DVD）中的数据量。

## 创建自定义视频预设

有时，您可能会发现需要一个与内置编码视频预设的设置不匹配的特殊视频预设。 如果您有特定大小的自定义视频，例如从3D动画软件创建的视频或从原始大小裁剪的视频，则可能会发生这种情况。 您可能希望尝试使用不同的带宽设置来提供更高或更低的视频质量。 无论出现什么情况，都应创建一个自定义的单编码视频预设。

### 视频预设工作流

1. 视频预设位于 **设置>应用程序设置>视频预设**. 在此，您将找到公司可用的所有编码预设的列表。

   - 每个流视频帐户都有数十个现成的预设，如果您创建自己的自定义预设，您也会在此处看到这些预设。
   - 您可以使用下拉菜单按类型进行过滤。 这些预设分为“计算机”、“移动设备”和“平板电脑”。
      ![图像](assets/video-overview/video-overview-4.jpg)

2. 活动列允许您选择是要在上传时显示所有预设，还是只显示您选择的预设。 如果您在美国，则可能要取消选中“欧洲PAL预设”，如果在英国/欧洲、中东和非洲，请取消选中“NTSC预设”。
3. 单击 **添加** 按钮以创建自定义预设。 此时将打开添加视频预设面板。 此处的过程与创建图像预设类似。
4. 首先，给它一个 **预设名称** 才能显示在预设列表中。 在以上示例中，此预设用于屏幕捕获教程视频。
5. 的 **描述** 是可选的，但它会为用户提供一个用于描述此预设用途的工具提示。
6. 的 **编码文件后缀** 将附加到您在此处创建的视频名称的末尾。 请记住，您将拥有一个主控视频以及这个编码视频，该视频是主控的一个派生项，并且Dynamic Media Classic中的任何两个资产都不能具有相同的资产ID。
7. **播放设备** 是您选择所需视频文件格式（计算机、移动设备或平板电脑）的位置。 请记住，移动和平板电脑会生成相同的MP4格式。 Dynamic Media Classic只需知道要将预设置放在哪个类别即可；但是，理论上的区别在于，平板电脑预设通常支持更快的互联网连接，因为所有预设都支持WiFi。
8. **目标数据率** 这是您必须自己搞清楚的事情，不过您可以在下图上看到建议的范围。 您也可以将滑块拖动到近似目标带宽。 要获得更精确的数字，请使用位速率计算器。 涉及一些试用和错误。

   ![图像](assets/video-overview/video-overview-5.jpg)

9. 设置源文件的 **宽高比**. 此设置直接与下面的大小绑定。 如果您选择 _自定义_，则必须手动输入宽度和高度。
10. 如果选择宽高比，则为 **分辨率大小** ，而Dynamic Media Classic将自动填充另一个值。 但是，对于自定义宽高比，请填写这两个值。 您的大小应与数据率一致。 如果设置低数据率和大数据量，则预计质量会很差。
11. 单击 **保存** 来保存预设。 与其他每个预设不同，您此时无需发布，因为这些预设仅用于上传文件。 之后，您必须发布已编码的视频，但预设仅供内部Dynamic Media Classic使用。
12. 要验证您的视频预设是否位于上传列表中，请转到 **上传**. 选择 **作业选项** 展开 **视频选项**. 您的预设会列在您选择的播放设备（“计算机”、“移动设备”或“平板电脑”）的类别中。

详细了解 [添加或编辑视频预设](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/video/uploading-encoding-videos.html#adding-or-editing-a-video-encoding-preset).

## 在视频中添加字幕

有时，在视频中添加字幕会很有用 — 例如，当您需要以多种语言向查看者提供视频，但不希望以其他语言复制音频或以不同语言再次录制视频时。 此外，添加字幕可为听力欠佳且使用隐藏式字幕的用户提供更多辅助功能。 Dynamic Media Classic让您能够轻松地在视频中添加字幕。

了解如何 [向视频中添加字幕](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/video/adding-captions-video.html).

## 将章节标记添加到视频

对于长格式视频，查看者可能希望通过使用章节标记导航视频来体验其功能和便利性。 Dynamic Media Classic提供轻松向视频添加章节标记的功能。

了解如何 [将章节标记添加到视频](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/video/adding-chapter-markers-video.html).

## 视频实施主题

### 发布和复制URL

Dynamic Media Classic工作流中的最后一步是发布视频内容。 但是，视频有其自己的发布作业，称为视频服务器发布，该作业位于高级下。

![图像](assets/video-overview/video-overview-6.jpg)

了解如何 [发布视频](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/video/deploying-video-websites-mobile-sites.html#publishing-video).

运行视频发布后，您便能够获得一个URL，以在Web浏览器中访问您的视频和任何现成的Dynamic Media Classic查看器预设。 但是，如果您自定义或创建自己的视频查看器预设，则需要运行单独的图像服务器发布。

- 了解如何 [将URL关联到移动设备网站或网站](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/video/deploying-video-websites-mobile-sites.html#linking-a-video-url-to-a-mobile-site-or-a-website).
- 了解如何 [在网页上嵌入视频查看器](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/video/deploying-video-websites-mobile-sites.html#embedding-the-video-viewer-on-a-web-page).

您还可以使用第三方或自定义构建的视频播放器来部署视频。

了解如何 [使用第三方视频播放器部署视频](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/video/deploying-video-websites-mobile-sites.html#deploying-video-using-a-third-party-video-player).

此外，如果您还要使用视频缩略图（从视频中提取的图像），则需要运行图像服务器发布。 这是因为视频的缩略图图像位于图像服务器上，而视频本身位于视频服务器上。 视频缩略图可用于视频搜索结果、视频播放列表，并可用作视频查看器中在视频播放之前显示的初始“海报帧”。

详细了解 [使用视频缩略图](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/video/deploying-video-websites-mobile-sites.html#working-with-video-thumbnails).

### 选择和自定义查看器预设

选择和自定义查看器预设的过程与图像的过程相同。 您可以创建预设或修改现有预设并使用新名称进行保存、进行编辑，然后运行图像提供发布。 所有查看器预设都会发布到图像服务器，而不仅仅是图像的预设，因此您必须运行图像发布才能查看新的或修改的预设。

>[!TIP]
>
>在视频服务器发布后运行图像提供发布，以发布与您的视频关联的任何缩略图。

## 视频搜索引擎优化

搜索引擎优化(SEO)是提高搜索引擎中网站或网页可见性的过程。 虽然搜索引擎在收集基于文本的内容的信息方面非常出色，但是它们无法充分获取有关视频的信息，除非向它们提供这些信息。 使用Dynamic Media Classic视频SEO，您可以使用元数据为搜索引擎提供视频的描述。 视频SEO功能允许您创建视频站点地图和媒体RSS(mRSS)源。

- **视频站点地图**. 准确告知Google视频内容在网站上的位置和内容。 因此，可在Google上完全搜索视频。 例如，视频站点地图可以指定视频的运行时间和类别。
- **mRSS馈送**. 内容发布者使用将媒体文件馈送到Yahoo! 视频搜索。 Google支持视频站点地图和媒体RSS(mRSS)源协议，以便向搜索引擎提交信息。

在创建视频站点地图和mRSS源时，您可以决定要包含的视频文件中的元数据字段。 这样，您就可以向搜索引擎描述您的视频，以便搜索引擎能够更准确地将流量定向到您网站上的视频。

创建站点地图或信息源后，您可以让Dynamic Media Classic自动发布、手动发布，或只生成稍后可编辑的文件。 此外，Dynamic Media Classic每天都可以自动生成和发布此文件。

在流程结束时，您会将文件或URL提交到您的搜索引擎。 这项任务是在Dynamic Media Classic以外完成的；不过，我们在下面简要地讨论了这个问题。

### 站点地图/mRSS文件的要求

要使Google和其他搜索引擎不会拒绝您的文件，它们必须采用正确的格式并包含某些信息。 Dynamic Media Classic生成格式正确的文件；但是，如果某些视频没有相应的信息，则文件中不会包含这些信息。

必填字段包括“登陆页面”（提供视频的页面的URL，而不是视频本身的URL）、“标题”和“描述”。 每个视频必须包含这些项目的条目，否则不会将其包含在生成的文件中。 可选字段包括“标记”和“类别”。

还有另外两个必填字段 — 内容URL、视频资产本身的URL以及视频缩略图的URL — 但Dynamic Media Classic会自动为您填充这些值。

建议的工作流是在使用XMP元数据上传之前将此数据嵌入到您的视频中，Dynamic Media Classic将在上传时提取此数据。 您将使用诸如Adobe Bridge(随所有Adobe Creative Cloud应用程序一起提供)之类的应用程序将数据填充到标准元数据字段中。

按照此方法操作，您无需使用Dynamic Media Classic手动输入此数据。 但是，您也可以使用Dynamic Media Classic中的元数据预设，作为每次输入相同数据的快速方法。

有关该主题的详细信息，请参阅 [查看、添加和导出元数据](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/managing-assets/viewing-adding-exporting-metadata.html).

![图像](assets/video-overview/video-overview-7.jpg)

填充元数据后，您便能够在该视频资产的详细信息视图中看到该元数据。 关键词也可能存在，但它们位于“关键词”选项卡下。

- 详细了解 [添加关键词](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/managing-assets/viewing-adding-exporting-metadata.html#add-or-edit-keywords).
- 详细了解 [视频SEO](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/setup/video-seo-search-engine-optimization.html).
- 了解 [视频SEO设置](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/setup/video-seo-search-engine-optimization.html#choosing-video-seo-settings).

#### 设置视频SEO

设置视频SEO首先需要选择所需的格式类型、生成方法以及应将哪些元数据字段放入文件中。

1. 转到 **设置>应用程序设置>视频SEO >设置**.
2. 在 **生成模式** 菜单，选择文件格式。 默认值为“关闭”，因此要启用它，请选择“视频站点地图”、“mRSS”或“两者均”。
3. 选择自动还是手动生成。 为简单起见，我们建议您将其设置为 **自动模式**. 如果选择“自动”，则还应将 **标记为发布** 选项，否则文件将不会上线。 站点地图和RSS文件是XML文档的类型，必须像任何其他资产一样进行发布。 如果您当前未准备好所有信息，或者只想进行一次性生成，请使用手动模式之一。
4. 填充文件中使用的元数据标记。 此步骤不是可选的。 您至少必须包含标有星号(\*)的三个字段： **登陆页面** , **标题**&#x200B;和 **描述**. 要将元数据用于这些任务，请将右侧元数据面板中的字段拖放到表单的相应字段中。 Dynamic Media Classic将自动使用每个视频的实际数据填充占位符字段。 您不必使用元数据字段。 您可以在此处键入一些静态文本，但每个视频将显示相同的文本。
5. 在三个必填字段中输入信息后，Dynamic Media Classic将启用 **保存** 和 **保存并生成** 按钮。 单击一个以保存设置。 使用 **保存** 如果您处于自动模式，并且希望让Dynamic Media Classic稍后生成文件。 使用 **保存并生成** 以立即创建文件。

### 测试和发布视频站点地图、mRSS源或两个文件

生成的文件将显示在帐户的根（基）目录中。

![图像](assets/video-overview/video-overview-8.jpg)

必须发布这些文件，因为视频SEO工具不能单独运行发布。 只要标记为发布，它们就会在下次运行发布时发送到发布服务器。

发布后，您的文件可使用此URL格式。

![图像](assets/video-overview/video-url-format.png)

示例:

![图像](assets/video-overview/video-format-example.png)

### 提交到搜索引擎

该过程的最后一步是将文件和/或URL提交到搜索引擎。 Dynamic Media Classic不能为你做这一步；但是，如果您提交的是URL而不是XML文件本身，则下次生成文件并发布时，您的信息源应会更新。

提交到搜索引擎的方法将有所不同，但对于Google，您使用的是Google Webmaster工具。 到了之后，转到 **网站配置>站点地图** ，然后单击 **提交站点地图** 按钮。 您可以在此将Dynamic Media Classic URL放置到SEO文件。

### 视频SEO报表

Dynamic Media Classic会提供一个报表，用于显示文件中成功包含的视频数量，更重要的是，由于错误，未包含这些视频。 要访问报表，请转到 **设置>应用程序设置>视频SEO >报表**.

![图像](assets/video-overview/video-overview-9.jpg)

## MP4视频的移动实施

Dynamic Media Classic不包含移动设备的查看器预设，因为查看器在支持的移动设备上播放视频时不需要使用查看器。 只要您编码为H.264 MP4格式（通过上传时转换或在台式机上预编码），受支持的平板电脑和智能手机就可以播放您的视频，而无需观看者。 Android和iOS(iPhone和iPad)设备支持此功能。

无需查看器的原因是，两个平台都支持本机H.264。 您可以将视频嵌入到HTML5网页中，或将视频嵌入到应用程序本身中，Android和iOS操作系统将提供一个控制器来播放视频。

因此，Dynamic Media Classic不会为您提供用于移动设备的查看器的URL，而是直接为您提供视频的URL。 在MP4视频的“预览”窗口中，有适用于桌面和移动设备的链接。 移动设备URL指向已发布的视频。

关于已发布的视频，需要注意的一个重要事项是，URL会列出视频的完整路径，而不仅仅是资产ID。 处理图像时，无论文件夹结构如何，您都会按其资产ID调用图像。 但是，对于视频，您还必须指定文件夹结构。 在以上URL中，视频存储在路径中：

![图像](assets/video-overview/mobile-implement-1.png)

也可以以公司名称/文件夹路径/视频名称表示。

### 方法#1:浏览器播放 — HTML5代码

要在网页中嵌入MP4视频，请使用HTML5视频标记。

![图像](assets/video-overview/browser-playback.png)

此方法也适用于桌面Web，但是您可能会在浏览器支持方面遇到问题 — 并非所有桌面Web浏览器本身都支持H.264视频，包括Firefox。

### 方法#2:iOS上的应用程序播放 — Media Player框架

或者，您也可以将Dynamic Media Classic MP4视频嵌入到移动应用程序代码中。 以下是使用媒体播放器框架的iOS的通用示例，该示例仅供说明用途：

![图像](assets/video-overview/app-playback.png)

## 其他资源

观看 [Dynamic Media技能培养：视频在Dynamic Media Classic](https://seminars.adobeconnect.com/p2ueiaswkuze) 按需网络研讨会，了解如何使用Dynamic Media Classic中的视频功能。
