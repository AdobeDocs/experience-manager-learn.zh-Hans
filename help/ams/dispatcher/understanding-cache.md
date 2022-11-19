---
title: 调度程序了解缓存
description: 了解Dispatcher模块如何运行其缓存。
topic: Administration, Performance
version: 6.5
role: Admin
level: Beginner
thumbnail: xx.jpg
source-git-commit: 04cd4002af7028ee9e3b1e1455b6346c56446245
workflow-type: tm+mt
source-wordcount: '1747'
ht-degree: 1%

---


# 了解缓存

[目录](./overview.md)

[&lt; — 上一个：配置文件说明](./explanation-config-files.md)

本文档将说明Dispatcher缓存的发生方式及其配置方式

## 缓存目录

我们在基线安装中使用以下默认缓存目录

- 创作
   - `/mnt/var/www/author`
- 发布者
   - `/mnt/var/www/html`

当每个请求遍历Dispatcher时，请求将遵循配置的规则，以保留本地缓存的版本以响应符合条件的项目

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>注意:</b>

我们有意将已发布的工作负载与创作工作负载分开，因为当Apache在DocumentRoot中查找文件时，它不知道它来自哪个AEM实例。 因此，即使在作者场中禁用了缓存，如果作者的DocumentRoot与发布者相同，则当存在时，它将从缓存中提供文件。 这意味着您将从已发布的缓存中为提供作者文件，并为访客提供非常糟糕的混合匹配体验。

为不同的已发布内容保留单独的DocumentRoot目录也是一个非常糟糕的主意。 您必须创建多个重新缓存的项目，这些项目在Clientlibs等站点之间没有差异，并且必须为您设置的每个DocumentRoot设置一个复制刷新代理。 在每次页面激活时，增加头部刷新量。 依赖文件的命名空间及其完整缓存路径，并避免发布站点出现多个DocumentRoot。
</div>

## 配置文件

Dispatcher控制在 `/cache {` 任意场文件的部分。 
在AMS基线配置场中，您将看到如下所示的包含：


```
/cache { 
    /rules { 
        $include "/etc/httpd/conf.dispatcher.d/cache/ams_author_cache.any" 
    }
```


在创建用于缓存或不缓存内容的规则时，请参阅此文档 [此处](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#configuring-the-dispatcher-cache-cache)


## 缓存作者

我们看到了许多实施，用户不会缓存创作内容。 
他们在性能和对作者的响应方面缺少大幅提升。

让我们讨论一下配置作者场以正确缓存时所采用的策略。

以下是一位基本作者 `/cache {` 作者场文件的一节：


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

这里需要注意的重要事项是 `/docroot` 设置为创作的缓存目录。

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>注意:</b>

确保 `DocumentRoot` 在作者的 `.vhost` 文件与场匹配 `/docroot` 参数
</div>

缓存规则include语句包含文件 `/etc/httpd/conf.dispatcher.d/cache/ams_author_cache.any` 其中包含以下规则：

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

在创作场景中，内容会随时有目的地发生更改。 您只想缓存不会频繁更改的项目。
我们有缓存规则 `/libs` 因为它们是基准AEM安装的一部分，在您安装了Service Pack、累积修补程序包、升级或修补程序之前，这些组件会发生更改。 因此，缓存这些元素很有意义，并且实际上对使用网站的最终用户的创作体验具有巨大好处。

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>注意:</b>

请记住，这些规则还会缓存 <b>`/apps`</b> 这是自定义应用程序代码所在的位置。 如果您正在此实例上开发代码，那么在保存文件时，如果由于提供了缓存的副本，因此看不到UI中是否反映，会非常混乱。 此处的意图是，如果您将代码部署到AEM中，则部署过程也不会频繁，部分部署步骤应该是清除创作缓存。 同样，其好处是使可缓存代码能够更快地为最终用户运行。
</div>


## ServeOnStale（AKA在旧版/SOS上提供）

这是Dispatcher功能的其中一个功能。 如果发布者处于负载下或变得无响应，则通常会引发502或503 http响应代码。 如果发生这种情况并且启用了此功能，Dispatcher将被指示仍要尽最大努力来提供缓存中仍然存在的内容，即使它不是新副本。 如果您已经获得某些内容，则最好提供该内容，而不是仅显示一条不提供任何功能的错误消息。

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>注意:</b>

请记住，如果发布者渲染器出现套接字超时或500错误消息，则此功能将不会触发。 如果AEM不可访问，则此功能不会执行任何操作
</div>

此设置可以在任何场中设置，但只有将其应用于发布场文件才有意义。 以下是场文件中启用的功能的语法示例：

```
/cache { 
    /serveStaleOnError "1"
```

## 使用查询参数/参数缓存页面

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>注意:</b>

Dispatcher模块的常见行为之一是，如果请求在URI中具有查询参数（通常如下所示） `/content/page.html?myquery=value`)将跳过缓存文件，并直接转到AEM实例。 它将此请求视为动态页面，不应缓存。 这可能会对缓存效率造成不良影响。
</div>
<br/>

