---
title: AEM Dispatcher vanity URL 기능
description: 콘텐츠를 게재 가장자리에 더 가깝게 매핑하기 위해 AEM이 규칙 다시 작성을 사용하여 vanity URL 및 추가 기술을 처리하는 방법을 이해합니다.
version: Experience Manager 6.5
topic: Administration, Performance
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
doc-type: Article
exl-id: 53baef9c-aa4e-4f18-ab30-ef9f4f5513ee
duration: 244
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1159'
ht-degree: 0%

---

# Dispatcher 별칭 URL

[목차](./overview.md)

[&lt;- 이전: Dispatcher 플러싱](./disp-flushing.md)

## 개요

이 문서는 AEM이 규칙 다시 작성을 사용하여 콘텐츠를 게재 가장자리에 더 가깝게 매핑하여 단축 URL 및 일부 추가 기법을 처리하는 방법을 이해하는 데 도움이 됩니다

## 단축 URL 소개

의미가 있는 폴더 구조에 들어 있는 컨텐츠가 있는 경우 참조하기 쉬운 URL에 항상 존재하는 것은 아닙니다. 단축 URL은 단축키와 유사합니다. 실제 컨텐츠가 존재하는 위치를 참조하는 더 짧거나 고유한 URL입니다.

예: `/content/we-retail/us/en/about-us.html`에 지정된 `/aboutus`

AEM 작성자에게는 AEM의 컨텐츠 일부에 대한 단축 url 속성을 설정하고 이를 게시하는 옵션이 있습니다.

이 기능을 사용하려면 단축 통과를 허용하도록 Dispatcher 필터를 조정해야 합니다. 이는 작성자가 이러한 단축 페이지 항목을 설정해야 하는 비율로 Dispatcher 구성 파일을 조정하는 것과 같이 비합리적입니다.

이러한 이유로 Dispatcher 모듈에는 컨텐츠 트리에 단축으로 나열된 모든 항목을 자동으로 허용하는 기능이 있습니다.


## 작동 방식

### 단축 URL 작성

작성자가 AEM의 페이지를 방문하고 페이지 속성을 클릭한 다음 _가상 URL_ 섹션의 항목을 추가합니다. 변경 사항을 저장하고 페이지를 활성화하면 페이지에 단축이 할당됩니다.

작성자가 _가상 URL_ 항목을 추가할 때 _가상 URL 리디렉션_ 확인란을 선택할 수도 있습니다. 이렇게 하면 가상 URL이 302 리디렉션으로 동작합니다. 즉, 브라우저에서 `Location` 응답 헤더를 통해 새 URL로 이동하라는 메시지가 표시되고 브라우저가 새 URL에 새 요청을 합니다.

#### Touch UI:

![사이트 편집기 화면의 AEM 작성 UI에 대한 드롭다운 대화 상자 메뉴](assets/disp-vanity-url/aem-page-properties-drop-down.png "aem-page-properties-drop-down")

![aem 페이지 속성 대화 상자 페이지](assets/disp-vanity-url/aem-page-properties.png "aem-page-properties")

#### 클래식 컨텐츠 파인더:

![AEM siteadmin 클래식 ui 사이드 킥 페이지 속성](assets/disp-vanity-url/aem-page-properties-sidekick.png "aem-page-properties-sidekick")

![클래식 UI 페이지 속성 대화 상자](assets/disp-vanity-url/aem-page-properties-classic.png "aem-page-properties-classic")


>[!NOTE]
>
>이름 공간 문제가 발생하기 쉽다는 것을 이해합니다. 단축 항목은 모든 페이지에 적용되며 문제 해결을 계획해야 하는 짧은 작업 중 하나에 불과합니다. 그 중 일부는 나중에 설명해 드리겠습니다.


## 리소스 확인/매핑

내부 리디렉션을 위한 각 단축 항목은 sling 맵 항목입니다.

AEM 인스턴스 Felix 콘솔( `/system/console/jcrresolver` )을 방문하여 맵을 볼 수 있습니다.

