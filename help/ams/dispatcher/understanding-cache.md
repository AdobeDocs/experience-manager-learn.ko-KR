---
title: Dispatcher 캐싱 이해
description: Dispatcher 모듈이 캐시를 작동하는 방식을 이해합니다.
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


# 캐싱 이해

[목차](./overview.md)

[&lt;- 이전: 구성 파일에 대한 설명](./explanation-config-files.md)

이 문서에서는 Dispatcher 캐싱이 발생하는 방식과 구성하는 방법을 설명합니다

## 캐시 디렉토리

기준 설치에서 다음과 같은 기본 캐시 디렉토리를 사용합니다

- 작성자
   - `/mnt/var/www/author`
- 게시자
   - `/mnt/var/www/html`

각 요청이 Dispatcher를 순회하면 요청은 구성된 규칙에 따라 적합한 항목의 응답에 로컬로 캐시된 버전을 유지합니다

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>메모:</b>

Apache가 DocumentRoot에서 파일을 찾을 때 게시된 AEM 인스턴스를 알 수 없으므로 게시된 작업 로드를 작성자 작업 로드와 의도적으로 분리시킵니다. 따라서 작성자 팜에서 캐시를 비활성화한 경우에도 작성자의 DocumentRoot가 게시자와 동일한 경우 존재할 때 캐시에서 파일을 제공합니다. 이것은 게시된 캐시에서 작성자 파일을 제공하고 방문자를 위해 매우 끔찍한 혼합 일치 경험을 만드는 것을 의미합니다.

게시된 컨텐츠마다 다른 DocumentRoot 디렉토리를 유지하는 것도 매우 잘못된 생각입니다. clientlibs와 같은 사이트 간에 다르지 않은 여러 개의 다시 캐시된 항목을 만들고 설정한 각 DocumentRoot에 대해 복제 플러시 에이전트를 설정해야 합니다. 각 페이지를 활성화하여 플러시 오버헤드의 양을 증가시킵니다. 파일의 네임스페이스와 전체 캐싱된 경로를 사용하고 게시된 사이트에 대해 여러 DocumentRoot를 사용하지 마십시오.
</div>

## 구성 파일

Dispatcher는에서 캐싱이 가능한 항목을 제어합니다 `/cache {` 팜 파일의 섹션 
AMS 기준 구성 팜에서는 다음과 같이 include를 찾을 수 있습니다.


```
/cache { 
    /rules { 
        $include "/etc/httpd/conf.dispatcher.d/cache/ams_author_cache.any" 
    }
```


캐싱할지 여부에 대한 규칙을 만들 때 설명서를 참조하십시오 [여기](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#configuring-the-dispatcher-cache-cache)


## 작성자 캐싱

사람들이 작성자 콘텐츠를 캐시하지 않는 많은 구현이 있습니다. 
이 팀은 방대한 성능 업그레이드가 없고 작성자에 대한 응답성이 없습니다.

작성자 팜을 제대로 캐시하도록 구성하는 데 사용된 전략에 대해 살펴보겠습니다.

다음은 기본 작성자입니다 `/cache {` 작성자 팜 파일 섹션:


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

여기서 주목할 중요한 것은 `/docroot` 작성자에 대한 캐시 디렉토리로 설정됩니다.

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>메모:</b>

다음 사항을 확인하십시오. `DocumentRoot` 저자의 `.vhost` 파일과 팜 일치 `/docroot` 매개 변수
</div>

캐시 규칙 include 문에 파일이 포함됩니다 `/etc/httpd/conf.dispatcher.d/cache/ams_author_cache.any` 여기에는 다음 규칙이 포함됩니다.

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

작성자 시나리오에서 컨텐츠는 항상 그리고 의도적으로 변경됩니다. 자주 변경되지 않는 항목만 캐시하려고 합니다.
캐싱할 규칙이 있습니다 `/libs` 기준 AEM 설치의 일부이며 서비스 팩, 누적 수정 팩, 업그레이드 또는 핫픽스를 설치할 때까지 변경되므로. 따라서 이러한 요소를 캐싱하는 것은 매우 적절하며 사이트를 사용하는 최종 사용자의 작성자 경험에서 상당한 이점을 얻을 수 있습니다.

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>메모:</b>

이러한 규칙도 캐시된다는 점에 유의하십시오 <b>`/apps`</b> 사용자 지정 애플리케이션 코드가 있는 위치입니다. 이 인스턴스에서 코드를 개발하고 있는 경우 파일을 저장하고 캐시된 복사본을 제공하여 UI에 반영하는지 확인하지 않으면 파일을 저장할 때 매우 혼란스러울 수 있습니다. 즉, 코드를 AEM에 배포하는 경우 드물지만 배포 단계의 일부로 작성자 캐시를 지워야 합니다. 또한 캐시 가능 코드를 최종 사용자가 보다 빠르게 실행할 수 있다는 이점이 있습니다.
</div>


## ServeOnStale(상한/SOS에서 AKA 서비스)

Dispatcher 기능의 이러한 보석 중 하나입니다. 게시자가 로드 중이거나 응답하지 않는 경우 일반적으로 502 또는 503 http 응답 코드가 발생합니다. 이러한 상황이 발생하고 이 기능이 활성화되어 있으면 Dispatcher는 새 복사본이 아닌 경우에도 여전히 캐시에 있는 콘텐츠를 최선을 다해 제공하도록 지시됩니다. 기능을 제공하지 않는 오류 메시지를 표시하는 대신 기능이 있는 경우 제공하는 것이 좋습니다.

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>메모:</b>

게시자 렌더러에 소켓 시간 제한 또는 500 오류 메시지가 있는 경우 이 기능이 트리거되지 않습니다. AEM에 연결할 수 없는 경우 이 기능은 아무 작업도 수행하지 않습니다
</div>

이 설정은 모든 팜에서 설정할 수 있지만 게시 팜 파일에만 적용하는 것이 적절합니다. 다음은 팜 파일에서 활성화된 기능의 구문 예입니다.

```
/cache { 
    /serveStaleOnError "1"
```

## 쿼리 매개 변수/인수를 사용하여 페이지 캐싱

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>메모:</b>

디스패처 모듈의 정상적인 동작 중 하나는 요청이 URI에 있는 쿼리 매개 변수를 포함하는 경우(일반적으로 다음과 같이 표시됨) `/content/page.html?myquery=value`) 파일 캐싱을 건너뛰고 AEM 인스턴스로 바로 이동합니다. 이 요청을 동적 페이지로 간주하므로 캐시하면 안 됩니다. 이로 인해 캐시 효율성에 좋지 않은 영향을 줄 수 있습니다.
</div>
<br/>

