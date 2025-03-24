---
title: 如何清除CDN缓存
description: 了解如何从AEM as a Cloud Service的CDN中清除或删除缓存的HTTP响应。
version: Experience Manager as a Cloud Service
feature: Operations, CDN Cache
topic: Administration, Performance
role: Admin, Architect, Developer
level: Intermediate
doc-type: Tutorial
duration: 0
last-substantial-update: 2024-08-13T00:00:00Z
jira: KT-15963
thumbnail: KT-15963.jpeg
exl-id: 5d81f6ee-a7df-470f-84b9-12374c878a1b
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '924'
ht-degree: 0%

---

# 如何清除CDN缓存

了解如何从AEM as a Cloud Service的CDN中清除或删除缓存的HTTP响应。 使用名为&#x200B;**清除API令牌**&#x200B;的自助服务功能，可以清除特定资源、一组资源和整个缓存的缓存。

在本教程中，您将学习如何使用自助服务功能设置和使用Purge API Token清除示例[AEM WKND](https://github.com/adobe/aem-guides-wknd)站点的CDN缓存。

>[!VIDEO](https://video.tv.adobe.com/v/3432948?quality=12&learn=on)

## 缓存失效与显式清除

有两种方法可从CDN中删除缓存的资源：

1. **缓存失效：**&#x200B;它是基于缓存标头（如`Cache-Control`、`Surrogate-Control`或`Expires`）从CDN中删除缓存资源的进程。 缓存标头的`max-age`属性值用于确定资源的缓存生命周期，也称为缓存TTL（生存时间）。 当缓存生命周期过期时，缓存的资源将自动从CDN缓存中删除。

1. **显式清除：**&#x200B;它是在TTL过期之前从CDN缓存中手动删除缓存资源的过程。 显式清除在要立即删除缓存的资源时很有用。 但是，它会增加到源服务器的流量。

当从CDN缓存中删除缓存的资源时，对同一资源的下一个请求将从源服务器获取最新版本。

## 设置清除API令牌

让我们了解如何设置清除API令牌以清除CDN缓存。

### 配置CDN规则

清除API令牌是通过在AEM项目代码中配置CDN规则创建的。

1. 从AEM项目的主`config`文件夹中打开`cdn.yaml`文件。 例如，[WKND项目的cdn.yaml](https://github.com/adobe/aem-guides-wknd/blob/main/config/cdn.yaml)文件。

1. 将以下CDN规则添加到`cdn.yaml`文件：

```yaml
kind: "CDN"
version: "1"
metadata:
  envTypes: ["dev", "stage", "prod"]
data:  
  authentication: # The main authentication configuration
    authenticators: # The list of authenticators
       - name: purge-auth # The name of the authenticator
         type: purge  # The type of the authenticator, must be purge
         purgeKey1: ${{CDN_PURGEKEY_081324}} # The first purge key, must be referenced by the Cloud Manager secret-type environment variable name ${{CDN_EDGEKEY_073124}}
         purgeKey2: ${{CDN_PURGEKEY_111324}} # The second purge key, must be referenced by the Cloud Manager secret-type environment variable name ${{CDN_EDGEKEY_111324}}. It is used for the rotation of secrets without any interruptions.
    rules: # The list of authentication rules
       - name: purge-auth-rule # The name of the rule
         when: { reqProperty: tier, equals: "publish" } # The condition when the rule should be applied
         action: # The action to be taken when the rule is applied
           type: authenticate # The type of the action, must be authenticate
           authenticator: purge-auth # The name of the authenticator to be used, must match the name from the above authenticators list               
```

在上述规则中，`purgeKey1`和`purgeKey2`都是从头开始添加的，以支持密钥的轮换而不会出现任何中断。 但是，在旋转密钥时，您只能从`purgeKey1`开始，以后再添加`purgeKey2`。

1. 保存、提交更改并将更改推送到Adobe上游存储库。

### 创建Cloud Manager环境变量

接下来，创建Cloud Manager环境变量以存储清除API令牌值。

1. 在[my.cloudmanager.adobe.com](https://my.cloudmanager.adobe.com/)登录Cloud Manager并选择您的组织和程序。

1. 在&#x200B;__环境__&#x200B;部分中，单击所需环境旁边的&#x200B;**省略号** (...)，然后选择&#x200B;**查看详细信息**。

   ![查看详细信息](../assets/how-to/view-env-details.png)

1. 然后选择&#x200B;**配置**&#x200B;选项卡并单击&#x200B;**添加配置**&#x200B;按钮。

1. 在&#x200B;**环境配置**&#x200B;对话框中，输入以下详细信息：
   - **名称**：输入环境变量的名称。 它必须匹配`cdn.yaml`文件中的`purgeKey1`或`purgeKey2`值。
   - **值**：输入清除API令牌值。
   - 已应用&#x200B;**服务**：选择&#x200B;**全部**&#x200B;选项。
   - **类型**：选择&#x200B;**密钥**&#x200B;选项。
   - 单击&#x200B;**添加**&#x200B;按钮。

   ![添加变量](../assets/how-to/add-cloud-manager-secrete-variable.png)

1. 重复上述步骤为`purgeKey2`值创建第二个环境变量。

1. 单击&#x200B;**保存**&#x200B;以保存并应用更改。

### 部署CDN规则

最后，使用Cloud Manager管道将配置的CDN规则部署到AEM as a Cloud Service环境。

1. 在Cloud Manager中，导航到&#x200B;**管道**&#x200B;部分。

1. 创建新管道或选择仅部署&#x200B;**Config**&#x200B;文件的现有管道。 有关详细步骤，请参阅[创建配置管道](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/how-to-setup#deploy-rules-through-cloud-manager)。

1. 单击&#x200B;**运行**&#x200B;按钮以部署CDN规则。

   ![运行管道](../assets/how-to/run-config-pipeline.png)

## 使用清除API令牌

要清除CDN缓存，请使用清除API令牌调用特定于AEM服务的域URL。 清除缓存的语法如下：

```
PURGE <URL> HTTP/1.1
Host: <AEM_SERVICE_SPECIFIC_DOMAIN>
X-AEM-Purge-Key: <PURGE_API_TOKEN>
X-AEM-Purge: <PURGE_TYPE>
Surrogate-Key: <SURROGATE_KEY>
```

其中：

- **清除`<URL>`**： `PURGE`方法后跟要清除的资源的URL路径。
- **主机：`<AEM_SERVICE_SPECIFIC_DOMAIN>`**：它指定AEM服务的域。
- **X-AEM-Purge-Key：`<PURGE_API_TOKEN>`**：包含清除API令牌值的自定义标头。
- **X-AEM-Purge：`<PURGE_TYPE>`**：指定清除操作类型的自定义标头。 该值可以是`hard`、`soft`或`all`。 下表描述了每种清除类型：

  | 清除类型 | 描述 |
  |:------------:|:-------------:|
  | hard（默认） | 立即删除缓存的资源。 避免使用它，因为它会增加到源服务器的流量。 |
  | 柔光 | 将缓存的资源标记为已过时，并从源服务器中获取最新版本。 |
  | 全部 | 从CDN缓存中删除所有缓存的资源。 |

- **Surrogate-Key：`<SURROGATE_KEY>`**： （可选）指定要清除的资源组的替代键（以空格分隔）的自定义标头。 代理密钥用于将资源分组在一起，并且必须在资源的响应标头中进行设置。

>[!TIP]
>
>在以下示例中，`X-AEM-Purge: hard`用于演示目的。 您可以根据自己的要求将其替换为`soft`或`all`。 使用`hard`清除类型时，请务必小心，因为它会增加到源服务器的流量。

### 清除特定资源的缓存

在此示例中，`curl`命令为部署在AEM as a Cloud Service环境中的WKND站点上的`/us/en.html`资源清除缓存。

```bash
curl -X PURGE "https://publish-p46652-e1315806.adobeaemcloud.com/us/en.html" \
-H "X-AEM-Purge-Key: 123456789" \
-H "X-AEM-Purge: hard"
```

成功清除后，将返回包含JSON内容的`200 OK`响应。

```json
{ "status": "ok", "id": "1000098-1722961031-13237063" }
```

### 清除一组资源的缓存

在此示例中，`curl`命令将清除代理项为`wknd-assets`的资源组的缓存。 `Surrogate-Key`响应标头在[`wknd.vhost`](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.d/available_vhosts/wknd.vhost#L176)中设置，例如：

```http
<VirtualHost *:80>
    ...

    # Core Component Image Component: long-term caching (30 days) for immutable URLs, background refresh to avoid MISS
    <LocationMatch "^/content/.*\.coreimg.*\.(?i:jpe?g|png|gif|svg)$">
        Header set Cache-Control "max-age=2592000,stale-while-revalidate=43200,stale-if-error=43200,public,immutable" "expr=%{REQUEST_STATUS} < 400"
        # Set Surrogate-Key header to group the cache of WKND assets, thus it can be flushed independtly
        Header set Surrogate-Key "wknd-assets"
        Header set Age 0
    </LocationMatch>

    ...
</VirtualHost>
```

```bash
curl -X PURGE "https://publish-p46652-e1315806.adobeaemcloud.com" \
-H "Surrogate-Key: wknd-assets" \
-H "X-AEM-Purge-Key: 123456789" \
-H "X-AEM-Purge: hard"
```

成功清除后，将返回包含JSON内容的`200 OK`响应。

```json
{ "wknd-assets": "10027-1723478994-2597809-1" }
```

### 清除整个缓存

在此示例中，使用`curl`命令从部署在AEM as a Cloud Service环境中的示例WKND站点中清除整个缓存。

```bash
curl -X PURGE "https://publish-p46652-e1315806.adobeaemcloud.com/" \
-H "X-AEM-Purge-Key: 123456789" \
-H "X-AEM-Purge: all"
```

成功清除后，将返回包含JSON内容的`200 OK`响应。

```json
{"status":"ok"}
```

### 验证缓存清除

要验证缓存清除，请访问Web浏览器中的资源URL并查看响应标头。 `X-Cache`标头值应为`MISS`。

![X缓存标头](../assets/how-to/x-cache-miss.png)