请参阅 [文章](https://github.com/adobe/aem-dispatcher-optimizer-tool/blob/main/docs/Rules.md#dot---the-dispatcher-publish-farm-cache-should-have-its-ignoreurlparams-rules-configured-in-an-allow-list-manner) 显示查询参数对网站性能的影响程度。

默认情况下，您要设置 `ignoreUrlParams` 规则允许 `*`.  这意味着将忽略所有查询参数，并允许所有页面缓存，而不考虑使用的参数。

以下示例是，某人构建了社交媒体深层链接引用机制，该机制使用URI中的参数引用来了解该人员的来源。

<b>可忽略示例：</b>

- https://www.we-retail.com/home.html?reference=android
- https://www.we-retail.com/home.html?reference=facebook

页面可100%缓存，但无法缓存，因为参数存在。 
配置 `ignoreUrlParams` 作为允许列表将帮助修复此问题：

```
/cache { 
    /ignoreUrlParams { 
        /0001 { /glob "*" /type "allow" }
    }
```

现在，当Dispatcher看到请求时，将忽略请求具有 `query` 参数 `?` 引用并仍然缓存页面

<b>动态示例：</b>

- https://www.we-retail.com/search.html?q=fruit
- https://www.we-retail.com/search.html?q=vegetables

请记住，如果您的查询参数确实会对页面进行更改，并且该参数会呈现输出，则需要将它们从忽略的列表中排除，并再次取消对页面的缓存。  例如，使用查询参数的搜索页面会更改呈现的原始html。

下面是每个搜索的html源：

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

如果您访问 `/search.html?q=fruit` 然后，它会缓存html，并显示结果。

然后你去 `/search.html?q=vegetables` 第二，它将显示水果的效果。
这是因为 `q` 被忽略。  要避免出现此问题，您需要注意根据查询参数呈现不同HTML的页面，并拒绝这些页面的缓存。

示例:

```
/cache { 
    /ignoreUrlParams { 
        /0001 { /glob "*" /type "allow" }
        /0002 { /glob "q" /type "deny"  }
    }
```

使用通过Javascript查询参数的页面仍将完全忽略此设置中的参数。  因为他们不会更改静态的html文件。  他们使用javascript在本地浏览器上实时更新浏览器dom。  这意味着如果您使用带有javascript的查询参数，则很可能会忽略此参数进行页面缓存。  允许该页面缓存并享受性能提升！

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>注意:</b>

跟踪这些页面确实需要一些维护，但值得获得性能提升。  您可以要求CSE在网站上运行报表流量，以提供一个列表，其中包含过去90天内使用查询参数的所有页面，供您分析和确保知道要查看的页面以及不要忽略的查询参数
</div>
<br/>

## 缓存响应标头

调度程序缓存很明显 `.html` 页面和clientlib(即 `.js`, `.css`)，但您是否知道它还可以将特定响应标头与同名文件(但 `.h` 文件扩展名。 这不仅允许对内容进行下一个响应，还允许从缓存对应的响应标头进行响应。

AEM不仅可以处理UTF-8编码

有时，项目具有特殊的标头，可帮助控制缓存TTL的编码详细信息和上次修改的时间戳。

默认情况下，缓存后会清空这些值，Apache httpd Webserver将使用其常规文件处理方法处理资产，这些方法通常仅限于基于文件扩展名的mime类型猜测。

如果您有Dispatcher缓存资产和所需的标头，则可以显示正确的体验，并确保所有详细信息都能够发送给客户端浏览器。

以下是一个场示例，该场具有指定的要缓存的标头：

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


在示例中，他们将AEM配置为提供CDN要查找的标头，以便知道何时使其缓存失效。 这意味着AEM现在可以根据标头正确指示哪些文件会失效。

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>注意:</b>

请记住，您不能使用正则表达式或全局匹配。 这是要缓存的标头的文字列表。 仅放入要缓存的文字标头列表。
</div>


## 自动失效宽限期

在具有大量来自作者的活动且进行了大量页面激活的AEM系统上，您可能会遇到争用情况，这种情况下会发生重复的无效。 重复刷新请求是不必要的，您可以构建一些容差，以便在宽限期清除之前不重复刷新。

### 其工作原理的示例：

如果您有5个无效请求 `/content/exampleco/en/` 所有都在3秒内发生。

禁用此功能后，将使缓存目录失效 `/content/exampleco/en/` 5次

启用此功能并将其设置为5秒后，将使缓存目录失效 `/content/exampleco/en/` <b>once</b>

以下是将此功能配置为5秒宽限期的示例语法：

```
/cache { 
    /gracePeriod "5"
```

## 基于TTL的失效

调度程序模块的一项新功能是 `Time To Live (TTL)` 基于缓存项目的失效选项。 当某个项目被缓存时，它会查找是否存在缓存控制标头，并在缓存目录中生成一个具有相同名称和 `.ttl` 扩展。

以下是场配置文件中正在配置的功能示例：

```
/cache { 
    /enableTTL "1"
```

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>注意：</b>
请记住，仍需要将AEM配置为发送TTL标头，Dispatcher才能遵守这些标头。 切换此功能仅使Dispatcher能够知道何时删除AEM已为其发送缓存控制标头的文件。 如果AEM不开始发送TTL标头，则Dispatcher不会在此处执行任何特殊操作。
</div>

## 缓存过滤器规则

以下是要在发布者上缓存哪些元素的基线配置示例：

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

我们希望使发布的站点尽可能贪婪，并缓存所有内容。

如果有元素在缓存时会中断体验，则可以添加规则以删除用于缓存该项目的选项。 如上例所示，不应缓存并排除csrf令牌。 有关编写这些规则的更多详细信息，请参阅 [此处](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#configuring-the-dispatcher-cache-cache)

[下一步 — >使用和了解变量](./variables.md)