다음은 단축 항목으로 만든 맵 항목의 스크린샷입니다.
![리소스 확인 규칙의 단축 항목의 콘솔 스크린샷](assets/disp-vanity-url/vanity-resource-resolver-entry.png "vanity-resource-resolver-entry")

위의 예에서 AEM 인스턴스에 `/aboutus` 방문을 요청하면 `/content/we-retail/us/en/about-us.html`(으)로 확인됩니다.

## Dispatcher 자동 허용 필터

보안 상태의 Dispatcher은 JCR 트리의 루트이므로 Dispatcher을 통해 `/` 경로의 요청을 필터링합니다.

게시자가 `/system`과(와) 같은 경로가 아닌 `/content` 및 기타 안전한 경로 등의 콘텐츠만 허용하고 있는지 확인하는 것이 중요합니다.

`/`의 기본 폴더에 있는 단축 URL은 다음과 같습니다. 보안 상태에서 게시자에게 도달할 수 있도록 하려면 어떻게 해야 합니까?

단순 Dispatcher에는 자동 필터 허용 메커니즘이 있으며 AEM 패키지를 설치한 다음 해당 패키지 페이지를 가리키도록 Dispatcher을 구성해야 합니다.

[https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html?package=/content/software-distribution/en/details.html/content/dam/aem/public/adobe/packages/granite/vanityurls-components](https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html?package=/content/software-distribution/en/details.html/content/dam/aem/public/adobe/packages/granite/vanityurls-components)

Dispatcher의 팜 파일에는 구성 섹션이 있습니다.

```
/vanity_urls { 
    /url    "/libs/granite/dispatcher/content/vanityUrls.html" 
    /file   "/tmp/vanity_urls" 
    /delay  300 
}
```

초 단위로 측정된 `/delay` 매개 변수는 고정된 간격으로 작동하지 않고 조건 기반 검사로 작동합니다. Dispatcher은 목록에 없는 URL에 대한 요청을 받으면 인식된 vanity URL 목록을 저장하는 `/file`의 수정 타임스탬프를 평가합니다. 현재 모멘트와 `/file`의 마지막 수정 날짜 사이의 시간 차이가 `/delay` 기간보다 작으면 `/file`이(가) 새로 고쳐지지 않습니다. `/file`을(를) 새로 고치는 작업은 다음 두 가지 조건에서 수행됩니다.

1. 캐시되지 않았거나 `/file`에 나열되지 않은 URL에 대해 들어오는 요청입니다.
1. `/file`을(를) 마지막으로 업데이트한 후 최소 `/delay`초가 경과되었습니다.

이 메커니즘은 Vanity URL 기능을 악용하여 요청으로 Dispatcher을 압도할 수 있는 서비스 거부(DoS) 공격으로부터 보호하도록 설계되었습니다.

간단히 말해, `/file`에 없는 URL에 대한 요청이 도착하고 `/file`의 마지막 수정 기간이 `/delay` 기간보다 오래 지난 경우에만 vanity URL이 포함된 `/file`이(가) 업데이트됩니다.

`/file`의 새로 고침을 명시적으로 트리거하기 위해 마지막 업데이트 이후 필요한 `/delay` 시간이 경과했는지 확인한 후 존재하지 않는 URL을 요청할 수 있습니다. 이러한 목적을 위한 URL의 예는 다음과 같습니다.

- `https://dispatcher-host-name.com/this-vanity-url-does-not-exist`
- `https://dispatcher-host-name.com/please-hand-me-that-planet-maestro`
- `https://dispatcher-host-name.com/random-vanity-url`

이 접근 방식은 마지막 수정 이후 지정된 `/delay` 간격이 경과하면 Dispatcher에서 `/file`을(를) 업데이트하도록 강제합니다.

이 예제에서는 `/tmp/vanity_urls`과(와) 같이 응답의 캐시를 `/file` 인수에 저장합니다

