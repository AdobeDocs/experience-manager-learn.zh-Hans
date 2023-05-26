---
title: Dispatcher了解缓存
description: 了解Dispatcher模块如何操作其缓存。
topic: Administration, Performance
version: 6.5
role: Admin
level: Beginner
thumbnail: xx.jpg
exl-id: 66ce0977-1b0d-4a63-a738-8a2021cf0bd5
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '1747'
ht-degree: 1%

---

# 了解缓存

[目录](./overview.md)

[&lt; — 上一页：配置文件说明](./explanation-config-files.md)

本文档将说明Dispatcher缓存的发生方式以及配置方式

## 缓存目录

在我们的基线安装中使用以下默认缓存目录

- 创作
   - `/mnt/var/www/author`
- 发布者
   - `/mnt/var/www/html`

当每个请求遍历Dispatcher时，请求遵循配置的规则以保留本地缓存的版本来响应符合条件的项目

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>注意:</b>

我们刻意将已发布的工作负载与创作工作负载分开，因为当Apache在DocumentRoot中查找文件时，它不知道该文件来自哪个AEM实例。 因此，即使您在作者场中禁用了缓存，如果作者的DocumentRoot与publisher相同，则它将在缓存中提供文件（如果存在）。 这意味着您将从已发布的缓存为提供创作文件，并为您的访客创造非常糟糕的混合匹配体验。

为不同的已发布内容保留单独的DocumentRoot目录也是一个非常糟糕的主意。 您必须创建多个重新缓存的项目，这些项目在clientlibs等站点之间没有区别，还必须为您设置的每个DocumentRoot设置复制刷新代理。 增加每次页面激活时头部的刷新量。 依赖文件的命名空间及其完全缓存的路径，并避免为已发布的站点使用多个DocumentRoot。
</div>

## 配置文件

Dispatcher控制在 `/cache {` 任意场文件的部分。 
在AMS基线配置场中，您会发现我们的includes，如下所示：


```
/cache { 
    /rules { 
        $include "/etc/httpd/conf.dispatcher.d/cache/ams_author_cache.any" 
    }
```


