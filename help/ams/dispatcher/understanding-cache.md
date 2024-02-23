---
title: Dispatcher 캐싱 이해
description: Dispatcher 모듈이 캐시를 작동하는 방식을 이해합니다.
topic: Administration, Performance
version: 6.5
role: Admin
level: Beginner
thumbnail: xx.jpg
doc-type: Article
exl-id: 66ce0977-1b0d-4a63-a738-8a2021cf0bd5
duration: 491
source-git-commit: 19beb662b63476f4745291338d944502971638a3
workflow-type: tm+mt
source-wordcount: '1708'
ht-degree: 0%

---

# 캐싱 이해

[목차](./overview.md)

[&lt;- 이전: 구성 파일에 대한 설명](./explanation-config-files.md)

이 문서에서는 Dispatcher 캐싱 발생 방법 및 구성 방법을 설명합니다

## 캐싱 디렉터리

기본 설치에서는 다음과 같은 기본 캐시 디렉터리를 사용합니다

- 작성자
   - `/mnt/var/www/author`
- 게시자
   - `/mnt/var/www/html`

각 요청이 Dispatcher를 통과할 때 요청은 구성된 규칙을 따라 적격 항목의 응답에 로컬로 캐시된 버전을 유지합니다

>[!NOTE]
>
>Apache가 DocumentRoot에서 파일을 찾을 때 해당 파일이 어느 AEM 인스턴스에서 발생했는지 알 수 없기 때문에 의도적으로 게시된 워크로드를 작성자 워크로드와 분리시킵니다. 따라서 작성자 팜에서 캐시를 비활성화한 경우에도 작성자의 DocumentRoot가 게시자와 동일하면 존재하는 경우 캐시에서 파일을 제공합니다. 즉, 게시된 캐시에서에 대한 작성자 파일을 제공하고 방문자를 위한 매우 끔찍한 믹스 매치 경험을 만듭니다.
>
>서로 다른 게시된 콘텐츠에 대해 별도의 DocumentRoot 디렉터리를 유지하는 것도 매우 잘못된 생각입니다. Clientlib과 같이 사이트마다 다르지 않고 재캐시된 항목을 여러 개 만들어야 하며 설정하는 각 DocumentRoot에 대해 복제 플러시 에이전트를 설정해야 합니다. 각 페이지 활성화로 헤드 오버의 플러시 양을 늘립니다. 파일 네임스페이스 및 전체 캐시 경로를 사용하고 게시된 사이트에 대해 여러 DocumentRoot를 사용하지 마십시오.

## 구성 파일

Dispatcher는에서 캐시 가능한 대상을 제어합니다. `/cache {` 모든 팜 파일의 섹션에 있는 섹션을 참조하십시오. 
AMS 기본 구성 팜에서는 아래와 같이 의 포함을 찾을 수 있습니다.


```
/cache { 
    /rules { 
        $include "/etc/httpd/conf.dispatcher.d/cache/ams_author_cache.any" 
    }
```


캐시할 내용 또는 캐시하지 않을 내용에 대한 규칙을 생성할 때 설명서를 참조하십시오 [여기](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#configuring-the-dispatcher-cache-cache)


## 캐싱 작성자

사람들이 작성자 콘텐츠를 캐시하지 않는 곳에서 확인한 구현은 많습니다. 
이들은 작성자에 대한 성능과 응답성에 있어서 엄청난 업그레이드를 놓치고 있습니다.

제대로 캐시하기 위해 작성자 팜을 구성하는 전략에 대해 살펴보겠습니다.

기본 작성자입니다. `/cache {` 작성자 팜 파일의 섹션:


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

여기서 주목할 중요한 사항은 `/docroot` 작성자의 캐시 디렉터리로 설정됩니다.

>[!NOTE]
>
>다음을 확인하십시오. `DocumentRoot` 작성자의 `.vhost` 파일이 팜과 일치함 `/docroot` 매개 변수

캐시 규칙에는 파일이 포함됩니다 `/etc/httpd/conf.dispatcher.d/cache/ams_author_cache.any` 에는 다음 규칙이 포함됩니다.

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

작성자 시나리오에서 콘텐츠는 항상 그리고 의도적으로 변경됩니다. 자주 변경되지 않는 항목만 캐시합니다.
캐시할 규칙이 있습니다. `/libs` 기본 AEM 설치의 일부이며 서비스 팩, 누적 수정 팩, 업그레이드 또는 핫픽스를 설치하기 전까지 변경됩니다. 따라서 이러한 요소를 캐시하는 것은 매우 적절하며, 실제로 사이트를 사용하는 최종 사용자의 작성자 경험의 큰 이점이 있습니다.

>[!NOTE]
>
>이러한 규칙은 캐시됩니다 <b>`/apps`</b> 사용자 정의 애플리케이션 코드가 있는 위치입니다. 이 인스턴스에서 코드를 개발 중인 경우 파일을 저장할 때 매우 혼란스러우며 캐시된 복사본을 제공하기 때문에 UI에 가 반영되었는지 표시되지 않습니다. 여기서의 의도는 AEM에 코드를 배포하는 경우 너무 자주 배포되지 않으며 배포 단계의 일부가 작성자 캐시를 지우는 것입니다. 캐시 가능한 코드를 최종 사용자가 더 빨리 실행할 수 있다는 이점이 큽니다.

## ServeOnStale(예: Stale/SOS에 제공)

이는 Dispatcher 기능의 보석 중 하나입니다. 게시자가 로드 중이거나 응답하지 않으면 일반적으로 502 또는 503 http 응답 코드가 발생합니다. 이 경우 이 기능이 활성화되면 Dispatcher는 새 복사본이 아니더라도 최대한 노력하여 캐시에 있는 콘텐츠를 계속 제공하도록 지침을 받게 됩니다. 기능을 제공하지 않는 오류 메시지를 표시하는 것보다 기능이 있는 경우 제공하는 것이 좋습니다.

>[!NOTE]
>
>게시자 렌더러에서 소켓 시간 제한 또는 500 오류 메시지가 있는 경우 이 기능이 트리거되지 않습니다. AEM에 연결할 수 없는 경우 이 기능은 아무 작업도 하지 않습니다

이 설정은 모든 팜에서 설정할 수 있지만 게시 팜 파일에 적용하는 것이 적절합니다. 다음은 팜 파일에서 활성화된 기능의 구문 예입니다.

```
/cache { 
    /serveStaleOnError "1"
```

## 쿼리 매개 변수/인수를 사용하여 페이지 캐싱

>[!NOTE]
>
>Dispatcher 모듈의 일반적인 동작 중 하나는 요청에 URI(일반적으로 다음과 같이 표시됨)에 쿼리 매개 변수가 있는 경우입니다 `/content/page.html?myquery=value`) 파일 캐싱을 건너뛰고 AEM 인스턴스로 바로 이동합니다. 이 요청을 동적 페이지로 간주하므로 캐시해서는 안 됩니다. 이로 인해 캐시 효율성에 좋지 않은 영향을 미칠 수 있습니다.

