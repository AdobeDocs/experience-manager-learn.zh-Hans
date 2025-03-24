---
title: Dispatcher了解缓存
description: 了解Dispatcher模块如何操作其缓存。
topic: Administration, Performance
version: Experience Manager 6.5
role: Admin
level: Beginner
thumbnail: xx.jpg
doc-type: Article
exl-id: 66ce0977-1b0d-4a63-a738-8a2021cf0bd5
duration: 407
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1708'
ht-degree: 0%

---

# 了解缓存

[目录](./overview.md)

[&lt; — 上一页：配置文件说明](./explanation-config-files.md)

本文档将说明Dispatcher缓存的发生方式以及如何对其进行配置

## 缓存目录

我们在基线安装中使用以下默认缓存目录

- 创作
   - `/mnt/var/www/author`
- 发布者
   - `/mnt/var/www/html`

当每个请求遍历Dispatcher时，这些请求遵循配置的规则以保留本地缓存的版本，从而响应符合条件的项目

>[!NOTE]
>
>我们刻意将已发布的工作负载与创作工作负载分开，因为当Apache在DocumentRoot中查找文件时，它不知道该文件来自哪个AEM实例。 因此，即使您在作者场中禁用了缓存，如果作者的DocumentRoot与publisher相同，则它将在存在时从缓存中提供文件。 这意味着您将从已发布的缓存为提供创作文件，并为您的访客创造非常糟糕的混合匹配体验。
>
>为不同的已发布内容保留单独的DocumentRoot目录也是个非常糟糕的主意。 您必须创建多个重新缓存的项目，这些项目在clientlibs等站点之间没有区别，还必须为您设置的每个DocumentRoot设置复制刷新代理。 增加每次页面激活时头顶的刷新量。 依赖文件的命名空间及其完全缓存的路径，并避免为已发布的站点使用多个DocumentRoot。

## 配置文件

Dispatcher控制任何场文件的`/cache {`部分中符合缓存条件的内容。 
在AMS基线配置场中，您会发现包含的，如下所示：


```
/cache { 
    /rules { 
        $include "/etc/httpd/conf.dispatcher.d/cache/ams_author_cache.any" 
    }
```