在创建缓存或不缓存内容的规则时，请参阅文档 [此处](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#configuring-the-dispatcher-cache-cache)


## 缓存作者

我们已经看到了许多实施中的用户不会缓存创作内容。 
他们错过了性能和对作者的响应能力的大幅提升。

让我们讨论一下在配置创作场以正确缓存时采取的策略。

这是基本作者 `/cache {` 部分内容：


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

这里需要注意的重要事项是 `/docroot` 设置为author的缓存目录。

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>注意:</b>

确保您的 `DocumentRoot` 在作者的 `.vhost` 文件与场匹配 `/docroot` 参数
</div>

缓存规则include语句包含文件 `/etc/httpd/conf.dispatcher.d/cache/ams_author_cache.any` ，其中包含下列规则：

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
我们有规则要缓存 `/libs` 因为它们是基准AEM安装的一部分，在安装Service Pack、累积修补程序包、升级或修补程序之前可能会发生更改。 因此，缓存这些元素非常合理，并且确实会给使用站点的最终用户的创作体验带来巨大好处。

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>注意:</b>

请记住，这些规则也会缓存 <b>`/apps`</b> 这是自定义应用程序代码所在的位置。 如果您正在此实例上开发代码，则在保存文件时将证明这会非常令人困惑，并且由于文件提供缓存副本，因此不会在UI中反映出来。 这里的意图是，如果您将代码部署到AEM中，那么该过程也不频繁，部署步骤的一部分应该是清除创作缓存。 同样，其好处是使您的可缓存代码能够为最终用户更快地运行。
</div>


## ServeOnStale（又称ServeOnStale/SOS）

这是Dispatcher的一项功能的精华之一。 如果发布者负载不足或变得无响应，它通常会引发502或503 http响应代码。 如果发生这种情况并启用了此功能，将指示Dispatcher尽最大努力仍然提供缓存中任何仍存在的内容，即使它不是新副本也是如此。 如果您已获得某些内容，则最好为其提供服务，而不是只显示一条不提供任何功能的错误消息。

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>注意:</b>

请记住，如果发布者渲染器发生套接字超时或500错误消息，则此功能不会触发。 如果AEM无法访问，则此功能不执行任何操作
</div>

此设置可以在任何场中设置，但只有将其应用于发布场文件时才有意义。 以下是场文件中启用的功能的语法示例：

```
/cache { 
    /serveStaleOnError "1"
```

## 使用查询参数/参数缓存页面

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>注意:</b>

Dispatcher模块的正常行为之一是，如果请求的URI中包含查询参数(通常如下所示 `/content/page.html?myquery=value`)它会跳过文件缓存，直接转到AEM实例。 它将此请求视为动态页面，不应缓存。 这可能会对缓存效率产生不良影响。
</div>
<br/>

查看此 [文章](https://github.com/adobe/aem-dispatcher-optimizer-tool/blob/main/docs/Rules.md#dot---the-dispatcher-publish-farm-cache-should-have-its-ignoreurlparams-rules-configured-in-an-allow-list-manner) 显示重要的查询参数如何影响网站性能。

默认情况下，您要设置 `ignoreUrlParams` 要允许的规则 `*`.  这意味着所有查询参数都将被忽略，并允许缓存所有页面，无论使用什么参数。

以下是一个示例，其中某人建立了一个社交媒体深层链接引用机制，该机制使用URI中的参数引用知道此人来自何处。

<b>可忽略的示例：</b>

- https://www.we-retail.com/home.html?reference=android
- https://www.we-retail.com/home.html?reference=facebook

该页面是100%可缓存的，但是由于参数存在，因此不会缓存。 
配置您的 `ignoreUrlParams` 作为允许列表，可帮助解决此问题：

```
/cache { 
    /ignoreUrlParams { 
        /0001 { /glob "*" /type "allow" }
    }
```

现在，当Dispatcher看到请求时，它将忽略该请求具有 `query` 参数 `?` 引用并且仍缓存页面

<b>动态示例：</b>

- https://www.we-retail.com/search.html?q=fruit
- https://www.we-retail.com/search.html?q=vegetables

请记住，如果您的查询参数使页面发生更改，则它将显示为渲染输出，然后您需要将它们从忽略列表中排除，并使页面再次不可缓存。  例如，使用查询参数的搜索页面会更改渲染的原始html。

因此，以下是每个搜索的html源：

`/search.html?q=fruit`:

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

`/search.html?q=vegetables`:

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

如果您访问了 `/search.html?q=fruit` 首先，它会缓存html，并显示结果。

然后您访问 `/search.html?q=vegetables` 第二，它将显示果实。
这是因为的查询参数 `q` 在缓存方面被忽略。  要避免出现此问题，您需要注意根据查询参数呈现不同HTML的页面，并拒绝对这些页面进行缓存。

示例:

```
/cache { 
    /ignoreUrlParams { 
        /0001 { /glob "*" /type "allow" }
        /0002 { /glob "q" /type "deny"  }
    }
```

通过Javascript使用查询参数的页面仍可完全正常运行，忽略此设置中的参数。  因为它们不会更改静态的html文件。  他们使用javascript在本地浏览器上实时更新浏览器。  这意味着，如果您使用javascript查询参数，则很可能会在页面缓存中忽略此参数。  允许该页面缓存并享受性能提升！

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>注意:</b>

跟踪这些页面确实需要一些维护，但性能提升非常值得。  您可以要求您的CSE对网站流量运行报告，以向您提供过去90天内使用查询参数的所有页面的列表，以便您进行分析并确保您知道要查看的页面以及不能忽略的查询参数
</div>
<br/>

## 缓存响应标头

很明显，Dispatcher会缓存 `.html` pages和clientlibs(即 `.js`， `.css`)，但是您是否知道它还可以将特定的响应标头与同一名称但具有 `.h` 文件扩展名。 这样，不仅可以对内容进行下一个响应，还可以对缓存中应随其一起传递的响应标头进行响应。

AEM可以处理的不只是UTF-8编码

有时，项目具有特殊的标头，可帮助控制缓存TTL的编码详细信息以及上次修改的时间戳。

默认情况下，缓存后的这些值会被清除，Apache httpd Webserver将使用其常规文件处理方法（通常仅限于根据文件扩展名进行MIME类型推测）自行处理资产。

如果您有Dispatcher缓存资产和所需的标头，则可以向客户端浏览器展示适当的体验，并确保所有详细信息都显示。

以下是场示例，其中指定要缓存的标头：

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


在示例中，他们已将AEM配置为提供标头，CDN会查找以了解何时将其缓存失效。 这意味着现在AEM可以根据标头正确指示哪些文件失效。

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>注意:</b>

请记住，不能使用正则表达式或glob匹配。 它是要缓存的标头的文字列表。 仅放入要缓存的文本标头的列表。
</div>


## 自动使宽限期失效

如果作者的AEM系统执行了大量页面激活活动，则可能会出现出现重复无效情况。 重复多次的刷新请求不是必需的，您可以构建一些容差，以便在宽限期清除之前不重复刷新。

### 其工作方式示例：

如果您有5个要失效的请求 `/content/exampleco/en/` 所有这一切都发生在3秒内。

如果关闭此功能，您将使缓存目录失效 `/content/exampleco/en/` 5次

启用此功能并将其设置为5秒后，它将使缓存目录失效 `/content/exampleco/en/` <b>一次</b>

以下是为5秒宽限期配置的此功能的语法示例：

```
/cache { 
    /gracePeriod "5"
```

## 基于TTL的失效

Dispatcher模块的一项新功能是 `Time To Live (TTL)` 基于缓存项目的失效选项。 缓存项目时，它会查找是否存在缓存控制标头，并在缓存目录中生成一个同名和 `.ttl` 扩展。

以下是场配置文件中配置的功能示例：

```
/cache { 
    /enableTTL "1"
```

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>注意：</b>
请记住，仍需要将AEM配置为发送TTL标头，以便Dispatcher遵守这些标头。 切换此功能只会让Dispatcher知道何时删除AEM为其发送缓存控制标头的文件。 如果AEM未开始发送TTL标头，则Dispatcher不会在此处执行任何特殊操作。
</div>

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

如果存在缓存时破坏体验的元素，您可以添加规则以删除缓存该项目的选项。 如上面的示例所示，不应缓存csrf令牌并将其排除。 有关编写这些规则的更多详细信息，请参阅 [此处](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#configuring-the-dispatcher-cache-cache)

[下一步 — >使用和了解变量](./variables.md)