이 항목 보기 [기사](https://github.com/adobe/aem-dispatcher-optimizer-tool/blob/main/docs/Rules.md#dot---the-dispatcher-publish-farm-cache-should-have-its-ignoreurlparams-rules-configured-in-an-allow-list-manner) 쿼리 매개 변수가 사이트 성능에 미치는 영향을 보여 줍니다.

기본적으로 `ignoreUrlParams` 허용할 규칙 `*`.  즉, 모든 쿼리 매개 변수는 무시되며 사용된 매개 변수에 관계없이 모든 페이지를 캐시할 수 있습니다.

다음은 누군가가 URI의 인수 참조를 사용하여 그 사람이 어디에서 왔는지를 아는 소셜 미디어 딥링크 참조 메커니즘을 구축한 예입니다.

*무시 가능한 예:*

- https://www.we-retail.com/home.html?reference=android
- https://www.we-retail.com/home.html?reference=facebook

페이지는 100% 캐시할 수 있지만 인수가 있으므로 캐시하지 않습니다. 
구성 `ignoreUrlParams` as a 허용 목록은 이 문제를 해결하는 데 도움이 됩니다.

```
/cache { 
    /ignoreUrlParams { 
        /0001 { /glob "*" /type "allow" }
    }
```

이제 Dispatcher가 요청을 볼 때 요청에 가 있다는 사실을 무시합니다. `query` 매개 변수 `?` 페이지를 참조하고 계속 캐시

<b>동적 예:</b>

- https://www.we-retail.com/search.html?q=fruit
- https://www.we-retail.com/search.html?q=vegetables

페이지를 변경하는 쿼리 매개 변수가 있으면 이 매개 변수가 렌더링된 출력이라는 점을 기억하십시오. 그러면 무시된 목록에서 해당 매개 변수를 제외하고 페이지를 다시 캐시할 수 없도록 만들어야 합니다.  예를 들어 쿼리 매개 변수를 사용하는 검색 페이지는 렌더링된 원시 html을 변경합니다.

다음은 각 검색의 html 소스입니다.

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

다음을 방문했다면 `/search.html?q=fruit` 그런 다음 fruit를 표시하는 결과와 함께 html을 캐시합니다.

그리고 다음을 방문합니다. `/search.html?q=vegetables` 두 번째지만, 그것은 과일의 결과를 보여줄 것이다.
이는 의 쿼리 매개 변수 `q` 캐싱과 관련하여 이(가) 무시됩니다.  이 문제를 방지하려면 쿼리 매개 변수를 기반으로 다른 HTML을 렌더링하는 페이지를 기록하고 해당 페이지에 대한 캐싱을 거부해야 합니다.

예:

```
/cache { 
    /ignoreUrlParams { 
        /0001 { /glob "*" /type "allow" }
        /0002 { /glob "q" /type "deny"  }
    }
```

Javascript를 통해 쿼리 매개 변수를 사용하는 페이지는 이 설정의 매개 변수를 무시하면서 여전히 완전히 작동합니다.  휴지통에서 HTML 파일을 변경하지 않기 때문입니다.  로컬 브라우저에서 JavaScript를 사용하여 실시간으로 브라우저를 업데이트합니다.  즉, Javascript와 함께 쿼리 매개 변수를 사용하는 경우 페이지 캐싱에 대해 이 매개 변수를 무시할 수 있습니다.  해당 페이지를 캐시하여 성능 향상을 누릴 수 있습니다.

>[!NOTE]
>
>이러한 페이지를 추적하려면 유지가 필요하지만 성능을 향상시킬 가치가 있습니다.  CSE에 웹 사이트 트래픽에 대한 보고서를 실행하여 지난 90일 동안 쿼리 매개변수를 사용하여 분석할 모든 페이지 목록과 무시하지 말아야 할 쿼리 매개변수를 알 수 있도록 할 수 있습니다

## 응답 헤더 캐싱

Dispatcher가 캐시하는 것은 거의 확실합니다 `.html` 페이지 및 clientlib(예: `.js`, `.css`)에 포함되지만, 이름이 같지만 `.h` 파일 확장명. 이렇게 하면 콘텐츠뿐만 아니라 캐시에서 이 콘텐츠와 함께 제공되어야 하는 응답 헤더에 다음 응답이 가능합니다.

AEM은 UTF-8 인코딩 이상을 처리할 수 있습니다.

경우에 따라 항목에는 캐시 TTL의 인코딩 세부 사항과 마지막으로 수정된 타임스탬프를 제어하는 데 도움이 되는 특수 헤더가 있습니다.

캐시된 경우 이러한 값은 기본적으로 제거되며 Apache httpd 웹 서버는 일반적인 파일 처리 방법으로 자산을 처리하는 자체 작업을 수행합니다. 이는 일반적으로 파일 확장명에 따른 MIME 유형 추측으로 제한됩니다.

Dispatcher가 에셋 및 원하는 헤더를 캐시하도록 한 경우 적절한 경험을 노출하고 모든 세부 정보가 클라이언트 브라우저에 표시되도록 할 수 있습니다.

다음은 캐시할 헤더가 지정된 팜의 예입니다.

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


이 예제에서는 CDN이 캐시를 무효화할 시기를 알 수 있도록 헤더를 제공하도록 AEM을 구성했습니다. 즉, 이제 AEM에서 헤더에 따라 무효화되는 파일을 적절히 지시할 수 있습니다.

>[!NOTE]
>
>정규 표현식이나 glob 일치를 사용할 수 없습니다. 캐시할 머리글의 리터럴 목록입니다. 캐시할 리터럴 헤더 목록에만 넣습니다.

## 유예 기간 자동 무효화

페이지 활성화가 많은 작성자의 활동이 많은 AEM 시스템에서는 반복 무효화가 발생하는 경합 조건이 있을 수 있습니다. 많이 반복되는 플러시 요청은 필요하지 않으며 유예 기간이 지워질 때까지 플러시를 반복하지 않도록 일부 허용치를 빌드할 수 있습니다.

### 작동 방식의 예:

무효화할 요청이 5개 있는 경우 `/content/exampleco/en/` 모든 일은 3초 이내에 일어난다.

이 기능을 사용하면 캐시 디렉토리를 무효화할 수 있습니다 `/content/exampleco/en/` 5회

이 기능을 켜고 5초로 설정하면 캐시 디렉터리가 무효화됩니다. `/content/exampleco/en/` <b>한 번</b>

다음은 5초 유예 기간 동안 구성되는 이 기능의 구문 예제입니다.

```
/cache { 
    /gracePeriod "5"
```

## TTL 기반 무효화

Dispatcher 모듈의 새로운 기능은 다음과 같습니다. `Time To Live (TTL)` 캐시된 항목에 대한 기반 무효화 옵션. 항목이 캐시되면 캐시 제어 헤더가 있는지 검색하고 캐시 디렉터리에 동일한 이름과 `.ttl` 확장명.

다음은 팜 구성 파일에 구성된 기능의 예입니다.

```
/cache { 
    /enableTTL "1"
```

>[!NOTE]
>
>Dispatcher가 헤더를 준수하도록 TTL 헤더를 보내도록 AEM을 계속 구성해야 합니다. 이 기능을 전환하면 Dispatcher는 AEM에서 송신 캐시 제어 헤더가 있는 파일을 제거할 시기만 알 수 있습니다. AEM이 TTL 헤더 전송을 시작하지 않으면 Dispatcher는 여기서 특별한 작업을 수행하지 않습니다.

## 캐시 필터 규칙

다음은 게시자에서 캐시할 요소에 대한 기준 구성의 예입니다.

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

게시된 사이트를 가능한 한 탐욕스럽게 만들고 모든 것을 캐시하려고 합니다.

캐시될 때 경험을 중단하는 요소가 있는 경우 규칙을 추가하여 해당 항목을 캐시하는 옵션을 제거할 수 있습니다. 위의 예에서 보듯이 csrf 토큰은 캐시되어서는 안 되며 제외되었습니다. 이러한 규칙 작성에 대한 자세한 내용은 을 참조하십시오 [여기](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#configuring-the-dispatcher-cache-cache)

[다음 -> 변수 사용 및 이해](./variables.md)
