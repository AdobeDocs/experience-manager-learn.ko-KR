---
source-git-commit: 10032375a23ba99f674fb59a6f6e48bf93908421
workflow-type: tm+mt
source-wordcount: '174'
ht-degree: 0%

---



# AEM Headless 배포

AEM Headless 클라이언트 배포는 많은 양식을 사용합니다. AEM에서 호스팅하는 SPA, 외부 SPA 또는 웹 사이트, 모바일 앱 또는 심지어 서버 간 프로세스까지

AEM Headless 배포는 클라이언트와 배포 방법에 따라 다른 고려 사항이 있습니다.

## AEM 서비스 아키텍처

배포 고려 사항을 살펴보기 전에 AEM 논리 아키텍처와 AEM as a Cloud Service 서비스 계층의 분리 및 역할을 이해하는 것이 중요합니다. AEM as a Cloud Service 은 두 개의 논리 서비스로 구성됩니다.

+ __AEM 작성자__ 는 팀이 컨텐츠 조각(및 기타 자산)을 작성, 공동 작업 및 게시하는 서비스입니다
+ __AEM 게시__ 게시된 컨텐츠 조각(및 기타 자산)이 일반 소비에 대해 복제되는 서비스입니다

프로덕션 용량으로 작동하는 AEM Headless 클라이언트는 일반적으로 승인된 게시된 컨텐츠를 포함하는 AEM Publish와 상호 작용합니다. AEM 작성자가 기본적으로 안전하므로 AEM 작성자와 상호 작용하는 클라이언트는 특별히 고려해야 합니다. 즉, 모든 요청에 대해 권한을 부여해야 하며, 불완전하거나 승인되지 않은 컨텐츠가 포함되어 있을 수 있습니다.

## 헤드리스 클라이언트 배포

+ [단일 페이지 앱](./spa.md)
+ [웹 구성 요소/JS](./web-component.md)
+ [모바일 앱](./mobile.md)
+ [서버 간](./server-to-server.md)

