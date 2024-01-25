---
title: URL 리디렉션
description: AEM에서 URL 리디렉션을 수행하는 다양한 옵션에 대해 알아봅니다.
version: 6.4, 6.5, Cloud Service
topic: Development, Administration
feature: Operations, Dispatcher
role: Developer, Architect
level: Intermediate
jira: KT-11466
last-substantial-update: 2022-10-14T00:00:00Z
index: y
doc-type: Article
exl-id: 8e64f251-e5fd-4add-880e-9d54f8e501a6
duration: 214
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '781'
ht-degree: 0%

---

# URL 리디렉션

URL 리디렉션은 웹 사이트 작업의 일부로 일반적인 측면입니다. 설계자와 관리자는 유연하고 빠른 리디렉션 배포 시간을 제공하는 URL 리디렉션을 관리하는 방법과 위치에 대한 최상의 솔루션을 찾아야 합니다.

다음을 잘 알고 있는지 확인합니다. [AEM (6.x) (예: AEM Classic)](https://experienceleague.adobe.com/docs/experience-manager-learn/dispatcher-tutorial/chapter-2.html#the-%E2%80%9Clegacy%E2%80%9D-setup) 및 [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/overview/architecture.html#runtime-architecture) 인프라. 주요 차이점은 다음과 같습니다.

1. AEM as a Cloud Service has [기본 CDN](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn.html)그러나 고객은 AEM 관리 CDN 앞에 CDN(BYOCDN)을 제공할 수 있습니다.
1. AEM 6.x는 온프레미스 또는 Adobe Managed Services(AMS)에 AEM 관리 CDN이 포함되어 있지 않은지 여부이며 고객은 직접 방문해야 합니다.

다른 AEM 서비스(AEM 작성자/게시 및 Dispatcher)는 AEM 6.x와 AEM as a Cloud Service 간에 개념적으로 유사합니다.

AEM URL 리디렉션 솔루션은 다음과 같습니다.

|                                                   | AEM 프로젝트 코드로 관리 및 배포됨 | 마케팅/콘텐츠 팀별 변경 기능 | Cloud Service 호환 AEM | 리디렉션 실행 발생 위치 |
|---------------------------------------------------|:-----------------------:|:---------------------:|:---------------------:| :---------------------:|
| [자체 CDN을 통해 Edge에서](#at-edge-via-bring-your-own-cdn) | ✘ | ✘ | ✔ | Edge/CDN |
| [Apache `mod_rewrite` 규칙을 Dispatcher 구성으로 사용](#apache-mod_rewrite-module) | ✔ | ✘ | ✔ | Dispatcher |
| [ACS Commons - 맵 관리자 리디렉션](#redirect-map-manager) | ✘ | ✔ | ✘ | Dispatcher |
| [ACS Commons - 리디렉션 관리자](#redirect-manager) | ✘ | ✔ | ✔ | AEM |
| [다음 `Redirect` 페이지 속성](#the-redirect-page-property) | ✘ | ✔ | ✔ | AEM |


## 솔루션 옵션

다음은 웹 사이트 방문자의 브라우저에 근접한 순서로 제공되는 솔루션 옵션입니다.

### 자체 CDN을 통해 Edge에서

일부 CDN 서비스는 에지 수준에서 리디렉션 솔루션을 제공하므로 오리진으로의 라운드 트립이 줄어듭니다. 다음을 참조하십시오 [Akamai Edge 리디렉터](https://techdocs.akamai.com/cloudlets/docs/what-edge-redirector), [AWS CloudFront 함수](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/cloudfront-functions.html). 에지 수준 리디렉션 기능에 대해서는 CDN 서비스 공급자에게 문의하십시오.

Edge 또는 CDN 수준에서 리디렉션을 관리하면 성능이 향상되지만 AEM의 일부로 관리되지 않고 개별 프로젝트로 관리됩니다. 문제를 방지하려면 리디렉션 규칙을 관리하고 배포하는 잘 알고 있는 프로세스가 중요합니다.


### Apache `mod_rewrite` 모듈

일반적인 솔루션은 [Apache 모듈 mod_rewrite](https://httpd.apache.org/docs/current/mod/mod_rewrite.html). 다음 [AEM Project Archetype](https://github.com/adobe/aem-project-archetype) 는 두 모두에 대한 Dispatcher 프로젝트 구조를 제공합니다. [AEM 6.x](https://github.com/adobe/aem-project-archetype/tree/develop/src/main/archetype/dispatcher.ams#file-structure) 및 [AEM as a Cloud Service](https://github.com/adobe/aem-project-archetype/tree/develop/src/main/archetype/dispatcher.cloud#file-structure) 프로젝트. 기본(변경할 수 없음) 및 사용자 지정 재작성 규칙은 `conf.d/rewrites` 폴더 및 재작성 엔진이 다음에 대해 켜짐 `virtualhosts` 포트에서 수신 대기 `80` 경유 `conf.d/dispatcher_vhost.conf` 파일. 예제 구현은에서 사용할 수 있습니다 [AEM WKND Sites 프로젝트](https://github.com/adobe/aem-guides-wknd/tree/main/dispatcher/src/conf.d/rewrites).

AEM as a Cloud Service에서 이러한 리디렉션 규칙은 AEM 코드의 일부로 관리되고 Cloud Manager를 통해 배포됩니다 [웹 계층 구성 파이프라인](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html#web-tier-config-pipelines) 또는 [전체 스택 파이프라인](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html#full-stack-pipeline). 따라서 AEM 프로젝트별 프로세스를 사용하여 리디렉션 규칙을 관리, 배포 및 추적할 수 있습니다.

대부분의 CDN 서비스는 상황에 따라 HTTP 301 및 302 리디렉션을 캐시합니다 `Cache-Control` 또는 `Expires` 머리글입니다. 이렇게 하면 Apache/Dispatcher에서 시작된 초기 리디렉션 후 왕복이 발생하지 않도록 하는 데 도움이 됩니다.


### ACS AEM Commons

에는 두 가지 기능이 있습니다 [ACS AEM Commons](https://adobe-consulting-services.github.io/acs-aem-commons/) URL 리디렉션을 관리합니다. ACS AEM Commons는 커뮤니티에서 운영하는 오픈 소스 프로젝트이며 Adobe에서 지원하지 않습니다.

#### 맵 관리자 리디렉션

[맵 관리자 리디렉션](https://adobe-consulting-services.github.io/acs-aem-commons/features/redirect-map-manager/index.html) AEM 6.x 관리자가 쉽게 유지 관리하고 게시할 수 있도록 허용 [Apache RewriteMap](https://httpd.apache.org/docs/2.4/rewrite/rewritemap.html) apache 웹 서버에 직접 액세스하지 않거나 Apache 웹 서버를 다시 시작하지 않아도 되는 파일입니다. 이 기능을 사용하면 권한 사용자가 개발 팀이나 AEM 배포의 도움 없이 AEM의 콘솔에서 리디렉션 규칙을 만들고, 업데이트하고, 삭제할 수 있습니다. 리디렉션 맵 관리자: **AEM과 as a Cloud Service으로 호환되지 않음**.

#### 리디렉션 관리자

[리디렉션 관리자](https://adobe-consulting-services.github.io/acs-aem-commons/features/redirect-manager/index.html) AEM의 사용자는 AEM에서 리디렉션을 쉽게 유지 관리하고 게시할 수 있습니다. 구현은 Java™ 서블릿 필터를 기반으로 하므로 일반적인 JVM 리소스 소비를 사용합니다. 또한 이 기능을 사용하면 AEM 개발 팀과 AEM 배포에 대한 종속성이 제거됩니다. 리디렉션 관리자는 둘 다 **AEM as a Cloud Service** 및 **AEM 6.x** 호환 가능. 초기 리디렉션 요청이 AEM Publish 서비스에 도달해야 기본적으로 301/302(대부분) CDN의 캐시 301/302를 생성하여 후속 요청이 Edge/CDN에서 리디렉션될 수 있습니다.

### 다음 `Redirect` 페이지 속성

기본 제공(OOTB) `Redirect` 의 페이지 속성 [고급 탭](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/fundamentals/page-properties.html#advanced) 콘텐츠 작성자는 현재 페이지의 리디렉션 위치를 정의할 수 있습니다. 이 솔루션은 페이지당 리디렉션 시나리오에 가장 적합하며 페이지 리디렉션을 보고 관리할 중앙 위치가 없습니다.

## 구현에 적합한 솔루션

다음은 적합한 솔루션을 결정하는 몇 가지 기준입니다. 또한 조직의 IT 및 마케팅 프로세스를 통해 적합한 솔루션을 선택할 수 있습니다.

1. 마케팅 팀 또는 수퍼 사용자가 AEM 개발 팀 및 AEM 배포 없이 리디렉션 규칙을 관리할 수 있도록 설정.
1. 변경 사항 또는 위험 완화를 관리, 확인, 추적 및 되돌리는 프로세스입니다.
1. 가용성 _주제별 전문 지식_ 대상 **CDN 서비스를 통한 에지** 해결책.
