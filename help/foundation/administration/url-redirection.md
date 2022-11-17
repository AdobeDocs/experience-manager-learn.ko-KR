---
title: URL 리디렉션
description: AEM에서 URL 리디렉션을 수행하는 다양한 옵션에 대해 알아봅니다.
version: 6.4, 6.5, Cloud Service
topic: Development, Administration
feature: Operations, Dispatcher
role: Developer, Architect
level: Intermediate
kt: 11466
last-substantial-update: 2022-10-14T00:00:00Z
index: y
source-git-commit: 00ea3a8e6b69cd99cf293093d38b59df51f6a26d
workflow-type: tm+mt
source-wordcount: '876'
ht-degree: 1%

---


# URL 리디렉션

URL 리디렉션은 웹 사이트 작업의 일부로서 일반적인 측면입니다. 설계자와 관리자는 유연하고 빠른 리디렉션 배포 시간을 제공하는 URL 리디렉션을 관리하는 방법 및 위치에 대한 최상의 솔루션을 찾아야 한다는 과제를 안고 있습니다.

에 대해 잘 알고 있는지 확인합니다. [AEM(6.x) aka AEM Classic](https://experienceleague.adobe.com/docs/experience-manager-learn/dispatcher-tutorial/chapter-2.html#the-%E2%80%9Clegacy%E2%80%9D-setup) 및 [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/overview/architecture.html#runtime-architecture) 인프라 주요 차이점은 다음과 같습니다.

1. AEM as a Cloud Service [기본 제공 CDN](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn.html)그러나 고객은 AEM 관리 CDN 앞에 BYOCDN(CDN)을 제공할 수 있습니다.
1. 온-프레미스 또는 Adobe Managed Services(AMS)가 AEM 관리 CDN을 포함하지 않는지 그리고 고객은 고유한 CDN을 가져와야 합니다.

다른 AEM 서비스(AEM 작성자/게시 및 Dispatcher)는 다르게 AEM 6.x와 AEM as a Cloud Service 간에 개념적으로 유사합니다.

AEM URL 리디렉션 솔루션은 다음과 같습니다.

|  | AEM 프로젝트 코드로 관리 및 배포 | 마케팅/콘텐츠 팀별 변경 기능 | AEM as a Cloud Service 호환 | 리디렉션 실행이 발생하는 위치 |
|---------------------------------------------------|:-----------------------:|:---------------------:|:---------------------:| :---------------------:|
| [고유한 CDN을 가져올 수 있는 Edge](#at-edge-via-bring-your-own-cdn) | ✘ | ✘ | ✔ | Edge/CDN |
| [Apache `mod_rewrite` 규칙을 Dispatcher 구성으로서 ](#apache-mod_rewrite-module) | ✔ | ✘ | ✔ | Dispatcher |
| [ACS Commons - 리디렉션 맵 관리자](#redirect-map-manager) | ✘ | ✔ | ✘ | Dispatcher |
| [ACS Commons - 리디렉션 관리자](#redirect-manager) | ✘ | ✔ | ✔ | AEM |
| [다음 `Redirect` 페이지 속성](#the-redirect-page-property) | ✘ | ✔ | ✔ | AEM |


## 솔루션 옵션

다음은 웹 사이트 방문자의 브라우저에 가까운 순서로 솔루션 옵션입니다.

### 고유한 CDN을 가져올 수 있는 Edge

일부 CDN 서비스는 에지 수준에서 리디렉션 솔루션을 제공하므로 원본에 대한 라운드 트립이 줄어듭니다. 자세한 내용은 [Akamai Edge Redirector](https://techdocs.akamai.com/cloudlets/docs/what-edge-redirector), [AWS CloudFront 기능](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/cloudfront-functions.html). 에지 수준 리디렉션 기능에 대해서는 CDN 서비스 공급자에게 문의하십시오.

Edge 또는 CDN 수준에서 리디렉션을 관리하는 것은 성능 이점이 있지만 AEM의 일부로 관리되지는 않고 개별 프로젝트의 일부로 관리됩니다. 문제를 방지하기 위해 리디렉션 규칙을 관리하고 배포하는 잘 생각해 있는 프로세스가 중요합니다.


### Apache `mod_rewrite` 모듈

일반적인 솔루션 사용 [Apache 모듈 mod_rewrite](https://httpd.apache.org/docs/current/mod/mod_rewrite.html). 다음 [AEM 프로젝트 원형](https://github.com/adobe/aem-project-archetype) 는 두 작업 모두에 대한 Dispatcher 프로젝트 구조를 제공합니다 [AEM 6.x](https://github.com/adobe/aem-project-archetype/tree/develop/src/main/archetype/dispatcher.ams#file-structure) 및 [AEM as a Cloud Service](https://github.com/adobe/aem-project-archetype/tree/develop/src/main/archetype/dispatcher.cloud#file-structure) 프로젝트. 기본(변경할 수 없음) 및 사용자 지정 재작성 규칙은 `conf.d/rewrites` 폴더 및 rewrite 엔진이 ON으로 설정되어 있습니다. `virtualhosts` 좌현 `80` via `conf.d/dispatcher_vhost.conf` 파일. 예제 구현은 [AEM WKND Sites 프로젝트](https://github.com/adobe/aem-guides-wknd/tree/main/dispatcher/src/conf.d/rewrites).

AEM as a Cloud Service에서 이러한 리디렉션 규칙은 AEM 코드의 일부로 관리되고 Cloud Manager를 통해 배포됩니다 [웹 계층 구성 파이프라인](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html#web-tier-config-pipelines) 또는 [전체 스택 파이프라인](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html#full-stack-pipeline). 따라서 AEM 프로젝트별 프로세스는 리디렉션 규칙을 관리, 배포 및 추적하는 데 사용됩니다.

대부분의 CDN 서비스는 해당 서비스에 따라 HTTP 301 및 302 리디렉션을 캐시합니다 `Cache-Control` 또는 `Expires` 헤더. 이렇게 하면 Apache/Dispatcher에서 처음 리디렉션된 후에 라운드 트립을 방지할 수 있습니다.


### ACS AEM Commons

에는 두 가지 기능이 있습니다 [ACS AEM Commons](https://adobe-consulting-services.github.io/acs-aem-commons/) url 리디렉션을 관리하려면 다음을 수행하십시오. ACS AEM Commons는 커뮤니티 운영 오픈 소스 프로젝트이며 Adobe에서 지원하지 않습니다.

#### 리디렉션 맵 관리자

[리디렉션 맵 관리자](https://adobe-consulting-services.github.io/acs-aem-commons/features/redirect-map-manager/index.html) AEM 6.x 관리자가 쉽게 유지 및 게시할 수 있도록 합니다. [Apache RewriteMap](https://httpd.apache.org/docs/2.4/rewrite/rewritemap.html) Apache 웹 서버에 직접 액세스하거나 Apache 웹 서버를 다시 시작할 필요가 없는 파일. 이 기능을 사용하면 개발 팀이나 AEM 배포 없이 권한 사용자가 AEM의 콘솔에서 리디렉션 규칙을 만들고, 업데이트하고 삭제할 수 있습니다. 리디렉션 맵 관리자: **AEM as a Cloud Service 호환 불가**.

#### 리디렉션 관리자

[리디렉션 관리자](https://adobe-consulting-services.github.io/acs-aem-commons/features/redirect-manager/index.html) AEM의 사용자는 AEM에서 리디렉션을 쉽게 유지 및 게시할 수 있습니다. 이 구현은 Java™ 서블릿 필터를 기반으로 하므로 일반적인 JVM 리소스 소비가 가능합니다. 또한 이 기능을 사용하면 AEM 개발 팀 및 AEM 배포에 대한 종속성도 제거됩니다. 리디렉션 관리자가 둘 다 **AEM as a Cloud Service** 및 **AEM 6.x** 호환 가능합니다. 초기 리디렉션된 요청이 AEM Publish 서비스301/302 히트해야 기본 CDN 캐시 301/302을 생성할 수 있지만 후속 요청을 Edge/CDN에서 리디렉션할 수 있습니다.

### 다음 `Redirect` 페이지 속성

즉시 사용 가능한(OOTB) `Redirect` 페이지의 페이지 속성 [고급 탭](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/fundamentals/page-properties.html#advanced) 컨텐츠 작성자가 현재 페이지의 리디렉션 위치를 정의할 수 있습니다. 이 솔루션은 페이지당 리디렉션 시나리오에 가장 적합하며 페이지 리디렉션을 보고 관리할 중앙 위치가 없습니다.

## 구현에 적합한 솔루션

다음은 올바른 솔루션을 결정하는 몇 가지 기준입니다. 또한 조직의 IT 및 마케팅 프로세스가 적합한 솔루션을 선택하는 데 도움이 되어야 합니다.

1. AEM 개발 팀 및 AEM 배포 없이 마케팅 팀 또는 슈퍼 사용자가 리디렉션 규칙을 관리할 수 있도록 설정.
1. 변경 또는 위험 완화를 관리, 확인, 추적 및 되돌리는 프로세스입니다.
1. 사용 가능 _분야별 전문 지식_ 대상 **CDN 서비스를 통해 Edge에서** 솔루션.

