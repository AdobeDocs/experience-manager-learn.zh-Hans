---
title: CSRF保护
description: 了解如何为经过身份验证的用户生成AEM CSRF令牌并将其添加到允许的POST、PUT和删除请求中AEM。
version: Cloud Service
feature: Security
topic: Development, Security
role: Developer
level: Intermediate
doc-type: Code Sample
last-substantial-update: 2023-07-14T00:00:00Z
jira: KT-13651
thumbnail: KT-13651.jpeg
exl-id: 747322ed-f01a-48ba-a4a0-483b81f1e904
duration: 149
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '439'
ht-degree: 0%

---

# CSRF保护

了解如何为经过身份验证的用户生成AEM CSRF令牌并将其添加到允许的POST、PUT和删除请求中AEM。

AEM需要发送有效的CSRF令牌 __已验证__ __POST__、__PUT或 __DELETE__ 向AEM创作和发布服务发出HTTP请求。

不需要使用CSRF令牌 __GET__ 请求，或 __匿名__ 请求。

如果CSRF令牌未随POST、PUT或DELETE请求一起发送，则AEM会返回403禁止响应，并且AEM会记录以下错误：

```log
[INFO][POST /path/to/aem/endpoint HTTP/1.1][com.adobe.granite.csrf.impl.CSRFFilter] isValidRequest: empty CSRF token - rejecting
[INFO][POST /path/to/aem/endpoint HTTP/1.1][com.adobe.granite.csrf.impl.CSRFFilter] doFilter: the provided CSRF token is invalid
```

请参阅 [有关AEM CSRF保护的更多详细信息的文档](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/csrf-protection.html).


## CSRF客户端库

AEM提供了一个客户端库，该库可用于生成和添加CSRF令牌XHR并通过修补核心原型功能而形成POST请求。 功能由 `granite.csrf.standalone` 客户端库类别。

要使用此方法，请添加 `granite.csrf.standalone` 作为与页面上加载的客户端库之间的依赖关系。 例如，如果您使用 `wknd.site` 客户端库类别，添加 `granite.csrf.standalone` 作为与页面上加载的客户端库之间的依赖关系。

## 具有CSRF保护的自定义表单提交

如果使用 [`granite.csrf.standalone` 客户端库](#csrf-client-library) 不适用于您的用例，您可以手动将CSRF令牌添加到表单提交。 以下示例说明如何向表单提交添加CSRF令牌。

此代码片段演示了在表单提交时，如何从AEM获取CSRF令牌，并将其添加到名为的表单输入中 `:cq_csrf_token`. 由于CSRF令牌的生命周期短，因此最好在提交表单之前立即检索和设置CSRF令牌，以确保其有效性。

```javascript
// Attach submit handler event to form onSubmit
document.querySelector('form').addEventListener('submit', async (event) => {
    event.preventDefault();

    const form = event.target;
    const response = await fetch('/libs/granite/csrf/token.json');
    const json = await response.json();
    
    // Create a form input named ``:cq_csrf_token`` with the CSRF token.
    let csrfTokenInput = form.querySelector('input[name=":cq_csrf_token"]');
    if (!csrfTokenInput?.value) {
        // If the form does not have a CSRF token input, add one.
        form.insertAdjacentHTML('beforeend', `<input type="hidden" name=":cq_csrf_token" value="${json.token}">`);
    } else {
        // If the form already has a CSRF token input, update the value.
        csrfTokenInput.value = json.token;
    }
    // Submit the form with the hidden input containing the CSRF token
    form.submit();
});
```

## 使用CSRF保护获取

如果使用 [`granite.csrf.standalone` 客户端库](#csrf-client-library) 不适用于您的用例，您可以手动将CSRF令牌添加到XHR或获取请求。 以下示例说明如何将CSRF令牌添加到使用fetch生成的XHR中。

此代码片段演示了如何从AEM获取CSRF令牌，并将其添加到获取请求的 `CSRF-Token` HTTP请求标头。 由于CSRF令牌的生命周期较短，因此最好在发出获取请求之前立即检索和设置CSRF令牌，以确保其有效性。

```javascript
/**
 * Get CSRF token from AEM.
 * CSRF token expire after a few minutes, so it is best to get the token before each request.
 * 
 * @returns {Promise<string>} that resolves to the CSRF token.
 */
async function getCsrfToken() {
    const response = await fetch('/libs/granite/csrf/token.json');
    const json = await response.json();
    return json.token;
}

// Fetch from AEM with CSRF token in a header named 'CSRF-Token'.
await fetch('/path/to/aem/endpoint', {
    method: 'POST',
    headers: {
        'CSRF-Token': await getCsrfToken()
    },
});
```

## Dispatcher配置

在AEM Publish服务上使用CSRF令牌时，必须更新Dispatcher配置，以允许向CSRF令牌端点发送GET请求。 以下配置允许向AEM Publish服务上的CSRF令牌端点发出GET请求。 如果未添加此配置，则CSRF令牌端点返回“404未找到”响应。

* `dispatcher/src/conf.dispatcher.d/filters/filters.any`

```
...
/0120 { /type "allow" /method "GET" /url "/libs/granite/csrf/token.json" }
...
```
