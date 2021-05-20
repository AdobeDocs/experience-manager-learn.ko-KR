---
title: 업그레이드해야 하는 이유 이해하기
description: 최신 버전의 Adobe Experience Manager으로 업그레이드하려는 고객을 위한 주요 기능에 대한 높은 수준의 분석입니다.
version: 6.5
sub-product: assets, cloud manager, commerce, content-services, dynamic-media, forms, foundation, screens, sites
topics: best-practices, upgrade
audience: all
activity: understand
doc-type: article
topic: 업그레이드
role: Leader, Architect, Developer, Administrator, Business Practitioner
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '3541'
ht-degree: 3%

---


# 업그레이드해야 하는 이유 이해하기

최신 버전의 Adobe Experience Manager으로 업그레이드하려는 고객을 위한 주요 기능에 대한 높은 수준의 분석입니다.

## AEM 6.5로 업그레이드하기 위한 주요 기능

+ [Adobe Experience Manager 6.5 릴리스 노트](https://helpx.adobe.com/kr/experience-manager/6-5/release-notes.html)

### 기초 개선 사항

Adobe Experience Manager 6.5는 다음을 통해 시스템의 안정성, 성능 및 지원 기능을 계속 향상시킵니다.

+ **Java 11** 지원(Java 8를 지원하는 동안).

### 웹 사이트 제작 및 관리

AEM Sites에서는 웹 사이트 제작 및 빌드를 가속화하기 위해 고안된 다양한 기능을 도입합니다.

+ **SPA** EditorSupport를 사용하면 AEM에서 SPA(단일 페이지 애플리케이션)이 완전히 작성되므로 마케터에게 친숙한 다양한 작성 환경을 지원할 수 있습니다.
+_ **JavaScript SDK의**, SPA 프로젝트 시작 키트 및 지원 빌드 도구를 사용하여 프런트 엔드 개발자는 AEM과 독립적으로 SPA Editor 호환 가능한 단일 페이지 응용 프로그램을 개발할 수 있습니다.
+ **핵심** 구성 요소는  **구성 요소 라이브러리와** 기존 핵심 구성요소에 대한 다양한 개선 사항을 제공합니다.
+ 또한 **번역** 개선 사항을 통해 AEM Sites의 번역이 간소화됩니다.

### 유연한 환경

AEM은 AEM 외부의 컨텐츠를 쉽게 사용할 수 있도록 해주는 새롭고 향상된 도구를 사용하여 Fluid Experiences를 계속 수용합니다.

+ **컨텐츠** 조각은 버전 비교/비교 및 주석을 지원합니다.
+ **AEM Assets HTTP** API는  **DAM에서 직접 컨텐츠** 조각을 JSON으로 노출할  **수 있도록 지원합니다**.
   **경험** 조각은 페이지 **를 참조하기** 위한 전체 텍스트 검색 및  **AEM Dispatcher 캐시** 무효화 **를 지원합니다**.

### 자산 관리

AEM Assets은 DAM의 사용, 관리 및 이해를 돕기 위해 다양한 자산 관리 기능을 기반으로 계속 빌드하고 있습니다. AEM 6.5는 Adobe Creative Cloud과 크리에이티브 워크플로우 간의 통합을 계속 개선합니다.

+ **Adobe 자산** 링크는 크리에이티브를 Adobe Creative Cloud 도구에서 AEM Assets에 직접 연결합니다.
+ **Adobe** Stock 통합을 통해 AEM Assets 경험에서 직접 Adobe Stock 이미지에 직접 액세스할 수 있으므로 완벽한 컨텐츠 검색 경험을 만들 수 있습니다.
+ **AEM Desktop** Applems 버전 2.0 및 재설계(성능 및 안정성 향상)
+ **연결된 자산** 은 다른 AEM Assets 인스턴스의 자산에 원활하게 액세스하고 사용할 수 있도록 개별 AEM Sites 인스턴스를 지원합니다.
+ **Dynamic Media**&#x200B;에서 **360 비디오** 및 **사용자 지정 비디오 축소판**&#x200B;을 포함하는 비디오 지원이 업데이트되었습니다.

### 콘텐츠 인텔리전스

AEM은 모든 경험을 향상시키기 위해 머신 러닝 및 인공 지능을 활용하여 스마트 기술과의 통합을 계속 구축합니다.

+ **Adobe 자산** 링크 **는 시각적 유사성 검색**&#x200B;을 추가하여  **Adobe Creative Cloud 도구**&#x200B;에서 유사한 이미지를 쉽게 검색하고 사용할 수 있도록 합니다.

### 통합

AEM은 다른 Adobe 서비스와 통합할 수 있습니다.

+ **경험** 조각은 Adobe Target에 JSON으로 내보내기  **및** Adobe Target **에서 경험 조각 기반 오퍼를** 삭제하 **는 기능을 지원함으로써 Adobe Target과의 통합**   ****&#x200B;을 강화합니다.

### AMS Cloud Manager

[Adobe AMS(Managed Services) 고객을 제외하는 Cloud Manager](https://adobe.ly/2HODmsv)에서는 다음 기능을 제공합니다.

+ Cloud Manager는 AEM Sites에서 **AEM Assets**&#x200B;로 AEM 배포 지원을 확장하여 자산 처리&#x200B;**의 자동화된 성능 테스트를 지원합니다.**
+ **사전** 정의된 임계값에서 AEM 게시 계층의 자동 스케일링을 통해 최적의 최종 사용자 경험을 제공할 수 있습니다.
+ **비프로덕션 파이프라인** 을 사용하면 개발 팀이 Cloud Manager를 활용하여 코드 품질을 지속적으로 확인하고 낮은 환경(개발 및 QA)에 배포할 수 있습니다.
+ **CI/CD Pipeline** APIsallow 고객은 Cloud Manager를 프로그래밍 방식으로 사용하여 온-프레미스 개발 인프라와의 통합을 더욱 깊이 있게 수행할 수 있습니다.

## 기초 기능

다음은 AEM에서 제공하는 주요 기초 기능의 표입니다. 이러한 기능 중 일부는 각 릴리스에 추가된 이전 버전의 증분 개선 사항에서 도입되었습니다.

+ [AEM Foundation 릴리스 노트](https://helpx.adobe.com/experience-manager/6-5/release-notes/wcm-platform.html)

***✔<sup>+</sup> 이 버전의 기능에 대한 중요한 개선 사항입니다.***

***✔<sup></sup> SP는 서비스 팩 또는 기능 팩을 통해 사용할 수 있음을 나타냅니다.***

<table>
    <thead>
        <tr>
            <td>기초 기능</td>
            <td>5.6.x</td>
            <td>6.0</td>
            <td>6.1</td>
            <td>6.2</td>
            <td>6.3</td>
            <td>6.4</td>
            <td>6.5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>
                <strong>Java 11 지원: </strong> AEM은 Java 11(Java 8 및 Java 8)을 지원합니다.
            </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
        </tr>
        <tr>
            <td>
                <strong><a href="https://jackrabbit.apache.org/oak/docs/index.html" target="_blank">Oak 컨텐츠 저장소</a>:</strong> 이전의 Jackrabbit 2보다 훨씬 뛰어난 성능과 확장성을 제공합니다.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/indexing-via-the-oak-run-jar.html">oak-run.jar 색인 지원</a>:</strong> Oak 인덱스에 대한 재인덱싱, 통계 수집 및 일관성 검사가 개선되었습니다.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/queries-and-indexing.html" target="_blank">사용자 지정 검색 색인</a>: </strong>
                사용자 지정 색인 정의를 추가하여 쿼리 성능을 최적화하고 검색 관련성을 최적화할 수 있습니다.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/kr/experience-manager/6-5/sites/deploying/using/revision-cleanup.html" target="_blank">온라인 개정 정리</a>:</strong>
                서버 다운타임 없이 저장소 유지 관리를 수행합니다.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/storage-elements-in-aem-6.html" target="_blank">TarMK 또는 MongoMK 저장소</a>:</strong>
                <br>  TarMK(차세대 TarPM 버전)의 간단한 성능 파일 기반 저장소를 사용하거나 MongoMK가 포함된 MongoDB 백업 저장소
                <br> 를 사용하여 가로로 확장할 수 있는 옵션입니다.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/aem-with-mongodb.html" target="_blank">MongoMK 성능 및 안정성</a>:</strong>
             AEM 6.0을 처음 사용하기 때문에 MongoMK가 계속 향상되었습니다.</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/data-store-config.html#AmazonS3DataStore">Amazon S3 DataStore</a>:</strong>
            확장 가능한 클라우드 스토리지 솔루션을 활용하여 바이너리 자산을 저장합니다.</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Touch UI 기능 패리티: </strong>
                생산성 및 클래식 UI의 기능 패리티를 높여 빠르게 UI 작성을 위한 지속적인 개선 사항.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Omnisearch:</strong>
                AEM을 빠르게 검색하고 탐색합니다.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/operations-dashboard.html" target="_blank">작업 대시보드</a>:</strong>
 AEM 내에서 유지 관리를 수행하고 서버 상태를 모니터링하고 성능을 분석합니다.</td>
            <td></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/upgrade.html" target="_blank">업그레이드 개선 사항</a>:</strong>
             업그레이드 개선 사항을 통해 AEM을 보다 쉽고 빠르게 업그레이드할 수 있습니다.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/kr/experience-manager/htl/using/overview.html" target="_blank">HTL 템플릿 언어</a>:</strong>
             프레젠테이션과 논리를 구분하는 최신 템플릿 엔진입니다. 구성 요소 개발 시간을 크게 줄입니다. 각 릴리스에 추가된 증분 기능입니다.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://sling.apache.org/documentation/bundles/models.html" target="_blank">Sling 모델</a>:</strong>
             JCR 리소스를 비즈니스 개체 및 로직으로 모델링하는 유연한 프레임워크입니다. 각 릴리스에 추가된 증분 기능입니다.
            </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://adobe.ly/2HODmsv" target="_blank">Cloud Manager</a>: </strong>
                Adobe AMS(Managed Services) 고객을 전용으로 하는 Cloud Manager는 최신 CI/CD 파이프라인의 상태를 통해 개발 및 배포를 가속화합니다.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
    </tbody>
</table>

## 보안 기능

다음은 AEM에서 제공하는 주요 보안 기능 매트릭스입니다. 이러한 기능 중 일부는 각 릴리스에 추가된 이전 버전의 증분 개선 사항에서 도입되었습니다.

+ [보안 릴리스 노트](https://helpx.adobe.com/experience-manager/6-5/release-notes/wcm-platform.html#Security)

***✔은 이 버전의 기능이 크게 향상된 것을 나타냅니다.***

***✔<sup>+</sup> 는 서비스 팩 또는 기능 팩을 통해 사용할 수 있음을 나타냅니다.***

<table>
    <thead>
        <tr>
            <td>보안 기능</td>
            <td>5.6.x</td>
            <td>6.0</td>
            <td>6.1</td>
            <td>6.2</td>
            <td>6.3</td>
            <td>6.4</td>
            <td>6.5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-service-users.html" target="_blank">서비스 사용자</a></strong>
            <br> 를 구분하면 권한이 분리되므로 관리자 권한이 필요하지 않습니다.</td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/kr/experience-manager/6-5/sites/administering/using/saml-2-0-authenticationhandler.html" target="_blank">키 저장소 </a></strong>
            <br> 관리Global 신뢰 저장소, 인증서 및 키가 저장소 내에서 모두 관리됩니다.</td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><a href="https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/csrf-protection.html" target="_blank"><strong></strong> <strong></strong></a>
            <br> CSRFprotectionCross-Site Request Forgery 보호를 즉시 사용할 수 있습니다.</td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/saml-2-0-authenticationhandler.html" target="_blank"><strong></strong> <strong></strong></a>
            <br> CORSsupportCORS(Cross-Origin Resource Sharing)를 통해 애플리케이션 유연성을 높일 수 있습니다.</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://docs.adobe.com/docs/en/aem/6-5/administer/security/saml-2-0-authenticationhandler.html" target="_blank">향상된 SAML 인증 </a><br>
 </strong>지원SAML 리디렉션, 최적화된 그룹 정보 및 키 암호화 문제가 해결되었습니다. 
            <br>
        </td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ldap-config.html" target="_blank">LDAP as a OSGi </a><br>
 </strong>ConfigurationLDAP 인증의 관리 및 업데이트를 단순화합니다.</td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong>일반 텍스트 암호에 대한 OSGi 암호화 <br>
 </strong>지원암호 및 기타 중요한 값을 암호화된 형태로 저장하고 자동으로 해독할 수 있습니다.</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/user-group-ac-admin.html" target="_blank">CUG </a><br>
 </strong>개선 사항Closed-User Group 구현이 성능 및 확장성 문제를 해결하기 위해 다시 작성되었습니다.</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔<sup>+</sup></td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/platform-repository/using/ssl-wizard-technical-video-use.html" target="_blank">SSL</a></strong>
            <br> 의 설정 및 관리를 간소화하는 SSL 마법사 UI.</td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/encapsulated-token.html" target="_blank">캡슐화된 토큰 </a></strong>
            <br> 지원게시 인스턴스 간에 수평 인증을 지원하기 위해 더 이상 "고정" 세션이 필요하지 않습니다.</td>
        <td> </td>
        <td> </td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ims-config-and-admin-console.html" target="_blank">Adobe IMS 인증 </a><br>
 </strong>지원AMS(Adobe Managed Services)만 지원하며, Adobe IMS(Identity Management 시스템)를 통해 AEM 작성자 인스턴스에 대한 액세스를 중앙에서 관리합니다.</td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td>✔</td>
        <td>✔</td>
    </tr>
</tbody>
</table>

## 사이트 기능

다음은 AEM에서 제공하는 주요 Sites 기능의 표입니다. 이러한 기능 중 일부는 각 릴리스에 추가된 이전 버전의 증분 개선 사항에서 도입되었습니다.

+ [AEM Sites 릴리스 노트](https://helpx.adobe.com/experience-manager/6-5/release-notes/sites.html)

***✔<sup>+</sup> 이 버전의 기능에 대한 중요한 개선 사항입니다.***

***✔<sup></sup> SP는 서비스 팩 또는 기능 팩을 통해 사용할 수 있음을 나타냅니다.***

<table>
    <thead>
        <tr>
            <td><strong>사이트 기능</strong></td>
            <td>5.6.x</td>
            <td>6.0</td>
            <td>6.1</td>
            <td>6.2</td>
            <td>6.3</td>
            <td>6.4</td>
            <td>6.5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/page-editor-feature-video-use.html" target="_blank">터치에 적합한 페이지 작성</a>:</strong>
            편집기에서 터치 화면이 있는 태블릿과 컴퓨터를 활용할 수 있습니다.</td>
            <td></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/responsive-layout.html" target="_blank">응답형 사이트 작성</a>:</strong>
                레이아웃 모드를 사용하면 편집기에서 응답형 사이트의 장치 너비를 기반으로 구성 요소의 크기를 조정할 수 있습니다.</td>
            <td></td>
            <td></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/template-editor-feature-video-use.html" target="_blank">편집 가능한 템플릿</a>:</strong>
            전문 작성자가 페이지 템플릿을 만들고 편집할 수 있도록 해줍니다.</td>
            <td></td>
            <td></td>
            <td></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/core-components/user-guide.html" target="_blank">핵심 구성 요소</a>: </strong>
            사이트 개발을 가속화합니다. GitHub에서 자주 릴리스 일정과 유연성을 제공합니다.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/spa-overview.html" target="_blank">SPA 편집기</a>:</strong>
            React 또는 Angular을 기반으로 구축된 SPA(단일 페이지 애플리케이션) 프레임워크를 사용하여 작성 가능한 웹 경험을 생성합니다.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/kr/experience-manager/6-5/release-notes/style-system-fp.html" target="_blank">스타일 시스템</a>:</strong>
            컨텍스트 내 스타일 시스템을 사용하여 AEM 구성 요소의 시각적 모양을 정의하여 구성 요소를 다시 사용합니다.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>SP</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/kr/experience-manager/6-5/sites/administering/using/msm.html" target="_blank">MSM(Multi-Site Manager)</a>:</strong>
            일반적인 컨텐츠(즉, 다국어, 여러 브랜드)를 공유하는 여러 웹 사이트를 관리합니다.</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/translation.html" target="_blank">컨텐츠 번역</a>:</strong>
            플러그인 및 플레이 프레임워크는 업계 최고의 타사 번역 서비스와 통합됩니다.</td>
            <td></td>
            <td></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/contexthub.html" target="_blank">ContextHub</a>:</strong>
            콘텐츠의 개인화를 위한 차세대 클라이언트 컨텍스트 프레임워크.</td>
            <td></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/launches.html" target="_blank">론치</a>:</strong>
            일상적인 작성을 중단하지 않고 향후 릴리스를 위한 컨텐츠를 개발합니다.</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/content-fragments-feature-video-understand.html" target="_blank">컨텐츠 조각</a>:</strong>
            프레젠테이션에서 편집 가능한 컨텐츠를 만들어 조정하면 쉽게 재사용할 수 있습니다.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/experience-fragments-feature-video-use.html" target="_blank">경험 조각</a>:</strong>
            데스크탑, 모바일 및 소셜 채널에 최적화된 재사용 가능한 경험 및 변형을 만듭니다.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/release-notes/content-services-fragments-featurepack.html" target="_blank">컨텐츠 서비스</a>: </strong>
            장치 및 애플리케이션 간에 소비할 수 있도록 AEM의 컨텐츠를 JSON으로 내보냅니다.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>SP</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Adobe Analytics 통합 및 컨텐츠 인사이트: </strong>
                Adobe Analytics과 DTM을 쉽게 통합합니다. 작성 환경 내에 성능 정보를 표시합니다.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/content-targeting-touch.html" target="_blank">Adobe Target 통합</a>:</strong>
            타깃팅된 경험을 만들고 재사용 가능한 오퍼 라이브러리를 만들기 위한 단계별 마법사.</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/campaign.html" target="_blank">Adobe Campaign 통합</a>:</strong>
            차세대 이메일 캠페인 솔루션과 손쉽게 통합</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/using/aem_launch_adobeio_integration.html" target="_blank">Adobe Launch 통합</a>:</strong>
            Adobe의 차세대 태그 관리 클라우드 서비스와 통합합니다.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/aem-screens-introduction.html" target="_blank">스크린</a>:</strong>
            디지털 간판 및 키오스크에 대한 경험을 관리합니다.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ecommerce.html" target="_blank">eCommerce</a>:</strong>
            웹, 모바일 및 소셜 터치포인트 간에 브랜딩되고 개인화된 쇼핑 경험을 제공합니다.
            </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/overview.html" target="_blank">커뮤니티</a>:</strong>
            포럼, 스레드 주석, 이벤트 달력 및 기타 많은 기능을 통해 사이트 방문자와 깊이 관여할 수 있습니다.</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
    </tbody>
</table>

## 자산 기능

다음은 AEM에서 제공하는 주요 Assets 기능 매트릭스입니다. 이러한 기능 중 일부는 각 릴리스에 추가된 이전 버전의 증분 개선 사항에서 도입되었습니다.

+ [AEM Assets 릴리스 노트](https://helpx.adobe.com/experience-manager/6-5/release-notes/assets.html)

***✔은 이 버전의 기능이 크게 향상된 것을 나타냅니다.***

***✔<sup>+</sup> 는 서비스 팩 또는 기능 팩을 통해 사용할 수 있음을 나타냅니다.***

<table>
    <thead>
        <tr>
            <td>자산 기능</td>
            <td>5.6.x</td>
            <td>6.0</td>
            <td>6.1</td>
            <td>6.2</td>
            <td>6.3</td>
            <td>6.4</td>
            <td>6.5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/managing-assets-touch-ui.html" target="_blank">터치에 적합한 UI</a>:</strong>
            데스크탑 컴퓨터 또는 터치 지원 장치에서 자산을 관리합니다.</td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/metadata.html" target="_blank">고급 메타데이터 관리</a>: </strong>
            메타데이터 템플릿, 메타데이터 스키마 편집기 및 벌크 메타데이터 편집.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/task-content.html" target="_blank"></a> 작업 및  <a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/projects-with-workflows.html" target="_blank"></a> 워크플로우 관리:</strong>
             AEM 프로젝트를 활용하는 디지털 자산을 검토하고 승인하는 데 필요한 사전 빌드된 워크플로우 및 작업입니다.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>확장성 및 성능: </strong>
            수집, 업로드 및 규모에 대한 지원이 향상되었습니다.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/mac-api-assets.html" target="_blank">Assets HTTP API</a>:</strong>
            HTTP 및 JSON을 통해 프로그래밍 방식으로 자산과 상호 작용합니다.</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/link-sharing.html" target="_blank">링크 공유</a>:</strong>
            로그인하지 않고도 디지털 자산을 임시로 공유하는 간단한 방법입니다.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/brand-portal/using/brand-portal.html" target="_blank">Brand Portal</a>:</strong>
            디지털 자산의 원활한 공유 및 배포를 위한 클라우드 서비스 SAAS 솔루션입니다.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/use-assets-across-connected-assets-instances.html" target="_blank">연결된 자산</a>:</strong>
            AEM Sites 인스턴스는 다른 AEM Assets 인스턴스의 자산에 원활하게 액세스하고 사용할 수 있습니다.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/touch-ui-asset-insights.html" target="_blank">자산 통찰력</a>: </strong>
            Adobe Analytics을 활용하여 AEM에서 디지털 자산 및 보기의 고객 상호 작용을 캡처합니다.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/multilingual-assets.html" target="_blank">다국어 자산</a>:</strong>
            언어 루트로 자산 메타데이터를 자동으로 번역할 수 있습니다.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/enhanced-smart-tags.html" target="_blank">스마트 태그 및 중재</a>: </strong>
            Adobe Sensei을 활용하여 유용한 메타데이터로 이미지에 자동으로 태그를 지정합니다.</td>
            <td> </td>
            <td></td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/assets/using/smart-translation-search-feature-video-use.html" target="_blank">스마트 번역 검색</a>:</strong>
            AEM Assets을 검색할 때 검색어를 자동으로 번역합니다.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/indesign.html" target="_blank">Adobe InDesign Server 통합</a>:</strong>
            제품 카탈로그를 생성합니다. InDesign 템플릿을 기반으로 브로셔, 전단 및 인쇄 광고를 만들 수 있습니다.</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/kr/experience-manager/desktop-app/aem-desktop-app.html" target="_blank">AEM 데스크탑 앱</a>:</strong>
            Creative Suite 제품으로 편집하려면 자산을 로컬 데스크탑에 동기화하십시오.
            </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/imaging-transcoding-library.html" target="_blank">Adobe 이미징 라이브러리</a>: </strong>
                <br> 고품질 파일 조작에 사용되는 Photoshop 및 Acrobat PDF 라이브러리입니다.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://www.adobe.com/kr/creativecloud/business/enterprise/adobe-asset-link.html" target="_blank">Adobe 자산 링크</a>: </strong>
            Adobe 만들기 클라우드 애플리케이션에서 직접 AEM Assets에 액세스합니다.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/aem-assets-adobe-stock.html" target="_blank">Adobe Stock 통합</a>:</strong>
            AEM에서 직접 Adobe Stock 이미지에 원활하게 액세스하고 사용합니다.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>SP</sup></td>
            <td>✔</td>
        </tr>
    </tbody>
</table>

### AEM Assets Dynamic Media

***✔<sup>+</sup> 이 버전의 기능에 대한 중요한 개선 사항입니다.***

***✔<sup></sup> SP는 서비스 팩 또는 기능 팩을 통해 사용할 수 있음을 나타냅니다.***


<table>
    <thead>
        <tr>
            <td>Dynamic Media 기능</td>
            <td>5.6.x</td>
            <td>6.0</td>
            <td>6.1</td>
            <td>6.2</td>
            <td>6.3 +FP</td>
            <td>6.4</td>
            <td>6.5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/managing-assets.html" target="_blank">이미징</a>:</strong>
            스마트 자르기를 포함하여 다양한 크기 및 형식으로 이미지를 동적으로 제공합니다.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/video-profiles.html" target="_blank">비디오</a>: </strong>
            고급 비디오 인코딩 및 응용 비디오 스트리밍</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/interactive-images.html" target="_blank">대화형 미디어</a>:</strong>
            클릭 가능한 콘텐츠로 대화형 배너, 비디오를 만들어 주요 오퍼를 보여줍니다.
            </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>세트(<a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/image-sets.html" target="_blank">이미지</a>,  <a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/spin-sets.html" target="_blank">스핀</a>,  <a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/mixed-media-sets.html" target="_blank">혼합 미디어</a>):</strong>
            사용자가 360도 보기 환경을 확대/축소, 패닝, 회전 및 시뮬레이션할 수 있도록 해줍니다.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://docs.adobe.com/docs/ko-KR/aem/6-5/administer/content/dynamic-media/viewer-presets.html" target="_blank">뷰어</a>:</strong>
            다양한 화면/장치를 지원하는 사용자 지정 브랜드 리치 미디어 플레이어 및 사전 설정.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/delivering-dynamic-media-assets.html" target="_blank">배달</a>:</strong>
            HTTP/2 프로토콜을 통해 Dynamic Media 컨텐츠 및 전달을 연결 또는 포함할 수 있는 유연한 옵션입니다.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Scene7에서 Dynamic Media으로 업그레이드: </strong>
            마스터 자산을 마이그레이션하고 기존 S7 URL을 계속 사용하는 기능.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
    </tbody>
</table>

## Forms 기능

다음은 AEM에서 제공하는 주요 AEM Forms 추가 기능 매트릭스입니다. 이러한 기능 중 일부는 각 릴리스에 추가된 이전 버전의 증분 개선 사항에서 도입되었습니다.

+ [AEM Forms 릴리스 노트](https://helpx.adobe.com/experience-manager/6-5/release-notes/forms.html)

***✔<sup>+</sup> 이 버전의 기능에 대한 중요한 개선 사항입니다.***

***✔<sup></sup> SP는 서비스 팩 또는 기능 팩을 통해 사용할 수 있음을 나타냅니다.***

<table>
    <thead>
        <tr>
            <td>Forms 기능</td>
            <td>5.6.x</td>
            <td>6.0</td>
            <td>6.1</td>
            <td>6.2</td>
            <td>6.3</td>
            <td>6.4</td>
            <td>6.5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/introduction-forms-authoring.html" target="_blank">적응형 Forms 편집기</a>:</strong>
            장치 및 브라우저 설정을 기반으로 매력적인 반응형 및 적응형 양식을 만듭니다.</td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/generate-document-of-record-for-non-xfa-based-adaptive-forms.html" target="_blank">기록 문서</a>:</strong>
            데이터 캡처 환경을 장기 보관하거나 준비 버전을 인쇄하도록 문서를 만듭니다.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/themes.html" target="_blank">테마 편집기</a>:</strong>
            양식의 구성 요소 및 패널에 스타일을 지정하는 재사용 가능한 테마를 만듭니다.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/template-editor.html" target="_blank">템플릿 편집기</a>:</strong>
            적응형 양식에 대한 우수 사례를 표준화하고 구현합니다.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#EnhancedintegrationwithAdobeSign" target="_blank">Adobe Sign 통합</a>:</strong>
            Adobe Sign 통합 양식 기반의 서명 시나리오를 배포할 수 있습니다.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/cm-overview.html" target="_blank">서신 관리</a>:</strong>
            AEM Forms을 사용하면 개인화된 대화형 고객 서신을 작성, 관리 및 전달할 수 있습니다.
            </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#AEMFormsdataintegration" target="_blank">타사 데이터 통합</a>:</strong>
             데이터 통합을 사용하여 양식의 사용자 입력을 기반으로 서로 다른 데이터 소스에서 데이터를 가져옵니다. 양식 제출 시 캡처된 데이터는 데이터 소스에 다시 기록됩니다.
            </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#FormscentricAEMWorkflowsforAEMFormsonOSGi" target="_blank">Forms 처리를 위한 워크플로우(OSGi)</a>: </strong>
            양식 승인 프로세스를 간단하게 배포합니다.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/user-guide.html?topic=/experience-manager/6-5/forms/morehelp/integrations.ug.js" target="_blank">Marketing Cloud과 통합</a>:</strong>
            Adobe Analytics 및 Adobe Target과 통합하여 고객 경험을 향상하고 측정합니다.</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/introduction-managing-forms.html" target="_blank">Form Manager</a>:</strong>
            분석, 번역, A/B 테스트, 검토 및 게시 활성화와 같은 모든 양식/문서/서신을 관리하는 단일 위치입니다.
            </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/aem-forms-app.html" target="_blank">AEM Forms 앱</a>:</strong>
             iOS, Android 또는 Windows의 앱 내에서 온라인/오프라인 양식 처리를 허용합니다.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/adaptive-document.html" target="_blank">인터랙티브한 통신</a>:</strong>
            차트와 같은 대화형 요소를 사용하여 타깃팅된 문장과 같은 풍부한 통신을 만듭니다(이전의 적응형 문서).</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/pdf/aem-forms/6-5/WorkbenchHelp.pdf" target="_blank">Forms 처리를 위한 워크플로우(J2EE)</a>:</strong>
             직관적인 IDE를 사용하여 복잡한 양식/문서 중심의 워크플로우를 빌드합니다.</td>
            <td></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#EnhancedDocumentSecurity" target="_blank">AEM Forms 문서 보안</a>:</strong>
            PDF 및 Office 문서의 안전한 액세스 및 권한 부여.
            </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#Simplifiedauthoringexperience" target="_blank">프레임워크 테스트</a>:</strong>
            Calvin 프레임워크 및 Chrome 플러그인을 사용하여 적응형 양식을 지원하고 디버깅합니다.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
    </tbody>
</table>

## 커뮤니티 기능

다음은 AEM에서 제공하는 주요 AEM Communities 추가 기능 매트릭스입니다. 이러한 기능 중 일부는 각 릴리스에 추가된 이전 버전의 증분 개선 사항에서 도입되었습니다.

+ [AEM Communities의 새로운 기능 요약](https://helpx.adobe.com/experience-manager/6-5/communities/using/whats-new-aem-communities.html#main-pars_text)

***✔<sup>+</sup> 이 버전의 기능에 대한 중요한 개선 사항입니다.***

***✔<sup></sup> SP는 서비스 팩 또는 기능 팩을 통해 사용할 수 있음을 나타냅니다.***

<table>
    <thead>
        <tr>
            <td> </td>
            <td>커뮤니티 기능</td>
            <td>6.0</td>
            <td>6.1</td>
            <td>6.2</td>
            <td>6.3</td>
            <td>6.4</td>
            <td>6.5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td rowspan="7">커뮤니티 함수</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/forum.html" target="_blank">포럼</a>:</strong>  (소셜 구성 요소 프레임워크) 새 주제를 만들거나 기존 항목을 보고, 따르고, 검색하고, 이동합니다.</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td>
                <p><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-qna.html" target="_blank">QnA</a>:</strong>
                 질문, 보기 및 답변합니다.</p>
            </td>
            <td></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td>
                <strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/blog-feature.html" target="_blank">블로그</a>:</strong>
                게시 측에서 블로그 문서와 주석을 만듭니다.
            </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td>
                <strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/ideation-feature.html" target="_blank">아이디어</a>:</strong>
                커뮤니티에 아이디어를 만들어 공유하거나, 기존 아이디어를 보고, 따르고, 댓글을 달습니다.
            </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td>
                <strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/calendar.html" target="_blank">달력</a>:</strong>
                 (소셜 구성 요소 프레임워크) 사이트 방문자에게 커뮤니티 이벤트 정보를 제공합니다.
            </td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td>
                <strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/file-library.html" target="_blank">파일 라이브러리</a>:</strong>
                커뮤니티 사이트 내에서 파일을 업로드, 관리 및 다운로드합니다.</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/users.html#AboutCommunityGroups" target="_blank">사용자 그룹</a>:
            </strong>사용자 집합은 구성원 그룹에 속할 수 있으며, 집합적으로 역할을 지정할 수 있습니다.</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong> </strong></td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/resource.html" target="_blank">배정</a>:</strong>
            학습 리소스를 만들고 커뮤니티 구성원에게 할당합니다.</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td rowspan="5">지원</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/catalog.html" target="_blank"></a> 카탈로그 및  <a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/resource.html" target="_blank">리소스 관리</a>:</strong>
            카탈로그에서 지원 리소스에 액세스합니다.</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/resources.html#CreateaLearningPath" target="_blank">학습 경로 관리</a>:</strong>
            교육 과정 또는 지원 리소스 그룹을 관리합니다.</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/reports.html#main-pars_text_1739724213" target="_blank">지원 보고</a>:</strong>
            지원 리소스 및 학습 경로에 대해 보고합니다.</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/resources.html#main-pars_text_899882038" target="_blank">지원 시 참여</a>: </strong>
            지원 리소스에 주석을 추가합니다.</td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/analytics.html" target="_blank">지원 Analytics</a>: </strong>
            Video Analytics, 진행률 보고 및 할당 보고</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td rowspan="8">Commons</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/comments.html" target="_blank"></a> 주석 및 첨부 파일:</strong>
             (Social Component Framework) 커뮤니티 회원은 커뮤니티 사이트의 콘텐츠에 대한 의견 및 지식을 공유합니다.</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>컨텐츠 조각 변환: </strong>
            UGC 기여도를 컨텐츠 조각으로 변환합니다.</td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/reviews.html" target="_blank">검토</a>:</strong>
                 (소셜 구성 요소 프레임워크) 커뮤니티 구성원은 댓글 및 등급 함수의 조합을 사용하여 컨텐츠 부분을 검토합니다.</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/rating.html" target="_blank">등급</a>:/strong&gt;(소셜 구성 요소 프레임워크) 커뮤니티 구성요소로 컨텐츠의 일부를 평가합니다.</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/voting.html" target="_blank">투표</a>:</strong>
                 (소셜 구성 요소 프레임워크) 커뮤니티 구성원이 한 콘텐츠를 업로드하거나 다운로드합니다.</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/tag-ugc.html" target="_blank">태그</a>:</strong>
             컨텐츠에 태그(키워드 또는 레이블)를 첨부하여 컨텐츠를 빠르게 찾습니다.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/search.html" target="_blank">검색</a>: </strong>
            예측 및 암시적 검색.</td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/translate-ugc.html" target="_blank">번역</a>:</strong>
            사용자가 생성한 콘텐츠의 기계 번역입니다.</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td rowspan="10">관리</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/create-site.html" target="_blank">사이트 관리</a>:</strong>
            커뮤니티 기능을 사용하여 사이트 만들기</td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/sites.html" target="_blank">템플릿</a>:</strong>
                <a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/sites.html" target="_blank"></a> 마법사 기반 <a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/tools-groups.html" target="_blank"></a> 의 완전한 기능 커뮤니티 사이트 생성을 위한 사이트 및 그룹 템플릿입니다.</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>편집 가능한 템플릿: </strong>
            커뮤니티 관리자가 AEM 편집 가능 템플릿을 사용하여 풍부한 경험을 구축할 수 있습니다.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/creating-groups.html" target="_blank">그룹 또는 하위 커뮤니티</a>:</strong>
            커뮤니티 사이트 내에서 하위 커뮤니티를 동적으로 만듭니다.
            </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/in-context.html" target="_blank">중재</a>: </strong>
            사용자가 생성한 컨텐츠 중재.
            </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/moderation.html" target="_blank">벌크 중재</a>: </strong>
            사용자 생성 컨텐츠를 벌크 관리할 중재 콘솔.</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/moderate-ugc.html#CommonModerationConcepts" target="_blank">스팸 감지 및 불경성 필터</a>:</strong>
            자동 스팸 감지</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/members.html" target="_blank">구성원 관리</a>:</strong>
            구성원 관리 영역에서 사용자 프로필 및 그룹을 관리합니다.</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/overview.html#main-pars_text_866731966" target="_blank">반응형 디자인</a>: </strong>
            AEM Communities 사이트는 반응형 사이트입니다.
            </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/analytics.html" target="_blank">Analytics</a>:</strong>
            커뮤니티 사이트 사용에 대한 주요 통찰력을 위해 Adobe Analytics과 통합합니다.</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td rowspan="4">구성원</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/advanced.html" target="_blank">점수 책정 및 배지</a>:</strong>
             (Adobe Sensei에서 제공하는 고급 점수) 커뮤니티 구성원을 전문가로 식별하고 포상합니다.</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/activities.html" target="_blank"></a> 활동 및  <a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/notifications.html" target="_blank">알림</a>:</strong>
            최근 활동 스트림을 보고 관심 이벤트에 대한 알림을 받습니다.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/configure-messaging.html" target="_blank">메시지</a>:</strong>
            사용자 및 그룹에 대한 직접 메시지.</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/social-login.html" target="_blank">Social 로그인</a>:</strong>
            Facebook 또는 Twitter 계정으로 로그인합니다.</td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td rowspan="5">플랫폼</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-srp.html" target="_blank">MSRP(Mongo Storage)</a>:</strong>
            사용자 생성 콘텐츠(UGC)는 로컬 MongoDB 인스턴스에 직접 유지됩니다</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-srp.html" target="_blank">DSRP(데이터베이스 저장소)</a>:</strong>
            사용자 생성 콘텐츠(UGC)는 로컬 MySQL 데이터베이스 인스턴스에 직접 유지됩니다.</td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-srp.html" target="_blank">SRP(클라우드 스토리지)</a>:</strong>
                사용자 생성 콘텐츠(UGC)는 Adobe에서 호스팅 및 관리하는 클라우드 서비스에서 원격으로 유지됩니다.</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-srp.html" target="_blank"><strong>JSRP</a>:</strong>
                커뮤니티 컨텐츠는 JCR에 저장되며 UGC는 게시된 작성자(또는 게시) 인스턴스에서 액세스할 수 있습니다.</td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/sync.html" target="_blank">사용자 및 그룹 동기화</a>:</strong>
             게시 팜 토폴로지를 사용할 때 게시 인스턴스 간에 사용자 및 그룹을 동기화합니다.</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
    </tbody>
</table>

AEM Communities은 조직에서 다음 방법으로 사용자를 참여시키고 활성화할 수 있도록 릴리스에 [개선 사항](https://helpx.adobe.com/experience-manager/6-5/communities/using/whats-new-aem-communities.html)을 추가합니다.

+ **사용자** 생성 콘텐츠의 @mentionsupport입니다.
+ **지원** 구성 요소의 **키보드 탐색**&#x200B;을 통해 액세스 가능성이 개선되었습니다.
+ **사용자 지정 필터**&#x200B;를 사용하여 **대량 조정**&#x200B;이 개선되었습니다.
+ **편집** 가능한 템플릿 을 사용하면 커뮤니티 관리자가 AEM에서 풍부한 커뮤니티 경험을 구축할 수 있습니다.
+ 이제 사용자는 그룹의 모든 구성원에게 **직접 메시지를 일괄**&#x200B;로 보낼 수 있습니다.
