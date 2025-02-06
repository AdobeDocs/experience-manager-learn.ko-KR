---
title: URL 리디렉션
description: AEM에서 URL 리디렉션을 수행하는 다양한 옵션에 대해 알아봅니다.
version: 6.4, 6.5, Cloud Service
topic: Development, Administration
feature: Operations, Dispatcher
role: Developer, Architect
level: Intermediate
jira: KT-11466
last-substantial-update: 2024-10-22T00:00:00Z
index: y
doc-type: Article
exl-id: 8e64f251-e5fd-4add-880e-9d54f8e501a6
duration: 164
source-git-commit: 515c4020e1c358b5ee044a81affc8d7e1e4ff4eb
workflow-type: tm+mt
source-wordcount: '949'
ht-degree: 0%

---

# URL 리디렉션

URL 리디렉션은 웹 사이트 작업의 일부로 일반적인 측면입니다. 설계자와 관리자는 유연하고 빠른 리디렉션 배포 시간을 제공하는 URL 리디렉션을 관리하는 방법과 위치에 대한 최상의 솔루션을 찾아야 합니다.

[AEM(6.x)(예: AEM Classic](https://experienceleague.adobe.com/en/docs/experience-manager-learn/dispatcher-tutorial/chapter-2) 및 [AEM as a Cloud Service](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/overview/architecture) 인프라)를 잘 알고 있는지 확인하십시오. 주요 차이점은 다음과 같습니다.

1. AEM as a Cloud Service에는 [기본 제공 CDN](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn)이 있지만 고객은 AEM 관리 CDN 앞에 CDN(BYOCDN)을 제공할 수 있습니다.
1. AEM 6.x는 온프레미스 또는 Adobe Managed Services(AMS)에 AEM 관리 CDN이 포함되어 있지 않은지 여부이며 고객은 직접 방문해야 합니다.

다른 AEM 서비스(AEM Author/Publish 및 Dispatcher)는 AEM 6.x와 AEM as a Cloud Service 간에 개념적으로 유사합니다.

AEM의 URL 리디렉션 솔루션은 다음과 같습니다.

|                                                   | AEM 프로젝트 코드로 관리 및 배포됨 | 마케팅/콘텐츠 팀별 변경 기능 | Cloud Service 호환 AEM | 리디렉션 실행 발생 위치 |
|---------------------------------------------------|:-----------------------:|:---------------------:|:---------------------:| :---------------------:|
| AEM 관리 CDN을 통한 Edge에서 [1}](#at-edge-via-aem-managed-cdn) | ✔ | ✘ | ✔ | Edge/CDN(기본 제공) |
| [자체 CDN(BYOCDN) 가져오기를 통한 Edge에서](#at-edge-via-bring-your-own-cdn) | ✘ | ✘ | ✔ | Edge/CDN(BYOCDN) |
| [Dispatcher 구성으로서의 Apache `mod_rewrite` 규칙](#apache-mod_rewrite-module) | ✔ | ✘ | ✔ | Dispatcher |
| [ACS Commons - 맵 관리자 리디렉션](#redirect-map-manager) | ✘ | ✔ | ✔ | Dispatcher |
| [ACS Commons - 리디렉션 관리자](#redirect-manager) | ✘ | ✔ | ✔ | AEM / DISPATCHER |
| [`Redirect` 페이지 속성](#the-redirect-page-property) | ✘ | ✔ | ✔ | AEM |


## 솔루션 옵션

다음은 웹 사이트 방문자의 브라우저에 근접한 순서로 제공되는 솔루션 옵션입니다.

### AEM 관리 CDN을 통한 Edge에서 {#at-edge-via-aem-managed-cdn}

이 옵션은 AEM as a Cloud Service 고객만 사용할 수 있습니다.

[AEM 관리 CDN](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn)은(는) Edge 수준에서 리디렉션 솔루션을 제공하므로 원본으로의 왕복 시간이 줄어듭니다. [클라이언트측 리디렉션](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn-configuring-traffic#client-side-redirectors) 기능을 사용하면 AEM 프로젝트 코드에서 리디렉션 규칙을 구성하고 [구성 파이프라인](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/how-to-setup#deploy-rules-through-cloud-manager)을 사용하여 배포할 수 있습니다. CDN 구성 파일(`cdn.yaml`) 크기는 100KB를 초과할 수 없습니다.

Edge 또는 CDN 수준에서 리디렉션을 관리하면 성능이 향상됩니다.

### 자체 CDN을 통해 Edge에서

일부 CDN 서비스는 Edge 수준에서 리디렉션 솔루션을 제공하므로 오리진으로의 라운드 트립이 줄어듭니다. [Akamai Edge 리디렉터](https://techdocs.akamai.com/cloudlets/docs/what-edge-redirector), [AWS CloudFront 함수](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/cloudfront-functions.html)를 참조하십시오. Edge 수준 리디렉션 기능에 대해서는 CDN 서비스 공급자에게 문의하십시오.

Edge 또는 CDN 수준에서 리디렉션을 관리하면 성능적인 이점이 있지만 AEM의 일부로 관리되지 않고 개별 프로젝트로 관리됩니다. 문제를 방지하기 위해 리디렉션 규칙을 관리하고 배포하는 프로세스를 잘 정의하는 것이 중요합니다.


### Apache `mod_rewrite` 모듈

일반적인 솔루션은 [Apache 모듈 mod_rewrite](https://httpd.apache.org/docs/current/mod/mod_rewrite.html)을 사용합니다. [AEM Project Archetype](https://github.com/adobe/aem-project-archetype)은(는) [AEM 6.x](https://github.com/adobe/aem-project-archetype/tree/develop/src/main/archetype/dispatcher.ams#file-structure) 및 [AEM as a Cloud Service](https://github.com/adobe/aem-project-archetype/tree/develop/src/main/archetype/dispatcher.cloud#file-structure) 프로젝트 모두에 대한 Dispatcher 프로젝트 구조를 제공합니다. 기본(변경할 수 없음) 및 사용자 지정 다시 작성 규칙이 `conf.d/rewrites` 폴더에 정의되어 있으며 `conf.d/dispatcher_vhost.conf` 파일을 통해 `80` 포트에서 수신하는 `virtualhosts`에 대해 다시 작성 엔진이 켜져 있습니다. 예제 구현은 [AEM WKND 사이트 프로젝트](https://github.com/adobe/aem-guides-wknd/tree/main/dispatcher/src/conf.d/rewrites)에서 사용할 수 있습니다.

AEM as a Cloud Service에서 이러한 리디렉션 규칙은 AEM 코드의 일부로 관리되며 Cloud Manager [웹 계층 구성 파이프라인](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines) 또는 [전체 스택 파이프라인](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines)을 통해 배포됩니다. 따라서 AEM 프로젝트별 프로세스를 사용하여 리디렉션 규칙을 관리, 배포 및 추적할 수 있습니다.

대부분의 CDN 서비스는 `Cache-Control` 또는 `Expires` 헤더에 따라 HTTP 301 및 302 리디렉션을 캐시합니다. Apache/Dispatcher에서 시작된 초기 리디렉션 후 왕복 여행을 피하는 데 도움이 됩니다.


### ACS AEM Commons

[ACS AEM Commons](https://adobe-consulting-services.github.io/acs-aem-commons/) 내에서 URL 리디렉션을 관리하는 두 가지 기능을 사용할 수 있습니다. ACS AEM Commons는 커뮤니티에서 운영하는 오픈 소스 프로젝트이며 Adobe에서 지원하지 않습니다.

#### 맵 관리자 리디렉션

[리디렉션 맵 관리자](https://adobe-consulting-services.github.io/acs-aem-commons/features/redirect-map-manager/index.html)를 사용하면 AEM 관리자가 Apache 웹 서버에 직접 액세스하거나 Apache 웹 서버를 다시 시작하지 않아도 [Apache RewriteMap](https://httpd.apache.org/docs/2.4/rewrite/rewritemap.html) 파일을 쉽게 유지 관리하고 게시할 수 있습니다. 이 기능을 사용하면 권한 사용자가 개발 팀이나 AEM 배포의 도움 없이 AEM의 콘솔에서 리디렉션 규칙을 만들고, 업데이트하고, 삭제할 수 있습니다. 리디렉션 맵 관리자가 **AEM as a Cloud Service**([파이프라인 없는 URL 리디렉션](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/content-delivery/pipeline-free-url-redirects) 전략 참조)과 **AEM 6.x** 모두 호환됩니다.

#### 리디렉션 관리자

[리디렉션 관리자](https://adobe-consulting-services.github.io/acs-aem-commons/features/redirect-manager/index.html)를 사용하면 AEM의 사용자가 AEM에서 리디렉션을 쉽게 유지 관리하고 게시할 수 있습니다. 구현은 Java™ 서블릿 필터를 기반으로 하므로 일반적인 JVM 리소스 소비를 사용합니다. 또한 이 기능을 사용하면 AEM 개발 팀과 AEM 배포에 대한 종속성이 제거됩니다. 리디렉션 관리자가 **AEM as a Cloud Service**&#x200B;과(와) **AEM 6.x** 모두 호환됩니다. 초기 리디렉션 요청이 AEM Publish 서비스에 도달해야 기본적으로 301/302(대부분) CDN의 캐시 301/302를 생성하여 후속 요청이 edge/CDN에서 리디렉션될 수 있습니다.

[리디렉션 관리자](https://adobe-consulting-services.github.io/acs-aem-commons/features/redirect-manager/index.html)는 [Apache RewriteMap](https://httpd.apache.org/docs/2.4/rewrite/rewritemap.html)에 대해 [리디렉션을 텍스트 파일로 컴파일](https://adobe-consulting-services.github.io/acs-aem-commons/features/redirect-manager/subpages/rewritemap.html)하여 **AEM as a Cloud Service**&#x200B;에 대해 [파이프라인 없는 URL 리디렉션](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/content-delivery/pipeline-free-url-redirects) 전략을 지원하므로 직접 액세스하거나 다시 시작하지 않고도 Apache 웹 서버에서 사용되는 리디렉션을 업데이트할 수 있습니다. 이 시나리오에서는 초기 리디렉션 요청이 AEM Publish 서비스가 아닌 Apache 웹 서버에 도달합니다.

### `Redirect` 페이지 속성

[고급 탭](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/sites-console/page-properties.html)의 기본 제공(OOTB) `Redirect` 페이지 속성을 사용하면 콘텐츠 작성자가 현재 페이지의 리디렉션 위치를 정의할 수 있습니다. 이 솔루션은 페이지당 리디렉션 시나리오에 가장 적합하며 페이지 리디렉션을 보고 관리할 중앙 위치가 없습니다.

## 구현에 적합한 솔루션

다음은 적합한 솔루션을 결정하는 몇 가지 기준입니다. 또한 조직의 IT 및 마케팅 프로세스를 통해 적합한 솔루션을 선택할 수 있습니다.

1. 마케팅 팀 또는 수퍼 사용자가 AEM 개발 팀 및 AEM 배포 없이 리디렉션 규칙을 관리할 수 있도록 설정.
1. 변경 사항 또는 위험 완화를 관리, 확인, 추적 및 되돌리는 프로세스입니다.
1. CDN 서비스를 통해 Edge에서 **솔루션**&#x200B;에 대한 _주제 전문 지식_&#x200B;의 가용성.
