---
title: AEM Dispatcher 별칭 URL 기능
description: 콘텐츠를 게재의 가장자리에 더 가깝게 매핑하기 위해 rewrite 규칙을 사용하여 AEM에서 별칭 URL 및 추가 기술을 처리하는 방법을 이해합니다.
version: 6.5
topic: Administration, Performance
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
source-git-commit: 04cd4002af7028ee9e3b1e1455b6346c56446245
workflow-type: tm+mt
source-wordcount: '994'
ht-degree: 4%

---


# 디스패처 별칭 URL

[목차](./overview.md)

[&lt;- 이전: Dispatcher 플러시](./disp-flushing.md)

## 개요

이 문서는 콘텐츠를 게재 가장자리에 더 가깝게 매핑하기 위해 rewrite 규칙을 사용하여 AEM에서 별칭 url 및 일부 추가 기술을 처리하는 방법을 이해하는 데 도움이 됩니다

## 별칭 URL이란?

의미가 있는 폴더 구조에 들어 있는 컨텐츠가 있는 경우 참조하기 쉬운 URL에 항상 포함되는 것은 아닙니다.  별칭 URL은 바로 가기와 같습니다.  실제 컨텐츠가 상주하는 위치를 참조하는 짧거나 고유한 URL입니다.

예: `/aboutus` 가리키기 `/content/we-retail/us/en/about-us.html`

AEM 작성자는 AEM의 컨텐츠에 대한 별칭 url 속성을 설정하고 게시할 수 있는 옵션이 있습니다.

이 기능이 작동하려면 Dispatcher 필터를 조정하여 별칭 을 통과해야 합니다.  따라서 작성자가 이러한 별칭 페이지 항목을 설정하는 데 필요한 비율로 Dispatcher 구성 파일을 조정하는 것과 함께 하는 것은 비합리적입니다.

이러한 이유로 Dispatcher 모듈에는 컨텐츠 트리에 별칭 목록에 있는 모든 것을 자동으로 허용하는 기능이 있습니다.


## 작동 방법

### 별칭 URL 작성

작성자는 AEM에서 페이지를 방문하여 페이지 속성을 방문하여 별칭 url 섹션에 의 항목을 추가합니다.

변경 사항을 저장하고 페이지를 활성화하면 이제 이 페이지에 대한 별칭 이 할당됩니다.

#### Touch UI:

![사이트 편집기 화면의 AEM 작성 UI에 대한 드롭다운 대화 상자 메뉴](assets/disp-vanity-url/aem-page-properties-drop-down.png "aem-page-properties-드롭다운")

![aem 페이지 속성 대화 상자 페이지](assets/disp-vanity-url/aem-page-properties.png "aem-page-properties")

#### 클래식 컨텐츠 파인더:

![AEM siteadmin classic ui 사이드 킥페이지 속성](assets/disp-vanity-url/aem-page-properties-sidekick.png "aem-page-properties-sidekick")

![클래식 UI 페이지 속성 대화 상자](assets/disp-vanity-url/aem-page-properties-classic.png "aem-page-properties-classic")

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>참고:</b>
이는 공간 문제에 대해 언급하기 매우 쉽습니다.

별칭 항목은 모든 페이지에 전반적입니다. 이는 나중에 몇 가지 해결 방법을 계획해야 하는 단점 중 하나입니다.
</div>

## 자원 해결/매핑

각 별칭 항목은 내부 리디렉션을 위한 sling 맵 항목입니다.

이러한 맵은 AEM 인스턴스 Felix 콘솔을 방문하여 볼 수 있습니다( `/system/console/jcrresolver` )

다음은 별칭 항목으로 만든 맵 항목의 스크린샷입니다.
![리소스에서 별칭 항목의 콘솔 스크린샷 규칙 해결](assets/disp-vanity-url/vanity-resource-resolver-entry.png "vanity-resource-resolver-entry")

위의 예에서 AEM 인스턴스에 방문을 요청할 때 `/aboutus` 다음 방법으로 `/content/we-retail/us/en/about-us.html`

## Dispatcher 자동 허용 필터

보안 상태의 Dispatcher는 경로에서 요청을 필터링합니다 `/` 이것이 JCR 트리의 루트이므로 Dispatcher를 통해 전송됩니다.

게시자가 의 콘텐츠만 허용하는지 확인하는 것이 중요합니다 `/content` 및 기타 안전한 경로 등  다음과 같은 경로가 아님 `/system` 등.

다음은 의 기본 폴더에 있는 rub, vanity url입니다 `/` 그렇다면 우리는 어떻게 그들이 안전한 상태를 유지하면서 출판사들에게 연락할 수 있을까요?

단순 Dispatcher에는 자동 필터 허용 메커니즘이 있으므로 AEM 패키지를 설치한 다음 해당 패키지 페이지를 가리키도록 Dispatcher를 구성해야 합니다.

[https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html?package=/content/software-distribution/en/details.html/content/dam/aem/public/adobe/packages/granite/vanityurls-components](https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html?package=/content/software-distribution/en/details.html/content/dam/aem/public/adobe/packages/granite/vanityurls-components)