따라서 URI의 AEM 인스턴스를 방문하면 해당 인스턴스가 가져오는 내용이 표시됩니다.

/libs/granite/dispatcher/content/vanityUrls.html](assets/disp-vanity-url/vanity-url-component.png "vanity-url-component")에서 렌더링된 컨텐츠의 ![스크린샷

말 그대로 매우 간단한 목록입니다

## 단축 규칙으로 규칙 다시 작성

위에서 설명한 대로 AEM에 내장된 기본 메커니즘 대신 규칙 다시 작성을 사용하는 것이 언급되는 이유는 무엇입니까?

간단히 설명하면, 네임스페이스 문제, 성능 및 더 높은 수준의 로직을 효과적으로 처리할 수 있습니다.

이를 위해 Apache의 `mod_rewrite` 모듈을 사용하여 해당 콘텐츠 `/content/we-retail/us/en/about-us.html`에 대한 단축 항목 `/aboutus`의 예를 살펴보겠습니다.

```
RewriteRule ^/aboutus /content/we-retail/us/en/about-us.html [PT,L,NC]
```

이 규칙은 단축 `/aboutus`을(를) 찾고 PT 플래그(통과)를 사용하여 렌더러에서 전체 경로를 가져옵니다.

또한 다른 모든 규칙 L 플래그(마지막) 처리를 중지하므로 JCR 해결과 같은 거대한 규칙 목록을 탐색하지 않아도 됩니다.

요청을 프록시하지 않아도 되고 AEM 게시자가 이 두 가지 방법으로 응답할 때까지 기다리면 더 많은 성능이 발휘됩니다.

금상첨화는 NC 플래그(대/소문자 구분 안 함)입니다. 즉, 고객이 URI를 `/aboutus` 대신 `/AboutUs`(으)로 입력하면 여전히 작동합니다.

다시 작성 규칙을 만들어 수행하려면 Dispatcher에 구성 파일을 만들고(예: `/etc/httpd/conf.d/rewrites/examplevanity_rewrite.rules`) 이러한 단축 URL을 적용해야 하는 도메인을 처리하는 `.vhost` 파일에 포함합니다.

다음은 `/etc/httpd/conf.d/enabled_vhosts/we-retail.vhost` 내에 포함된 예제 코드 조각입니다.

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

## 사용 방법 및 위치

AEM을 사용하여 단축 항목을 제어하면 다음과 같은 이점이 있습니다

- 작성자는 즉석으로 만들 수 있습니다
- 콘텐츠와 함께 라이브하며 콘텐츠와 함께 패키지화할 수 있습니다

`mod_rewrite`을(를) 사용하여 단축 항목을 제어하면 다음과 같은 이점이 있습니다

- 신속한 컨텐츠 해결
- 최종 사용자 콘텐츠 요청의 가장자리에 더 가깝게
- 기타 조건에서 컨텐츠가 매핑되는 방식을 제어하는 더 많은 확장성 및 옵션
- 대/소문자를 구분하지 않을 수 있음

두 가지 방법을 모두 사용하되 다음 경우에 사용할 권고 사항 및 기준은 다음과 같습니다.

- 단축이 임시적이고 계획된 트래픽이 낮은 수준인 경우 AEM 기본 기능을 사용하십시오
- 단축이 자주 변경되지 않고 자주 사용하는 스테이플 끝점인 경우 `mod_rewrite` 규칙을 사용하십시오.
- 단축 네임스페이스(예: `/aboutus`)를 동일한 AEM 인스턴스의 많은 브랜드에 대해 다시 사용해야 하는 경우 규칙 다시 작성을 사용하십시오.

>[!NOTE]
>
>AEM 단축 기능을 사용하고 네임스페이스를 사용하지 않으려면 명명 규칙을 만들 수 있습니다. `/brand1/aboutus`, `brand2/aboutus`, `brand3/aboutus`과(와) 같이 중첩된 단축 URL을 사용합니다.

[다음 -> 공통 로깅](./common-logs.md)
