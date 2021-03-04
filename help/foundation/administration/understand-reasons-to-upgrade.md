---
title: 업그레이드해야 하는 이유 이해
description: 최신 버전의 Adobe Experience Manager으로 업그레이드하는 것을 고려 중인 고객을 위한 주요 기능의 상세 분류.
version: 6.5
sub-product: 자산, 클라우드 관리자, 커머스, 컨텐츠 서비스, 다이내믹 미디어, 양식, 기반, 스크린, 사이트
topics: best-practices, upgrade
audience: all
activity: understand
doc-type: article
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '3540'
ht-degree: 3%

---


# 업그레이드해야 하는 이유 이해

최신 버전의 Adobe Experience Manager으로 업그레이드하는 것을 고려 중인 고객을 위한 주요 기능의 상세 분류.

## AEM 6.5로 업그레이드하기 위한 주요 기능

+ [Adobe Experience Manager 6.5 릴리스 노트](https://helpx.adobe.com/kr/experience-manager/6-5/release-notes.html)

### 향상된 기본 기능

Adobe Experience Manager 6.5는 다음을 통해 시스템의 안정성, 성능 및 지원 기능을 지속적으로 향상시켜 줍니다.

+ **Java 11** 지원(Java 8 지원 유지 관리).

### 웹 사이트 제작 및 관리

AEM Sites은 웹 사이트 제작 및 빌드를 가속화하기 위해 고안된 다양한 기능을 제공합니다.

+ **SPA** Editorsupport를 통해 SPA(단일 페이지 애플리케이션)는 AEM에서 완벽하게 제작되므로 마케터에게 익숙한 리치 저작 경험을 지원할 수 있습니다.
+_ **JavaScript SDK의**, SPA 프로젝트 시작 키트 및 지원 빌드 도구를 사용하여 프런트 엔드 개발자는 AEM과 독립적으로 SPA 편집기와 호환되는 단일 페이지 애플리케이션을 개발할 수 있습니다.
+ **핵심** 구성 요소에는 다양한 새 구성 요소,  **구성 요소 라이브러리** 및 기존 핵심 구성 요소에 대한 다양한 개선 사항이 포함되어 있습니다.
+ 또한 **번역**&#x200B;개선 사항을 통해 AEM Sites의 번역이 능률화됩니다.

### 유연한 경험

AEM은 AEM 외부에 있는 컨텐츠를 손쉽게 사용할 수 있는 새롭고 향상된 툴을 통해 유동적인 경험을 지속적으로 수용하고 있습니다.

+ **컨텐츠** 조각은 버전 비교/비교 및 주석을 지원합니다.
+ **AEM Assets HTTP** API는 DAM에서 바로  **컨텐츠 조각** 을 JSON **으로 표시할 수 있습니다**.
   **경험** 조각 **은 페이지** 를 참조하기 위해  **전체 텍스트** 검색 및  **AEM Dispatcher**&#x200B;캐시 무효화를지원합니다.

### 자산 관리

AEM Assets은 DAM의 사용, 관리 및 이해를 개선하기 위해 풍부한 에셋 관리 기능을 기반으로 계속 구축하고 있습니다. AEM 6.5는 Adobe Creative Cloud과 크리에이티브 워크플로우 간의 통합을 계속 개선합니다.

+ **Adobe Asset** Link는 Adobe Creative Cloud 툴에서 크리에이티브 전문가를 AEM Assets에 바로 연결할 수 있습니다.
+ **Adobe** Stockintegration을 사용하면 AEM Assets 환경에서 바로 Adobe Stock 이미지에 액세스할 수 있으므로 컨텐츠를 매끄럽게 검색할 수 있습니다.
+ **AEM Desktop** Improps 버전 2.0을 다시 구상하고 성능과 안정성을 개선합니다.
+ **연결된** 자산은 서로 다른 AEM Assets 인스턴스의 에셋에 원활하게 액세스하고 사용할 수 있도록 개별 AEM Sites 인스턴스를 지원합니다.
+ **Dynamic Media**&#x200B;에서 **360 비디오** 및 **사용자 지정 비디오 축소판**&#x200B;을(를) 포함하여 비디오 지원을 업데이트했습니다.

### 콘텐츠 인텔리전스

AEM은 모든 경험을 개선하기 위해 머신 러닝과 인공 지능을 활용하여 스마트 기술과의 통합을 지속적으로 구축하고 있습니다.

+ **Adobe Asset** Linkes는  **시각적 유사성 검색** 기능을 추가하므로  **Adobe Creative Cloud 도구** 내에서 유사한 이미지를 쉽게 검색하여 사용할 수 있습니다.

### 통합

AEM은 다른 Adobe 서비스와 통합할 수 있는 역량을 강화하고 있습니다.

+ **경험** 조각은 JSON을 Adobe Target으로  **내보내기** 를 지원하고, Adobe Target에서 Experience Fragment-based를 Experience Fragment-based Offerspost를  **삭제하** 는 기능을 지원함으로써 Adobe Target과의 통합을  ****   ****&#x200B;더욱 강화합니다.

### AMS Cloud Manager

[Adobe AMS(Managed Services) 고객을 위한 Cloud Manager](https://adobe.ly/2HODmsv) 기능은 다음과 같습니다.

+ Cloud Manager는 자산 처리&#x200B;**의 자동 성능 테스트를 포함하여 AEM Sites에서** AEM Assets **까지 AEM 배포 지원을 확장합니다.**
+ **사전** 정의된 임계값에서 AEM 게시 계층의 자동 크기 조정을 통해 최적의 최종 사용자 환경을 보장합니다.
+ **비프로덕션** 파이프라인을 통해 개발 팀은 Cloud Manager를 활용하여 코드 품질을 지속적으로 확인하고 하위 환경(개발 및 QA)에 배포할 수 있습니다.
+ **CI/CD Pipeline** APIsallow 고객은 Cloud Manager를 프로그래밍 방식으로 사용하여 사내 개발 인프라와의 통합을 더욱 강화할 수 있습니다.

## 기본 기능

다음은 AEM에서 제공하는 주요 기본 기능의 매트릭스입니다. 이러한 기능 중 일부는 각 릴리스에 추가된 이전 버전의 증분 개선 사항에서 도입되었습니다.

+ [AEM Foundation 릴리스 노트](https://helpx.adobe.com/experience-manager/6-5/release-notes/wcm-platform.html)

***✔<sup>+</sup> 이 버전의 기능이 크게 향상되었습니다.***

***✔<sup></sup> SP는 서비스 팩 또는 기능 팩을 통해 해당 기능을 사용할 수 있음을 나타냅니다.***

<table>
    <thead>
        <tr>
            <td>기본 기능</td>
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
                <strong>Java 11 지원: </strong> AEM은 Java 11(및 Java 8)을 지원합니다.
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
                <strong><a href="https://jackrabbit.apache.org/oak/docs/index.html" target="_blank">Oak 컨텐츠 저장소</a>:</strong> 이전의 Jackrabbit 2보다 뛰어난 성능과 확장성을 제공합니다.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/indexing-via-the-oak-run-jar.html">oak-run.jar 인덱스 지원</a>:</strong> Oak 인덱스의 다시 색인 작성, 통계 수집 및 일관성 확인 기능이 개선되었습니다.</td>
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
                사용자 정의 색인 정의를 추가하여 쿼리 성능을 최적화하고 관련성을 검색하는 기능</td>
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/storage-elements-in-aem-6.html" target="_blank">TarMK 또는 MongoMK 리포지토리</a>:</strong>
                <br> Options는 TarMK(차세대 버전의 TarPM)의 간단한 성능 파일 기반 저장 공간
                <br> 을 사용하거나 MongoMK가 지원되는 MongoDB 저장소를 사용하여 가로로 확장할 수 있습니다.</td>
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
            AEM 6.0이 소개되면서 MongoMK는 계속 향상되었습니다.</td>
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
            확장 가능한 클라우드 스토리지 솔루션을 활용하여 이진 자산을 저장합니다.</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>터치 UI 기능 패리티: </strong>
                향상된 생산성과 클래식 UI와의 기능 패리티 덕분에 UI 작성의 속도를 계속 향상시킬 수 있습니다.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Omnisearch: </strong>
                신속하게 AEM 검색 및 탐색</td>
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
 AEM 내에서 유지 관리를 수행하고 서버 상태를 모니터링하며 성능을 분석합니다.</td>
            <td></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/upgrade.html" target="_blank">업그레이드 개선</a> 사항: </strong>
            업그레이드 기능을 통해 AEM을 보다 빠르고 손쉽게 업그레이드할 수 있습니다.</td>
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
            프레젠테이션과 논리를 구분하는 최신 템플릿 엔진입니다. 구성 요소 개발 시간을 크게 줄입니다. 각 릴리스에 추가 기능</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://sling.apache.org/documentation/bundles/models.html" target="_blank">Sling Models</a>:</strong>
            JCR 리소스를 비즈니스 개체 및 로직에 모델링할 수 있는 유연한 프레임워크입니다. 각 릴리스에 추가 기능
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
            <td><strong><a href="https://adobe.ly/2HODmsv" target="_blank">클라우드 관리자</a>: </strong>
                Adobe AMS(Managed Services) 고객을 위한 Cloud Manager는 첨단 CI/CD 파이프라인을 통해 개발 및 배포 시간을 단축시켜 줍니다.</td>
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

다음은 AEM에서 제공하는 주요 보안 기능의 매트릭스입니다. 이러한 기능 중 일부는 각 릴리스에 추가된 이전 버전의 증분 개선 사항에서 도입되었습니다.

+ [보안 릴리스 노트](https://helpx.adobe.com/experience-manager/6-5/release-notes/wcm-platform.html#Security)

***✔은 이 버전의 기능이 크게 개선되었음을 나타냅니다.***

***✔<sup>+</sup> 는 서비스 팩 또는 기능 팩을 통해 해당 기능을 사용할 수 있음을 나타냅니다.***

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
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-service-users.html" target="_blank">서비스 </a></strong>
            <br> 사용자 구획된 권한을 통해 관리자 권한이 필요하지 않습니다.</td>
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
            <br> 관리전역 신뢰 저장소, 인증서 및 키는 모두 저장소 내에서 관리됩니다.</td>
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
            <br> CSRFprotectionCross-Site 요청 위조 보호를 즉시 제공합니다.</td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/saml-2-0-authenticationhandler.html" target="_blank"><strong>CORSsupport</strong> <strong></strong></a>
            <br> 교차 출처 리소스 공유 지원을 통해 애플리케이션 유연성을 높일 수 있습니다.</td>
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
 </strong>지원향상된 SAML 리디렉션, 최적화된 그룹 정보 및 키 암호화 문제가 해결되었습니다. 
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
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ldap-config.html" target="_blank">LDAP를 OSGi </a><br>
 </strong>구성LDAP 인증의 관리 및 업데이트를 간소화할 수 있습니다.</td>
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
 </strong>지원암호 및 기타 중요한 값은 암호화된 형태로 저장하고 자동으로 해독할 수 있습니다.</td>
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
        <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/platform-repository/using/ssl-wizard-technical-video-use.html" target="_blank">SSL </a></strong>
            <br> 마법사 UI를 사용하여 SSL의 설정 및 관리를 간소화할 수 있습니다.</td>
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
            <br> 지원더 이상 게시 인스턴스 전체에서 가로 인증을 지원하기 위해 "고정" 세션이 필요하지 않습니다.</td>
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
 </strong>지원Adobe AMS(Managed Services)에만 국한되며 Adobe IMS(Identity Management System)를 통해 AEM 작성자 인스턴스에 대한 액세스를 중앙에서 관리할 수 있습니다.</td>
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

다음은 AEM에서 제공하는 주요 사이트 기능의 매트릭스입니다. 이러한 기능 중 일부는 각 릴리스에 추가된 이전 버전의 증분 개선 사항에서 도입되었습니다.

+ [AEM Sites 릴리스 노트](https://helpx.adobe.com/experience-manager/6-5/release-notes/sites.html)

***✔<sup>+</sup> 이 버전의 기능이 크게 향상되었습니다.***

***✔<sup></sup> SP는 서비스 팩 또는 기능 팩을 통해 해당 기능을 사용할 수 있음을 나타냅니다.***

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
            편집자가 터치스크린이 있는 태블릿 및 컴퓨터를 활용할 수 있습니다.</td>
            <td></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/responsive-layout.html" target="_blank">반응형 사이트 제작</a>:</strong>
                레이아웃 모드를 사용하면 반응형 사이트의 장치 너비를 기반으로 구성 요소의 크기를 조정할 수 있습니다.</td>
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
            전문 작성자가 페이지 템플릿을 만들고 편집할 수 있도록 허용합니다.</td>
            <td></td>
            <td></td>
            <td></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/core-components/user-guide.html" target="_blank">핵심 구성 요소</a>:</strong>
            사이트 개발 시간을 단축합니다. GitHub에서 자주 출시 일정 및 유연성을 이용할 수 있습니다.</td>
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
            반응형 또는 Angular을 기반으로 구축된 SPA(Single-Page Application) 프레임워크를 사용하여 저작 가능한 매력적인 웹 경험을 제작할 수 있습니다.</td>
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
            컨텍스트 내 스타일 시스템을 사용하여 시각적 모양을 정의하여 AEM 구성 요소를 다시 사용합니다.</td>
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
            공통 컨텐츠(예: 다중 언어, 다중 브랜드)를 공유하는 여러 웹 사이트를 관리합니다.</td>
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
            플러그인 및 재생 프레임워크는 업계 선도적인 타사 번역 서비스와 통합되어 있습니다.</td>
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
            콘텐츠 개인화를 위한 차세대 클라이언트 컨텍스트 프레임워크.</td>
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
            일상적인 저작을 방해하지 않고 향후 릴리스를 위한 컨텐츠를 개발할 수 있습니다.</td>
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
            프레젠테이션에서 편집 내용을 비결합 상태로 만들어 선별하여 손쉽게 재사용할 수 있습니다.</td>
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
            데스크탑, 모바일 및 소셜 채널에 맞게 최적화된 재사용 가능한 경험 및 변형을 만듭니다.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/release-notes/content-services-fragments-featurepack.html" target="_blank">컨텐츠 서비스</a>:</strong>
            AEM의 컨텐츠를 JSON으로 내보내 디바이스 및 애플리케이션에서 컨텐츠를 소비할 수 있습니다.</td>
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
                Adobe Analytics 및 DTM의 손쉬운 통합 작성 환경 내에서 성능 정보를 표시합니다.</td>
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
            단계별 마법사를 사용하여 타깃팅된 경험을 만들고 재사용 가능한 오퍼 라이브러리를 만들 수 있습니다.</td>
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/using/aem_launch_adobeio_integration.html" target="_blank">Adobe 실행 통합</a>:</strong>
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/aem-screens-introduction.html" target="_blank">화면</a>:</strong>
            디지털 사이니지 및 키오스크 경험을 관리합니다.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ecommerce.html" target="_blank">e커머스</a>:</strong>
            웹, 모바일 및 소셜 접점에서 브랜드화되고 개인화된 쇼핑 경험을 제공합니다.
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/overview.html" target="_blank">커뮤니티</a>: </strong>
            포럼, 스레드된 주석, 이벤트 달력 및 기타 많은 기능을 통해 사이트 방문자에 대한 참여도를 높일 수 있습니다.</td>
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

## 에셋 기능

다음은 AEM에서 제공하는 주요 자산 기능의 매트릭스입니다. 이러한 기능 중 일부는 각 릴리스에 추가된 이전 버전의 증분 개선 사항에서 도입되었습니다.

+ [AEM Assets 릴리스 노트](https://helpx.adobe.com/experience-manager/6-5/release-notes/assets.html)

***✔은 이 버전의 기능이 크게 개선되었음을 나타냅니다.***

***✔<sup>+</sup> 는 서비스 팩 또는 기능 팩을 통해 해당 기능을 사용할 수 있음을 나타냅니다.***

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
            데스크톱 컴퓨터 또는 터치 지원 장치에서 자산을 관리합니다.</td>
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/task-content.html" target="_blank">작업 </a> 및  <a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/projects-with-workflows.html" target="_blank"></a> 워크플로우 관리: </strong>
            AEM 프로젝트를 활용하는 디지털 자산을 검토 및 승인하는 미리 만들어진 워크플로우 및 작업입니다.</td>
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
            통합, 업로드 및 저장 규모에 대한 향상된 지원.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/mac-api-assets.html" target="_blank">에셋 HTTP API</a>:</strong>
            HTTP 및 JSON을 통해 프로그래밍 방식으로 에셋과 상호 작용할 수 있습니다.</td>
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
            로그인하지 않고도 디지털 자산을 간단하게 임시 공유할 수 있습니다.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/brand-portal/using/brand-portal.html" target="_blank">브랜드 포털</a>:</strong>
            클라우드 서비스 SAAS 솔루션을 통해 디지털 자산을 매끄럽게 공유하고 배포할 수 있습니다.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/use-assets-across-connected-assets-instances.html" target="_blank">연결된 에셋</a>: </strong>
            AEM Sites 인스턴스는 다른 AEM Assets 인스턴스의 에셋에 원활하게 액세스하고 사용할 수 있습니다.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/touch-ui-asset-insights.html" target="_blank">에셋 인사이트</a>:</strong>
            Adobe Analytics을 활용하여 디지털 에셋의 고객 인터랙션을 캡처하고 AEM에서 볼 수 있습니다.</td>
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
            언어 루트와 함께 에셋 메타데이터를 자동으로 변환할 수 있습니다.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/enhanced-smart-tags.html" target="_blank">스마트 태그 및 중재</a>:</strong>
            Adobe Sensei을 활용하여 유용한 메타데이터로 이미지에 자동으로 태그를 지정할 수 있습니다.</td>
            <td> </td>
            <td></td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/assets/using/smart-translation-search-feature-video-use.html" target="_blank">고급 번역 검색</a>:</strong>
            AEM Assets을 검색할 때 검색어를 자동으로 변환합니다.</td>
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
            제품 카탈로그를 생성합니다. InDesign 템플릿을 기반으로 브로셔, 전단지 및 인쇄 광고를 만들 수 있습니다.</td>
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
            Creative Suite 제품을 사용하여 편집할 수 있도록 자산을 로컬 데스크탑에 동기화합니다.
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/imaging-transcoding-library.html" target="_blank">Adobe 이미징 라이브러리</a>:</strong>
                <br> Photoshop 및 Acrobat PDF 라이브러리를 사용하여 고품질의 파일을 작성할 수 있습니다.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://www.adobe.com/kr/creativecloud/business/enterprise/adobe-asset-link.html" target="_blank">Adobe 에셋 링크</a>:</strong>
            Adobe Create Cloud 응용 프로그램에서 직접 AEM Assets에 액세스합니다.</td>
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
            AEM에서 바로 Adobe Stock 이미지를 원활하게 액세스하고 사용할 수 있습니다.</td>
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

***✔<sup>+</sup> 이 버전의 기능이 크게 향상되었습니다.***

***✔<sup></sup> SP는 서비스 팩 또는 기능 팩을 통해 해당 기능을 사용할 수 있음을 나타냅니다.***


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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/managing-assets.html" target="_blank">이미지</a>:</strong>
            스마트 자르기 등 다양한 크기와 포맷으로 이미지를 동적으로 전달할 수 있습니다.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/video-profiles.html" target="_blank">비디오</a>:</strong>
            고급 비디오 인코딩 및 적응형 비디오 스트리밍</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/interactive-images.html" target="_blank">인터랙티브한 미디어</a>:</strong>
            클릭 가능한 컨텐츠가 포함된 인터랙티브한 배너, 비디오를 제작하여 주요 프로모션을 선보일 수 있습니다.
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
            <td><strong>세트(<a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/image-sets.html" target="_blank">이미지</a>,  <a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/spin-sets.html" target="_blank">회전</a>,  <a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/mixed-media-sets.html" target="_blank">혼합 미디어</a></strong>
            ):360도 보기 환경을 확대/축소, 이동, 회전 및 시뮬레이트할 수 있도록 합니다.</td>
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
            다양한 화면/장치에 대한 지원을 통해 브랜딩된 맞춤형 리치 미디어 플레이어 및 사전 설정.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/delivering-dynamic-media-assets.html" target="_blank">전달</a>:</strong>
            HTTP/2 프로토콜을 통해 Dynamic Media 컨텐츠를 연결하거나 임베드하는 유연한 옵션</td>
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
            마스터 자산을 마이그레이션하고 기존 S7 URL을 계속 사용할 수 있습니다.</td>
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

다음은 AEM에서 제공하는 주요 AEM Forms Add-on 기능의 매트릭스입니다. 이러한 기능 중 일부는 각 릴리스에 추가된 이전 버전의 증분 개선 사항에서 도입되었습니다.

+ [AEM Forms 릴리스 노트](https://helpx.adobe.com/experience-manager/6-5/release-notes/forms.html)

***✔<sup>+</sup> 이 버전의 기능이 크게 향상되었습니다.***

***✔<sup></sup> SP는 서비스 팩 또는 기능 팩을 통해 해당 기능을 사용할 수 있음을 나타냅니다.***

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
            디바이스 및 브라우저 설정에 따라 매력적인 반응형 양식을 만들 수 있습니다.</td>
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
            데이터 캡처 환경을 장기 보관하거나 인쇄용 버전을 유지하도록 문서를 만듭니다.</td>
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
            재사용 가능한 테마를 만들어 양식의 구성 요소 및 패널에 스타일을 지정합니다.</td>
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
            적응형 양식의 모범 사례를 표준화하고 구현합니다.</td>
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/cm-overview.html" target="_blank">커뮤니케이션 관리</a>:</strong>
            AEM Forms을 사용하면 개인화되고 인터랙티브한 고객 커뮤니케이션을 제작, 관리 및 전달할 수 있습니다.
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
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#AEMFormsdataintegration" target="_blank">제3자 데이터 통합</a>:</strong>
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
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#FormscentricAEMWorkflowsforAEMFormsonOSGi" target="_blank">Forms 처리를 위한 워크플로우(OSGi</a>):</strong>
            양식 승인 프로세스의 간소화된 배포.</td>
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
            Adobe Analytics 및 Adobe Target과의 통합을 통해 고객 경험을 향상시키고 측정할 수 있습니다.</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/introduction-managing-forms.html" target="_blank">양식 관리자</a>:</strong>
            분석, 번역, A/B 테스트, 검토, 퍼블리싱 등 모든 양식/문서/커뮤니케이션을 관리할 수 있는 단일 위치입니다.
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
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/adaptive-document.html" target="_blank">인터랙티브 커뮤니케이션</a>:</strong>
            차트(이전의 적응형 문서)와 같은 인터랙티브한 요소가 포함된 대상화된 문구와 같은 풍부한 커뮤니케이션을 만듭니다.</td>
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
            직관적인 IDE를 활용하여 복잡한 양식/문서 중심의 워크플로우를 구축할 수 있습니다.</td>
            <td></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#EnhancedDocumentSecurity" target="_blank">AEM Forms Document Security</a>:</strong>
            PDF 및 Office 문서에 대한 안전한 액세스 및 인증
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
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#Simplifiedauthoringexperience" target="_blank">테스트 프레임워크</a>:</strong>
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

다음은 AEM에서 제공하는 주요 AEM Communities Add-on 기능의 매트릭스입니다. 이러한 기능 중 일부는 각 릴리스에 추가된 이전 버전의 증분 개선 사항에서 도입되었습니다.

+ [AEM Communities의 새로운 기능 요약](https://helpx.adobe.com/experience-manager/6-5/communities/using/whats-new-aem-communities.html#main-pars_text)

***✔<sup>+</sup> 이 버전의 기능이 크게 향상되었습니다.***

***✔<sup></sup> SP는 서비스 팩 또는 기능 팩을 통해 해당 기능을 사용할 수 있음을 나타냅니다.***

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
            <td rowspan="7">커뮤니티 기능</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/forum.html" target="_blank">포럼</a>:</strong> (소셜 구성 요소 프레임워크) 새 항목을 만들거나 기존 항목을 보거나, 팔로우, 검색 및 이동합니다.</td>
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
                질문, 보기 및 답변</p>
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
                게시 측에서 블로그 아티클과 주석을 만듭니다.
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
                아이디어를 만들어 커뮤니티와 공유하거나 기존 아이디어를 보거나 팔로우하고 의견을 추가할 수 있습니다.
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
            </strong>사용자 집합은 구성원 그룹에 속할 수 있으며 통칭해서 역할을 할당할 수 있습니다.</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong> </strong></td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/resource.html" target="_blank">할당</a>:</strong>
            커뮤니티 구성원에게 학습 리소스를 만들고 할당합니다.</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td rowspan="5">지원</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/catalog.html" target="_blank">카탈로그 </a> 및  <a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/resource.html" target="_blank">리소스 관리</a>:</strong>
            카탈로그에서 역량 강화 리소스에 액세스합니다.</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/resources.html#CreateaLearningPath" target="_blank">학습 경로 관리</a>:</strong>
            교육 과정 또는 역량 강화 리소스 그룹을 관리합니다.</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/reports.html#main-pars_text_1739724213" target="_blank">활성 보고</a>:</strong>
            지원 리소스 및 학습 경로에 대한 보고.</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/resources.html#main-pars_text_899882038" target="_blank">역량 강화에 대한 참여</a>:</strong>
            지원 리소스에 주석을 추가합니다.</td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/analytics.html" target="_blank">활성 분석</a>: </strong>
            비디오 분석, 진행 보고 및 할당 보고</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td rowspan="8">Commons</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/comments.html" target="_blank">주석 </a> 및 첨부 파일:</strong>
            (Social Component Framework) 커뮤니티 멤버는 커뮤니티 사이트에서 컨텐트에 대한 의견 및 지식을 공유합니다.</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>컨텐츠 조각 변환:</strong>
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
                 (소셜 구성 요소 프레임워크) 커뮤니티 멤버는 주석과 등급 함수의 조합을 사용하여 컨텐츠 일부를 검토합니다.</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/rating.html" target="_blank">등급</a>:/strong&gt;(소셜 구성 요소 프레임워크) 커뮤니티 멤버는 콘텐츠 일부를 평가합니다.</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/voting.html" target="_blank">투표</a>:</strong>
                 (소셜 구성 요소 프레임워크) 커뮤니티 멤버는 컨텐츠를 업로드하거나 다운로드할 수 있습니다.</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/tag-ugc.html" target="_blank">태그</a>:</strong>
            컨텐츠와 태그(키워드 또는 레이블)를 연결하여 컨텐츠를 신속하게 찾을 수 있습니다.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/search.html" target="_blank">검색</a>:</strong>
            예측 및 제안 검색.</td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/translate-ugc.html" target="_blank">번역</a>:</strong>
            사용자 생성 컨텐츠의 기계 번역.</td>
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
            커뮤니티 기능이 있는 사이트 만들기.</td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/sites.html" target="_blank">템플릿</a>:</strong>
                <a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/sites.html" target="_blank"></a> 마법사 기반 <a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/tools-groups.html" target="_blank"></a> 의 전체 기능 커뮤니티 사이트 생성을 위한 사이트 및 그룹 템플릿입니다.</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>편집 가능한 템플릿: </strong>
            커뮤니티 관리자는 AEM 편집 가능 템플릿을 사용하여 풍부한 경험을 구축할 수 있습니다.</td>
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/in-context.html" target="_blank">중재</a>:</strong>
            사용자 생성 컨텐트 중재
            </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/moderation.html" target="_blank">일괄 중재</a>:</strong>
            중재 콘솔을 사용하여 사용자 생성 컨텐츠를 일괄 관리할 수 있습니다.</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/moderate-ugc.html#CommonModerationConcepts" target="_blank">스팸 감지 및 비속어 필터</a>:</strong>
            자동 스팸 감지</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/members.html" target="_blank">멤버 관리</a>:</strong>
            구성원 관리 영역에서 사용자 프로필 및 그룹을 관리합니다.</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/overview.html#main-pars_text_866731966" target="_blank">반응형 디자인</a>:</strong>
            AEM Communities 사이트는 반응형으로 작동하며
            </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/analytics.html" target="_blank">분석</a>:</strong>
            Adobe Analytics과 통합하여 커뮤니티 사이트 사용에 대한 주요 통찰력을 얻을 수 있습니다.</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td rowspan="4">구성원</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/advanced.html" target="_blank">채점 및 배지</a>:</strong>
            (Adobe Sensei에서 제공하는 고급 채점) 커뮤니티 구성원을 전문가로 식별하고 이에 대한 보상으로 만듭니다.</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/activities.html" target="_blank">활동 </a> 및  <a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/notifications.html" target="_blank">알림</a>:</strong>
            최근 활동 스트림을 보고 관심 있는 이벤트에 대한 알림을 받습니다.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/configure-messaging.html" target="_blank">메시지</a>:</strong>
            사용자 및 그룹에 직접 메시징을 제공합니다.</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/social-login.html" target="_blank">소셜 로그인</a>:</strong>
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
            사용자 생성 콘텐츠(UGC)는 로컬 MongoDB 인스턴스에 직접 유지됩니다.</td>
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
                사용자 생성 콘텐츠(UGC)는 Adobe에 호스팅되고 관리되는 클라우드 서비스에서 원격으로 유지됩니다.</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-srp.html" target="_blank"><strong>JSRP</a>:</strong>
                커뮤니티 컨텐츠는 JCR에 저장되고 UGC는 게시된 작성자(또는 게시) 인스턴스에서 액세스할 수 있습니다.</td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/sync.html" target="_blank">사용자 및 그룹 동기화</a>:</strong>
            게시 팜 토폴로지를 사용할 때 게시 인스턴스 간에 사용자와 그룹을 동기화합니다.</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
    </tbody>
</table>

AEM Communities은 다음과 같이 [개선 사항](https://helpx.adobe.com/experience-manager/6-5/communities/using/whats-new-aem-communities.html)을 릴리스에 추가하여 조직에서 사용자를 참여 및 활성화할 수 있도록 합니다.

+ **사용자** 생성 콘텐츠의 @mentionsupport입니다.
+ **지원** 구성 요소의 **키보드 탐색**&#x200B;을 통한 액세서빌러티 개선
+ **사용자 지정 필터**&#x200B;를 사용하여 **벌크 중재**&#x200B;를 개선했습니다.
+ **편집** 가능한 템플릿을 사용하면 커뮤니티 관리자가 AEM에서 풍부한 커뮤니티 경험을 구축할 수 있습니다.
+ 이제 사용자는 그룹의 모든 구성원에게 **쪽지를 일괄**&#x200B;으로 보낼 수 있습니다.