Dispatcher의 팜 파일에 구성 섹션이 있습니다.

```
/vanity_urls { 
    /url    "/libs/granite/dispatcher/content/vanityUrls.html" 
    /file   "/tmp/vanity_urls" 
    /delay  300 
}
```

이 구성은 Dispatcher에 300초마다 최전방에서 이 URL을 가져와 허용하려는 항목 목록을 가져오도록 지시합니다.

응답의 캐시를 `/file` 이 예제와 같이 인수를 지정합니다. `/tmp/vanity_urls`

따라서 URI에서 AEM 인스턴스를 방문하면 이 인스턴스가 가져오는 내용을 보게 됩니다.
![/libs/granite/dispatcher/content/vanityUrls.html에서 렌더링된 컨텐츠의 스크린샷](assets/disp-vanity-url/vanity-url-component.png "vanity-url-component")

말 그대로 아주 단순한 목록이죠

## 규칙을 별칭 규칙으로 다시 작성

위에서 설명한 대로 AEM에 빌드된 기본 메커니즘 대신 rewrite 규칙을 사용하는 것을 언급하는 이유는 무엇입니까?

간단히 설명하면, 더 잘 처리할 수 있는 네임스페이스 문제, 성능 및 상위 수준 로직입니다.

별칭 항목의 예를 살펴보겠습니다 `/aboutus` 콘텐츠 `/content/we-retail/us/en/about-us.html` apache 사용 `mod_rewrite` 이러한 작업을 수행하는 모듈입니다.

```
RewriteRule ^/aboutus /content/we-retail/us/en/about-us.html [PT,L,NC]
```

이 규칙은 허영심을 찾을 것이다 `/aboutus` PT 플래그(통과)를 사용하여 렌더러에서 전체 경로를 가져옵니다.

또한 다른 모든 규칙 L 플래그(마지막) 처리를 중단할 것이며, 이는 JCR Resolving이 수행해야 하는 것과 같은 큰 규칙 목록을 트래버스하지 않아도 된다는 것을 의미합니다.

요청을 프록시할 필요가 없고 AEM 게시자가 이 두 가지 방법의 요소에 응답할 때까지 기다리면 성능이 더욱 향상됩니다.

그러면 여기서 케이크에 있는 아이싱은 고객이 URI를 `/AboutUs` 대신 `/aboutus` 여전히 작동하며 올바른 페이지를 가져올 수 있도록 허용합니다.

재작성 규칙을 만들어 만들려면 Dispatcher에 구성 파일을 만듭니다(예: `/etc/httpd/conf.d/rewrites/examplevanity_rewrite.rules`)에 포함하고 `.vhost` 이 별칭 url을 적용해야 하는 도메인을 처리하는 파일입니다.

다음은 내부에 include의 코드 조각입니다 `/etc/httpd/conf.d/enabled_vhosts/we-retail.vhost`

```
<VirtualHost *:80> 
 ServerName weretail.com 
 ServerAlias www.weretail.com 
        ........ SNIP ........ 
 <IfModule mod_rewrite.c> 
  ReWriteEngine on 
  LogLevel warn rewrite:info 
  Include /etc/httpd/conf.d/rewrites/examplevanity_rewrite.rules 
 </IfModule> 
        ........ SNIP ........ 
</VirtualHost>
```

## 어떤 방법 및 위치

AEM을 사용하여 별칭 항목을 제어하는 경우 다음과 같은 이점이 있습니다
- 작성자가 즉시 만들 수 있습니다
- 이러한 수정 사항은 컨텐츠와 함께 저장되며 컨텐츠와 함께 패키지화할 수 있습니다

사용 `mod_rewrite` 별칭 항목을 제어하려면 다음과 같은 이점이 있습니다.
- 컨텐츠 해결 속도 향상
- 최종 사용자 컨텐츠 요청의 끝에 근접함
- 더 많은 확장성 및 옵션을 사용하여 컨텐츠가 다른 조건에서 매핑되는 방식을 제어합니다
- 대/소문자를 구분하지 않을 수 있습니다.

두 가지 방법을 모두 사용하되, 다음 경우에 사용할 조언 및 기준은 다음과 같습니다.
- 별칭 이 일시적이며 계획된 트래픽 수준이 낮은 경우 AEM 기본 제공 기능을 사용하십시오
- 자주 변경되지 않고 자주 사용하는 POI가 기본 엔드포인트인 경우 `mod_rewrite` 규칙.
- vanity 네임스페이스가 있는 경우(예: `/aboutus`)를 동일한 AEM 인스턴스의 많은 브랜드에 대해 재사용한 다음 다시 작성 규칙을 사용해야 합니다.

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>메모:</b>

AEM 별칭 기능을 사용하고 네임스페이스를 사용하지 않으려면 이름 지정 규칙을 만들 수 있습니다.  다음과 같이 중첩된 별칭 url 사용 `/brand1/aboutus`, `brand2/aboutus`, `brand3/aboutus`.
</div>

[다음 -> 일반 로깅](./common-logs.md)