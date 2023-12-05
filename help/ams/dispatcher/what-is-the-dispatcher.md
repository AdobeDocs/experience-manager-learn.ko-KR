---
title: '"Dispatcher"란?'
description: Dispatcher가 실제로 무엇인지 이해합니다.
version: 6.5
topic: Administration
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
doc-type: Article
exl-id: 96c8dd09-e0a7-4abc-b04b-a805aaa67502
duration: 109
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '256'
ht-degree: 2%

---

# &quot;Dispatcher&quot;란?

[목차](./overview.md)

AEM Dispatcher를 포함하는 항목에 대한 기본 설명부터 시작합니다.

## Apache 웹 서버

Linux 서버에서 기본 Apache 웹 서버 설치로 시작합니다.

Apache 서버의 기능에 대한 기본 설명:

- 간단한 규칙을 따라 정적 문서 디렉토리에서 HTTP(s) 프로토콜을 통해 파일을 제공합니다(`DocumentRoot`)
- 기본 위치에 저장된 파일(`/var/www/html`)은 요청에 일치하며 요청하는 클라이언트의 브라우저에 렌더링됩니다




## AEM 특정 모듈 파일(`mod_dispatcher.so`)

그런 다음 Dispatcher 모듈이라는 Apache 웹 서버에 플러그인을 추가합니다

Adobe AEM Dispatcher 모듈의 기능에 대한 기본 설명:

- 기본 파일 처리기를 늘립니다.
- 잘못된 요청을 필터링하고 AEM 소프트 밸리/엔드포인트를 보호합니다.
- 두 개 이상의 렌더러가 있는 경우 부하 분산
- 라이브 캐시 디렉터리 허용 / 정체된 파일 플러시 지원
- 모든 AMS 설치의 정문이며 웹 사이트와 에셋을 클라이언트의 브라우저에 전달합니다
- AEM 서버가 자체적으로 수행할 수 있는 속도보다 훨씬 빠른 속도로 재서비스 요청을 캐시합니다
- 훨씬 더...

## 웹 트래픽 워크플로

기본 Dispatcher 서버를 구축하기 위해 함께 설치되는 부분을 이해하면 Adobe 관리자 서비스 구성을 위한 기본 웹 트래픽 워크플로를 이해할 수 있습니다.
따라서 AEM 콘텐츠 방문자에게 콘텐츠를 제공하는 시스템 체인에서 어떤 역할을 하는지 이해하는 데 도움이 됩니다.

<b>이미 캐시된 콘텐츠 제공</b>

```
End User's Browser request 
    → Cloud Provider Load Balancer 
        → "The Dispatcher" 
            → Checks for cached request locally if found 
                → return request 
                    → End User
```

<b>AEM에서 새로운 콘텐츠 제공</b>

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
