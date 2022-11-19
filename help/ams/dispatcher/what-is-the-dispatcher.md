---
title: '"디스패처"란?'
description: Dispatcher가 실제로 무엇인지 파악합니다.
version: 6.5
topic: Administration
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
source-git-commit: 829ad9733b4326c79b9b574b13b1d4c691abf877
workflow-type: tm+mt
source-wordcount: '256'
ht-degree: 2%

---


# &quot;디스패처&quot;란?

[목차](./overview.md)

AEM Dispatcher가 필요한 사항에 대한 기본 설명부터 시작하십시오.

## Apache 웹 서버

Linux 서버에 기본 Apache 웹 서버 설치로 시작하십시오.

Apache 서버가 수행하는 작업에 대한 기본 설명:

- 간단한 규칙에 따라 정적 문서 디렉토리(`DocumentRoot`)
- 기본 위치에 저장된 파일(`/var/www/html`)가 요청 시 일치하고 요청 클라이언트의 브라우저에서 렌더링됩니다.




## AEM 특정 모듈 파일(`mod_dispatcher.so`)

그런 다음 Dispatcher 모듈이라는 Apache 웹 서버에 플러그인을 추가합니다

Adobe AEM Dispatcher 모듈에서 수행하는 작업에 대한 기본 설명:

- 기본 파일 처리기를 보강합니다.
- 잘못된 요청을 필터링하고/AEM 소프트 벨리/끝점을 보호합니다.
- 두 개 이상의 렌더러가 있는 경우 로드 밸런싱
- Live Cache 디렉터리 허용/Relied 파일 플러시 지원
- 모든 AMS 설치를 위한 프론트도어이며, 웹 사이트 및 자산을 클라이언트의 브라우저에 전달합니다
- AEM 서버가 단독으로 수행할 수 있는 것보다 훨씬 빠른 속도로 요청을 캐시합니다
- 훨씬 더..

## 웹 트래픽 워크플로우

기본 Dispatcher 서버를 구축하기 위해 함께 설치되는 조각을 이해하면 Adobe Manager Services 구성에 대한 기본 웹 트래픽 워크플로우를 이해할 수 있습니다.
이렇게 하면 AEM 컨텐츠 방문자에게 컨텐츠를 제공하는 시스템 체인에서 이 역할이 수행하는 역할을 이해하는 데 도움이 됩니다.

<b>이미 캐시된 컨텐츠 제공</b>

```
End User's Browser request 
    → Cloud Provider Load Balancer 
        → "The Dispatcher" 
            → Checks for cached request locally if found 
                → return request 
                    → End User
```

<b>AEM의 새로운 컨텐츠 제공</b>

```
End User's Browser request 
    → Cloud Provider Load Balancer 
        → "The Dispatcher" 
            → Checks for cached request locally if NOT found 
                → requests content from publisher 
                    → publisher sends content 
                        → Dispatcher adds content to cache and replies 
                            → End User
```

<b>컨텐츠 게시/변경 사항</b>

```
AEM Author User activates content 
    → Triggers content to be replicated to Publisher 
        → Publisher gets content and triggers the flush request to Dispatcher 
            → Dispatcher invalidates changed content 
            * Next request for that content will request fresh copy from publisher *
```

[다음 -> 기본 파일 레이아웃](./basic-file-layout.md)