在创建缓存或不缓存内容的规则时，请参阅文档[此处](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#configuring-the-dispatcher-cache-cache)


## 缓存作者

我们已经看到有许多实施中的用户不缓存创作内容。 
他们在性能和对作者的响应能力方面错过了巨大的提升。

让我们讨论一下在将创作场配置为正确缓存时采取的策略。

以下是作者场文件的基本作者`/cache {`部分：


```
/cache { 
    /docroot "/mnt/var/www/author" 
    /statfileslevel "2" 
    /allowAuthorized "1" 
    /rules { 
        $include "/etc/httpd/conf.dispatcher.d/cache/ams_author_cache.any" 
    } 
    /invalidate { 
        /0000 { 
            /glob "*" 
            /type "allow" 
        } 
    } 
    /allowedClients { 
        /0000 { 
            /glob "*.*.*.*" 
            /type "deny" 
        } 
        $include "/etc/httpd/conf.dispatcher.d/cache/ams_author_invalidate_allowed.any" 
    } 
}
```

此处需要注意的重要事项是，`/docroot`设置为作者的缓存目录。

>[!NOTE]
>
>确保作者的`.vhost`文件中的您的`DocumentRoot`与场`/docroot`参数匹配

缓存规则include语句包含文件`/etc/httpd/conf.dispatcher.d/cache/ams_author_cache.any`，该文件包含以下规则：

```
/0000 { 
 /glob "*" 
 /type "deny" 
} 
/0001 { 
 /glob "/libs/*" 
 /type "allow" 
} 
/0002 { 
 /glob "/libs/*.html" 
 /type "deny" 
} 
/0003 { 
 /glob "/libs/granite/csrf/token.json" 
 /type "deny" 
} 
/0004 { 
 /glob "/apps/*" 
 /type "allow" 
} 
/0005 { 
 /glob "/apps/*.html" 
 /type "deny" 
} 
/0006 { 
 /glob "/libs/cq/core/content/welcome.*" 
 /type "deny" 
}
```

在创作场景中，内容会随时随意更改。 您只想缓存不经常更改的项目。
我们有缓存`/libs`的规则，因为它们是基准AEM安装的一部分，在安装Service Pack、累积修补程序包、升级或修补程序之前可能会发生更改。 因此，缓存这些元素非常合理，并且真正为使用站点的最终用户的创作体验带来了巨大好处。

>[!NOTE]
>
>请记住，这些规则还会缓存<b>`/apps`</b>自定义应用程序代码所在的位置。 如果您在此实例上开发代码，则在保存文件时会发现非常混乱，并且不会看到是否在UI中反映，因为它提供缓存的副本。 这样做的意图是，如果您将代码部署到AEM中，那么这种部署也是不频繁的，并且部署步骤的一部分应当是清除创作缓存。 同样，其优势也是巨大的，可缓存的代码可为最终用户更快地运行。

## ServeOnStale（又称陈旧/SOS服务）

这是Dispatcher的一项功能所创造的宝石之一。 如果发布者负载不足或变得无响应，它通常会引发502或503 http响应代码。 如果发生这种情况并启用了此功能，则会指示Dispatcher尽最大努力仍提供缓存中任何内容即使它不是新副本也是如此。 如果您已获得某些内容，则最好提供该内容，而不是只显示一条不提供任何功能的错误消息。

>[!NOTE]
>
>请记住，如果发布者渲染器存在套接字超时或500错误消息，则此功能不会触发。 如果AEM无法访问，则此功能不执行任何操作

此设置可以在任何场中设置，但只能在发布场文件中应用此设置。 以下是场文件中启用的功能的语法示例：

```
/cache { 
    /serveStaleOnError "1"
```

## 使用查询参数/参数缓存页面

>[!NOTE]
>
>Dispatcher模块的正常行为之一是，如果请求在URI中具有查询参数（通常显示为`/content/page.html?myquery=value`），它将跳过文件缓存并直接转到AEM实例。 它将此请求视为动态页面，不应缓存。 这可能会对缓存效率产生不良影响。

请参阅此[文章](https://github.com/adobe/aem-dispatcher-optimizer-tool/blob/main/docs/Rules.md#dot---the-dispatcher-publish-farm-cache-should-have-its-ignoreurlparams-rules-configured-in-an-allow-list-manner)，其中显示重要的查询参数如何影响网站性能。

默认情况下，您希望将`ignoreUrlParams`规则设置为允许`*`。  这意味着所有查询参数都将被忽略，并且允许缓存所有页面，无论使用什么参数。

以下是一个示例，其中某人建立了一个社交媒体深层链接引用机制，该机制使用URI中的参数引用知道此人来自何处。

*可忽略的示例：*

- https://www.we-retail.com/home.html?reference=android
- https://www.we-retail.com/home.html?reference=facebook

该页面是100%可缓存的，但由于参数存在而不进行缓存。 
将您的`ignoreUrlParams`配置为允许列表将有助于解决此问题：

```
/cache { 
    /ignoreUrlParams { 
        /0001 { /glob "*" /type "allow" }
    }
```

现在，当Dispatcher看到该请求时，它将忽略该请求具有`?`引用的`query`参数并且仍缓存页面这一事实

<b>动态示例：</b>

- https://www.we-retail.com/search.html?q=fruit
- https://www.we-retail.com/search.html?q=vegetables

请记住，如果您的查询参数使页面发生更改，则它会呈现输出，然后您需要将它们从忽略列表中排除，并使页面再次不可缓存。  例如，使用查询参数的搜索页面会更改渲染的原始html。

下面是每个搜索的html源：

`/search.html?q=fruit`：

```
<html>
...SNIP...
    <div id='results'>
        <div class='result'>
            Orange
        </div>
        <div class='result'>
            Apple
        </div>
        <div class='result'>
            Strawberry
        </div>
    </div>
</html>
```

`/search.html?q=vegetables`：

```
<html>
...SNIP...
    <div id='results'>
        <div class='result'>
            Carrot
        </div>
        <div class='result'>
            Cucumber
        </div>
        <div class='result'>
            Celery
        </div>
    </div>
</html>
```

如果您先访问了`/search.html?q=fruit`，那么它将缓存html并显示结果。

然后您访问`/search.html?q=vegetables`秒，但将显示水果结果。
这是因为在缓存方面忽略了`q`的查询参数。  要避免出现此问题，您需要记录根据查询参数呈现不同HTML并拒绝缓存这些参数的页面。

示例：

```
/cache { 
    /ignoreUrlParams { 
        /0001 { /glob "*" /type "allow" }
        /0002 { /glob "q" /type "deny"  }
    }
```

通过Javascript使用查询参数的页面仍可完全发挥作用，忽略此设置中的参数。  因为它们不会更改静态的html文件。  他们使用javascript在本地浏览器上实时更新浏览器。  这意味着如果您使用javascript查询参数，则很有可能在页面缓存中忽略此参数。  允许该页面缓存并享受性能提升！

>[!NOTE]
>
>跟踪这些页面确实需要一些维护，但性能提升非常值得。  您可以要求您的CSE对您的网站流量运行报告，为您提供过去90天内使用查询参数的所有页面列表，以便您进行分析并确保您知道要查看的页面以及不能忽略的查询参数

## 缓存响应标头

很明显，Dispatcher缓存`.html`页面和clientlibs（即`.js`、`.css`），但是您知道它还可以将特定响应标头与内容一起缓存到文件中，该文件的名称相同，但文件扩展名为`.h`。 这样不仅可以对内容进行下一个响应，还可以对缓存中应随其一起使用的响应标头进行响应。

AEM可以处理的不只是UTF-8编码

有时，项目具有特殊的标头，可帮助控制缓存TTL的编码详细信息以及上次修改的时间戳。

默认情况下，缓存后的这些值会被清除，Apache httpd Web服务器会使用其正常的文件处理方法自行处理资产，一般情况下，这些方法仅限于根据文件扩展名进行mime类型推测。

如果您拥有Dispatcher缓存资产和所需的标头，则可以向客户端浏览器展示适当的体验并确保所有详细信息都显示。

以下是场示例，其中指定了要缓存的标头：

```
/cache { 
 /headers { 
  "Cache-Control" 
  "Content-Disposition" 
  "Content-Type" 
  "Expires" 
  "Last-Modified" 
  "X-Content-Type-Options" 
 } 
}
```


在示例中，他们已将AEM配置为提供标头，CDN将查找以了解何时将其缓存失效。 这意味着，AEM现在可以基于标头正确规定哪些文件无效。

>[!NOTE]
>
>请记住，您不能使用正则表达式或glob匹配。 它是要缓存的标头的文字列表。 仅放入要缓存的文本标头的列表中。

## 自动使宽限期失效

如果作者的AEM系统具有大量活动，而这些作者又具有许多页面激活功能，则您可能会遇到反复出现无效情况的争用情况。 无需频繁重复的刷新请求，您可以构建某种容差，以在宽限期清除之前不重复刷新。

### 其工作原理示例：

如果您有5个使`/content/exampleco/en/`失效的请求，则所有请求都会在3秒内发生。

如果关闭此功能，您将使缓存目录`/content/exampleco/en/`失效5次

此功能打开且设置为5秒时，它将使缓存目录`/content/exampleco/en/` <b>失效</b>一次

以下是为5秒宽限期配置的此功能的语法示例：

```
/cache { 
    /gracePeriod "5"
```

## 基于TTL的失效

Dispatcher模块的较新功能是基于`Time To Live (TTL)`的缓存项目失效选项。 当项目被缓存时，它会查找是否存在缓存控制标头，并在缓存目录中生成一个具有相同名称且扩展名为`.ttl`的文件。

以下是场配置文件中配置的功能示例：

```
/cache { 
    /enableTTL "1"
```

>[!NOTE]
>
>请记住，仍需要将AEM配置为发送TTL标头以便Dispatcher遵循这些标头。 切换此功能只会让Dispatcher知道何时删除AEM发送缓存控制标头的文件。 如果AEM未开始发送TTL标头，则Dispatcher不会在此处执行任何特殊的操作。

## 缓存筛选规则

以下是要在发布服务器上缓存其元素的基线配置示例：

```
/cache{ 
    /0000 { 
        /glob "*" 
        /type "allow" 
    } 
    /0001 { 
        /glob "/libs/granite/csrf/token.json" 
        /type "deny" 
    }
```

我们希望使已发布的网站尽可能贪婪，并缓存所有内容。

如果存在在缓存时破坏体验的元素，则可以添加规则以删除缓存该项目的选项。 如上面的示例所示，不应缓存并排除csrf令牌。 有关编写这些规则的更多详细信息可在[此处](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#configuring-the-dispatcher-cache-cache)找到

[下一步 — >使用和了解变量](./variables.md)
