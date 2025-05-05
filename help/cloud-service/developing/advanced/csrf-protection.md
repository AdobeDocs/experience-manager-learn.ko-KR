---
title: CSRF 보호
description: 인증된 사용자를 위해 AEM에 허용된 POST, PUT 및 Delete 요청에 AEM CSRF 토큰을 생성하고 추가하는 방법에 대해 알아봅니다.
version: Experience Manager as a Cloud Service
feature: Security
topic: Development, Security
role: Developer
level: Intermediate
doc-type: Code Sample
last-substantial-update: 2023-07-14T00:00:00Z
jira: KT-13651
thumbnail: KT-13651.jpeg
exl-id: 747322ed-f01a-48ba-a4a0-483b81f1e904
duration: 125
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '439'
ht-degree: 0%

---

# CSRF 보호

인증된 사용자를 위해 AEM에 허용된 POST, PUT 및 Delete 요청에 AEM CSRF 토큰을 생성하고 추가하는 방법에 대해 알아봅니다.

AEM을 사용하려면 __authenticated__ __POST__, __PUT 또는 __DELETE__ HTTP 요청에 대해 유효한 CSRF 토큰을 AEM Author 및 Publish 서비스로 보내야 합니다.

__GET__ 요청 또는 __익명__ 요청에는 CSRF 토큰이 필요하지 않습니다.

CSRF 토큰이 POST, PUT 또는 DELETE 요청과 함께 전송되지 않으면 AEM이 403 금지 응답을 반환하고 AEM은 다음 오류를 기록합니다.

```log
[INFO][POST /path/to/aem/endpoint HTTP/1.1][com.adobe.granite.csrf.impl.CSRFFilter] isValidRequest: empty CSRF token - rejecting
[INFO][POST /path/to/aem/endpoint HTTP/1.1][com.adobe.granite.csrf.impl.CSRFFilter] doFilter: the provided CSRF token is invalid
```

AEM의 CSRF 보호에 대한 자세한 내용은 [설명서를 참조하십시오](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/csrf-protection.html?lang=ko).


## CSRF 클라이언트 라이브러리

AEM은 핵심 프로토타입 함수 패칭을 통해 CSRF 토큰 XHR 및 양식 POST 요청을 생성하고 추가하는 데 사용할 수 있는 클라이언트 라이브러리를 제공합니다. 이 기능은 `granite.csrf.standalone` 클라이언트 라이브러리 범주에서 제공합니다.

이 방법을 사용하려면 페이지에서 로드되는 클라이언트 라이브러리에 종속성으로 `granite.csrf.standalone`을(를) 추가하십시오. 예를 들어 `wknd.site` 클라이언트 라이브러리 범주를 사용하는 경우 페이지에서 로드되는 클라이언트 라이브러리에 종속성으로 `granite.csrf.standalone`을(를) 추가합니다.

## CSRF 보호를 통한 맞춤형 양식 제출

[`granite.csrf.standalone` 클라이언트 라이브러리](#csrf-client-library)을(를) 사용 사례에 맞게 사용할 수 없는 경우 양식 제출에 CSRF 토큰을 수동으로 추가할 수 있습니다. 다음 예에서는 양식 제출에 CSRF 토큰을 추가하는 방법을 보여줍니다.

이 코드 조각은 양식 제출 시 CSRF 토큰을 AEM에서 가져와서 `:cq_csrf_token`(이)라는 양식 입력에 추가하는 방법을 보여 줍니다. CSRF 토큰은 수명이 짧으므로 양식이 제출되기 직전에 CSRF 토큰을 검색하여 설정하는 것이 가장 좋습니다.

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

## CSRF 보호를 사용하여 가져오기

[`granite.csrf.standalone` 클라이언트 라이브러리](#csrf-client-library)의 사용이 사용 사례에 맞지 않는 경우 CSRF 토큰을 수동으로 XHR에 추가하거나 요청을 가져올 수 있습니다. 다음 예에서는 fetch를 사용하여 만든 XHR에 CSRF 토큰을 추가하는 방법을 보여 줍니다.

이 코드 조각은 AEM에서 CSRF 토큰을 가져와서 가져오기 요청의 `CSRF-Token` HTTP 요청 헤더에 추가하는 방법을 보여 줍니다. CSRF 토큰은 수명이 짧기 때문에 가져오기 요청이 만들어지기 직전에 CSRF 토큰을 검색하고 설정하여 유효성을 확인하는 것이 좋습니다.

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

## Dispatcher 구성

AEM Publish 서비스에서 CSRF 토큰을 사용할 때 Dispatcher에서 CSRF 토큰 엔드포인트에 대한 GET 요청을 허용하도록 구성을 업데이트해야 합니다. 다음 구성을 사용하면 GET에서 AEM Publish 서비스의 CSRF 토큰 엔드포인트에 대한 요청을 수행할 수 있습니다. 이 구성이 추가되지 않으면 CSRF 토큰 엔드포인트가 404 Not Found 응답을 반환합니다.

* `dispatcher/src/conf.dispatcher.d/filters/filters.any`

```
...
/0120 { /type "allow" /method "GET" /url "/libs/granite/csrf/token.json" }
...
```