다음 보기 [문서](https://github.com/adobe/aem-dispatcher-optimizer-tool/blob/main/docs/Rules.md#dot---the-dispatcher-publish-farm-cache-should-have-its-ignoreurlparams-rules-configured-in-an-allow-list-manner) 사이트 성능에 중요한 쿼리 매개 변수가 미치는 영향을 보여 줍니다.

기본적으로 `ignoreUrlParams` 규칙 허용 `*`.  즉, 모든 쿼리 매개 변수는 무시되고 사용된 매개 변수에 관계없이 모든 페이지를 캐시할 수 있습니다.

다음은 누군가 URI의 인수 참조를 사용하여 사용자가 어디에서 왔는지 아는 소셜 미디어 딥 링크 참조 메커니즘을 구축한 경우의 예입니다.

<b>Ignorable 예:</b>

- https://www.we-retail.com/home.html?reference=android
- https://www.we-retail.com/home.html?reference=facebook

페이지는 100% 캐시할 수 있지만 인수가 있으므로 캐시하지 않습니다. 
구성 `ignoreUrlParams` 허용 목록은 이 문제를 해결하는 데 도움이 됩니다.

```
/cache { 
    /ignoreUrlParams { 
        /0001 { /glob "*" /type "allow" }
    }
```

이제 Dispatcher가 요청을 보면 요청에 가 `query` 매개 변수 `?` 페이지를 참조하고 계속 캐시합니다.

<b>동적 예:</b>

- https://www.we-retail.com/search.html?q=fruit
- https://www.we-retail.com/search.html?q=vegetables

페이지를 변경하는 쿼리 매개 변수가 있으면 렌더링된 출력이 출력됩니다. 이 매개 변수는 무시된 목록에서 제거하고 페이지를 다시 캐시 취소할 수 있도록 해야 합니다.  예를 들어 쿼리 매개 변수를 사용하는 검색 페이지는 렌더링된 원시 html을 변경합니다.

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

방문하신 경우 `/search.html?q=fruit` 먼저 결과를 보고 html을 캐시합니다.

그런 다음 `/search.html?q=vegetables` 둘째, 그것은 과일의 결과를 보여줄 것이다.
이것은 의 쿼리 매개 변수가 `q` 캐싱과 관련하여 무시됩니다.  이 문제를 방지하려면 쿼리 매개 변수에 따라 다른 HTML을 렌더링하고 해당 페이지에 대한 거부 캐싱을 렌더링하는 페이지를 기억해야 합니다.

예:

```
/cache { 
    /ignoreUrlParams { 
        /0001 { /glob "*" /type "allow" }
        /0002 { /glob "q" /type "deny"  }
    }
```

Javascript를 통해 쿼리 매개 변수를 사용하는 페이지는 여전히 이 설정의 매개 변수를 무시하고 완전히 작동합니다.  HTML 파일이 남아 있을 때 변경되지 않으므로  로컬 브라우저에서 javascript를 사용하여 브라우저를 dom 실시간으로 업데이트합니다.  즉, javascript로 쿼리 매개 변수를 사용하는 경우 페이지 캐싱에 대해 이 매개 변수를 무시할 수 있습니다.  이 페이지에서 캐시 및 성능 향상을 즐길 수 있습니다!

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>메모:</b>

이러한 페이지를 추적하려면 약간의 유지 관리가 필요하지만 성능이 향상될 만한 가치가 있습니다.  CSE에서 지난 90일 동안 쿼리 매개 변수를 사용하여 모든 페이지의 목록을 보고 확인할 수 있도록 웹 사이트 트래픽에 대한 보고서를 실행하도록 요청할 수 있습니다. 이를 통해 어떤 페이지를 보고 무시할 수 없는 쿼리 매개 변수를 확인할 수 있습니다
</div>
<br/>

## 응답 헤더 캐싱

Dispatcher가 캐시하는 것은 명백합니다 `.html` 페이지 및 clientlibs(즉, `.js`, `.css`)이지만, 이름이 같지만 `.h` 파일 확장명을 사용합니다. 이렇게 하면 컨텐츠뿐만 아니라 캐시에서 컨텐츠와 함께 사용해야 하는 응답 헤더에 다음 응답이 허용됩니다.

AEM은 UTF-8 인코딩 이외의 인코딩을 처리할 수 있습니다

경우에 따라 항목에는 캐시 TTL의 인코딩 세부 사항 및 마지막으로 수정된 타임스탬프를 제어하는 데 도움이 되는 특수 헤더가 있습니다.

캐싱될 때 이러한 값은 기본적으로 제거되며 Apache httpd 웹 서버는 일반적인 파일 처리 방법으로 자산을 처리하는 자체 작업을 수행합니다. 이 작업은 일반적으로 파일 확장자를 기반으로 하는 MIME 유형 추측으로 제한됩니다.

Dispatcher가 자산 및 원하는 헤더를 캐시하도록 하면 적절한 경험을 제공하고 모든 세부 사항이 클라이언트 브라우저에 표시되도록 할 수 있습니다.

다음은 캐싱할 헤더를 포함하는 팜의 예입니다.

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


이 예에서는 CDN이 캐시를 무효화할 시점을 알기 위해 찾는 헤더를 제공하도록 AEM을 구성했습니다. 이것은 이제 AEM에서 헤더를 기반으로 무효화되는 파일을 적절히 지정할 수 있음을 의미합니다.

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>메모:</b>

정규 표현식이나 glob 일치를 사용할 수 없습니다. 캐싱할 헤더의 리터럴 목록입니다. 캐싱할 리터럴 헤더 목록에만 넣습니다.
</div>


## 유예 기간 자동 무효화

많은 페이지 활동을 수행하는 작성자의 활동이 많은 AEM 시스템에서는 반복 무효화가 발생하는 경합 조건이 있을 수 있습니다. 과도하게 반복되는 플러시 요청은 필요하지 않으며 유예 기간이 지워질 때까지 플러시를 반복하지 않도록 몇 가지 허용치를 작성할 수 있습니다.

### 작동 방법의 예:

무효화 요청이 5개 있는 경우 `/content/exampleco/en/` 모두 3초 이내에 발생합니다.

이 기능을 해제하면 캐시 디렉토리를 무효화합니다 `/content/exampleco/en/` 5배

이 기능을 설정하고 5초로 설정하면 캐시 디렉토리가 무효화됩니다 `/content/exampleco/en/` <b>한 번</b>

다음은 5초 유예 기간 동안 구성되는 이 기능의 구문 예입니다.

```
/cache { 
    /gracePeriod "5"
```

## TTL 기반 무효화

디스패처 모듈의 최신 기능은 다음과 같습니다 `Time To Live (TTL)` 캐시된 항목에 대한 기반 무효화 선택 사항. 항목이 캐싱되면 캐시 제어 헤더가 있는지 확인하고 동일한 이름과 `.ttl` 확장.

다음은 팜 구성 파일에서 구성되는 기능의 예입니다.

```
/cache { 
    /enableTTL "1"
```

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>참고:</b>
Dispatcher가 TTL 헤더를 보내도록 AEM을 구성해야 합니다. 이 기능을 전환하면 Dispatcher가 AEM에서 캐시 제어 헤더를 보내는 파일을 제거할 시기를 알 수 있습니다. AEM에서 TTL 헤더 전송을 시작하지 않으면 Dispatcher가 여기서 특별한 작업을 수행하지 않습니다.
</div>

## 캐시 필터 규칙

다음은 게시자에서 캐싱할 요소를 구성하는 기준 구성의 예입니다.

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

게시된 사이트를 가능한 한 탐욕스럽게 만들고 모든 것을 캐싱하려고 합니다.

캐시 시 경험을 중단시키는 요소가 있는 경우 해당 항목을 캐시하는 옵션을 제거하는 규칙을 추가할 수 있습니다. 위의 예에서 보듯이 csrf 토큰은 캐시되거나 제외되어서는 안 됩니다. 이러한 규칙 작성에 대한 자세한 내용은 [여기](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#configuring-the-dispatcher-cache-cache)

[다음 -> 변수 사용 및 이해](./variables.md)