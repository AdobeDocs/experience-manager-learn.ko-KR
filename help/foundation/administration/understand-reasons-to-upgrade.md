---
title: 업그레이드해야 하는 이유 이해하기
description: 최신 버전의 Adobe Experience Manager 6으로 업그레이드하는 것을 고려하는 고객을 위한 주요 기능에 대한 높은 수준의 분류입니다.
version: 6.5
topic: Upgrade
feature: Release Information
role: Leader, Architect, Developer, Admin, User
level: Beginner
doc-type: Article
exl-id: bf4030b0-67c4-4b00-af95-f63e6f79e995
duration: 820
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '2586'
ht-degree: 1%

---

# 업그레이드 이유 이해하기

최신 버전의 Adobe Experience Manager 6으로 업그레이드하는 것을 고려하는 고객을 위한 주요 기능에 대한 높은 수준의 분류입니다.

## AEM 6.5로 업그레이드하기 위한 주요 기능

+ [Adobe Experience Manager 6.5 릴리스 노트](https://helpx.adobe.com/kr/experience-manager/6-5/release-notes.html)

### 기초 개선 사항

Adobe Experience Manager 6.5는 다음을 통해 시스템의 안정성, 성능 및 지원 능력을 지속적으로 향상시킵니다.

+ **Java 11** 지원(Java 8 지원은 유지 관리).

### 웹 사이트 생성 및 관리

AEM Sites은 웹 사이트 생성 및 구축을 가속화하기 위해 고안된 다양한 기능을 도입했습니다.

+ **SPA 편집기** 지원을 통해 SPA(단일 페이지 애플리케이션)를 AEM에서 완전히 작성할 수 있으므로 마케터에게 친숙한 풍부한 작성 경험을 지원합니다.
+_ **JavaScript SDK**, SPA 프로젝트 시작 키트 및 지원 빌드 도구를 사용하면 프론트엔드 개발자가 AEM과 독립적으로 SPA 편집기 호환 단일 페이지 애플리케이션을 개발할 수 있습니다.
+ **핵심 구성 요소** 는 다수의 새 구성 요소를 추가합니다. **구성 요소 라이브러리** 기존 핵심 구성 요소에 대한 다양한 개선 사항이 있습니다.
+ 추가 **번역** 향상된 기능은 AEM Sites 번역을 간소화합니다.

### 유연한 환경

AEM은 AEM 외부의 콘텐츠를 쉽게 사용할 수 있도록 새롭고 개선된 도구로 Fluid Experiences를 계속 수용합니다.

+ **컨텐츠 조각** 버전 비교/비교 및 주석 지원.
+ **AEM Assets HTTP API** 노출을 지원합니다. **컨텐츠 조각** DAM에서 **JSON**.
  **경험 조각** 지원 **전체 텍스트 검색** 및 **AEM 디스패처 캐시 무효화** 참조용 **페이지**.

### 자산 관리

AEM Assets은 DAM의 사용, 관리 및 이해를 개선하기 위해 풍부한 에셋 관리 기능을 기반으로 하고 있습니다. AEM 6.5는 Adobe Creative Cloud과 크리에이티브 워크플로 간의 통합을 지속적으로 개선합니다.

+ **Adobe 에셋 링크** Adobe Creative Cloud 도구에서 크리에이티브를 AEM Assets에 직접 연결합니다.
+ **Adobe Stock** 통합을 통해 AEM Assets 환경에서 Adobe Stock 이미지에 직접 액세스할 수 있으므로 원활한 콘텐츠 검색 경험을 만들 수 있습니다.
+ **AEM Desktop App** 는 버전 2.0을 릴리스하고 자체 리비전하면서 성능과 안정성을 개선합니다.
+ **연결된 자산** 는 다른 AEM Sites 인스턴스의 자산에 원활하게 액세스하고 사용할 수 있도록 개별 AEM Assets 인스턴스를 지원합니다.
+ 의 비디오 지원이 업데이트되었습니다. **Dynamic Media**, 포함 **360 비디오** 및 **사용자 지정 비디오 썸네일**.

### 콘텐츠 인텔리전스

AEM은 머신 러닝 및 인공 지능을 활용하여 모든 경험을 개선하면서 스마트 기술과의 통합을 지속적으로 구축하고 있습니다.

+ **Adobe 에셋 링크** 추가 **시각적 유사성 검색**&#x200B;를 사용하여 유사한 이미지를 내에서 쉽게 검색하고 사용할 수 있습니다 **Adobe Creative Cloud 도구**.

### 통합

AEM은 다른 Adobe 서비스와 통합할 수 있는 기능을 확장합니다.

+ **경험 조각** 과의 통합을 심화합니다. **Adobe Target** 를 지원하여 **JSON으로 내보내기** Adobe Target 및 기능 **경험 조각 기반 오퍼 삭제** 출처: **Adobe Target**.

### AMS Cloud Manager

[Cloud Manager](https://adobe.ly/2HODmsv)Adobe Managed Services(AMS) 고객에게만 제공되는 은 다음 기능을 제공합니다.

+ Cloud Manager는 AEM 배포 지원을 AEM Sites에서 로 확장합니다. **AEM Assets**, 포함 **자산 처리의 자동화된 성능 테스트**.
+ **자동 확장** 사전 정의된 임계값의 AEM 게시 계층 중에서 최적의 최종 사용자 경험을 보장하십시오.
+ **비프로덕션 파이프라인** 개발팀이 Cloud Manager를 활용하여 코드 품질을 지속적으로 확인하고 낮은 환경(개발 및 QA)에 배포할 수 있도록 허용합니다.
+ **CI/CD 파이프라인 API** 고객이 프로그래밍 방식으로 Cloud Manager에 참여하도록 허용하여 온프레미스 개발 인프라와의 통합 가능성을 높입니다.

## 기초 기능

다음은 AEM에서 제공하는 주요 기초 기능의 매트릭스입니다. 이러한 기능 중 일부는 각 릴리스에 추가된 이전 버전의 증분 개선 사항에 도입되었습니다.

+ [AEM Foundation 릴리스 노트](https://helpx.adobe.com/experience-manager/6-5/release-notes/wcm-platform.html)

***✔<sup>+</sup> 이 버전의 기능에 대한 중요한 개선 사항입니다.***

***✔<sup>SP</sup> 는 서비스 팩 또는 기능 팩을 통해 사용할 수 있는 기능을 나타냅니다.***

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
                <strong>Java 11 지원:</strong> AEM은 Java 11(및 Java 8)을 지원합니다.
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
                <strong><a href="https://jackrabbit.apache.org/oak/docs/index.html" target="_blank">Oak 콘텐츠 저장소</a>:</strong> 이전 Jackrabbit 2보다 훨씬 뛰어난 성능과 확장성을 제공합니다.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/indexing-via-the-oak-run-jar.html">oak-run.jar 색인 지원</a>:</strong> Oak 인덱스의 리인덱싱, 통계 수집 및 일관성 검사 기능이 개선되었습니다.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/queries-and-indexing.html" target="_blank">사용자 정의 검색 인덱스</a>: </strong>
                쿼리 성능 및 검색 관련성을 최적화하기 위해 사용자 정의 인덱스 정의를 추가하는 기능.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/revision-cleanup.html" target="_blank">온라인 개정 정리</a>:</strong>
                서버 다운타임 없이 저장소 유지 관리를 수행할 수 있습니다.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/storage-elements-in-aem-6.html" target="_blank">TarMK 또는 MongoMK 저장소 저장소</a>:</strong>
                <br> TarMK(차세대 TarPM 버전)의 간단하고 성능이 뛰어난 파일 기반 스토리지 사용 옵션
                <br> 또는 MongoMK를 통해 MongoDB 지원 저장소를 사용하여 수평으로 확장할 수 있습니다.</td>
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
            MongoMK는 AEM 6.0으로 도입된 이후 계속 개선되었습니다.</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/data-store-config.html#AmazonS3DataStore">Amazon 데이터 저장소</a>:</strong>
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
            <td><strong>Touch UI 기능 패리티:</strong>
                클래식 UI를 통해 향상된 생산성 및 기능 패리티를 통해 속도를 높일 수 있도록 UI 작성에 대한 지속적인 개선 사항.</td>
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
 AEM 내에서 유지 관리를 수행하고, 서버 상태를 모니터링하고, 성능을 분석합니다.</td>
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
            업그레이드 개선을 통해 AEM을 더 쉽고 빠르게 즉각적으로 업그레이드할 수 있습니다.</td>
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
            프레젠테이션과 논리를 구분하는 최신 템플릿 엔진입니다. 구성 요소 개발 시간이 크게 단축됩니다. 각 릴리스에 증분 기능이 추가되었습니다.</td>
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
            JCR 리소스를 비즈니스 오브젝트 및 논리로 모델링하기 위한 유연한 프레임워크. 각 릴리스에 증분 기능이 추가되었습니다.
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
                Adobe Managed Services(AMS) 고객에게만 독점적인 Cloud Manager는 최신 CI/CD 파이프라인을 통해 개발 및 배포를 가속화합니다.</td>
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

다음은 AEM에서 제공하는 주요 보안 기능의 매트릭스입니다. 이러한 기능 중 일부는 각 릴리스에 추가된 이전 버전의 증분 개선 사항에 도입되었습니다.

+ [보안 릴리스 정보](https://helpx.adobe.com/experience-manager/6-5/release-notes/wcm-platform.html#Security)

***✔은 이 버전의 기능에 대한 중요한 개선 사항이 있음을 나타냅니다.***

***✔<sup>+</sup> 는 서비스 팩 또는 기능 팩을 통해 사용할 수 있는 기능을 나타냅니다.***

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
            <br> 권한을 구분하므로 불필요한 관리자 권한 사용을 방지할 수 있습니다.</td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/saml-2-0-authenticationhandler.html" target="_blank">키 저장소 관리</a></strong>
            <br> 저장소 내에서 관리되는 글로벌 신뢰 저장소, 인증서 및 키입니다.</td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><a href="https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/csrf-protection.html" target="_blank"><strong>CSRF</strong> <strong>보호</strong></a>
            <br> 즉시 사용 가능한 크로스 사이트 요청 위조 보호.</td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/saml-2-0-authenticationhandler.html" target="_blank"><strong>CORS</strong> <strong>지원</strong></a>
            <br> 원본 간 리소스 공유 지원을 통해 응용 프로그램의 유연성을 높일 수 있습니다.</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://experienceleague.adobe.com/docs/" target="_blank">향상된 SAML 인증 지원</a><br>
 </strong>향상된 SAML 리디렉션, 최적화된 그룹 정보 및 키 암호화 문제가 해결되었습니다.
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
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ldap-config.html" target="_blank">OSGi 구성으로서의 LDAP</a><br>
 </strong>LDAP 인증 관리 및 업데이트 간소화.</td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong>일반 텍스트 암호에 대한 OSGi 암호화 지원<br>
 </strong>암호 및 기타 중요한 값은 암호화된 형식으로 저장하고 자동으로 해독할 수 있습니다.</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/user-group-ac-admin.html" target="_blank">CUG 개선 사항</a><br>
 </strong>성능 및 확장성 문제를 해결하기 위해 폐쇄형 사용자 그룹 구현이 다시 작성되었습니다.</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔<sup>+</sup></td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/platform-repository/using/ssl-wizard-technical-video-use.html" target="_blank">SSL 마법사</a></strong>
            <br> SSL 설정 및 관리를 간소화하는 UI.</td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/encapsulated-token.html" target="_blank">압축된 토큰 지원</a></strong>
            <br> 게시 인스턴스 간 수평 인증을 지원하기 위해 "고정" 세션이 더 이상 필요하지 않습니다.</td>
        <td> </td>
        <td> </td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ims-config-and-admin-console.html" target="_blank">Adobe IMS 인증 지원</a><br>
 </strong>AMS(Managed Services) Adobe 전용이며 Adobe IMS(Identity Management System)를 통해 AEM 작성자 인스턴스에 대한 액세스를 중앙에서 관리합니다.</td>
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

## Sites 기능

다음은 AEM에서 제공하는 주요 Sites 기능의 매트릭스입니다. 이러한 기능 중 일부는 각 릴리스에 추가된 이전 버전의 증분 개선 사항에 도입되었습니다.

+ [AEM Sites 릴리스 노트](https://helpx.adobe.com/experience-manager/6-5/release-notes/sites.html)

***✔<sup>+</sup> 이 버전의 기능에 대한 중요한 개선 사항입니다.***

***✔<sup>SP</sup> 는 서비스 팩 또는 기능 팩을 통해 사용할 수 있는 기능을 나타냅니다.***

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
            편집자는 터치 스크린이 있는 태블릿과 컴퓨터를 활용할 수 있습니다.</td>
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
                레이아웃 모드를 사용하면 편집기에서 응답형 사이트의 장치 너비에 따라 구성 요소의 크기를 조정할 수 있습니다.</td>
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
            사이트 개발을 가속화합니다. 빈번한 릴리스 일정과 유연성을 위해 GitHub에서 사용할 수 있습니다.</td>
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
            React를 기반으로 구축된 단일 페이지 애플리케이션(SPA) 프레임워크를 사용하여 작성 가능한 웹 경험을 만듭니다.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong>스타일 시스템:</strong>
            컨텍스트 내 스타일 시스템을 사용하여 시각적 모양을 정의하여 AEM 구성 요소 재사용을 늘립니다.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>SP</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/msm.html" target="_blank">다중 사이트 관리자(MSM)</a>:</strong>
            공통 콘텐츠를 공유하는 여러 웹 사이트(즉, 다국어, 여러 브랜드)를 관리합니다.</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/translation.html" target="_blank">콘텐츠 번역</a>:</strong>
            플러그 앤 플레이 프레임워크는 업계 최고의 타사 번역 서비스와 통합됩니다.</td>
            <td></td>
            <td></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/contexthub.html" target="_blank">컨텍스트 허브</a>:</strong>
            콘텐츠의 개인화를 위한 차세대 Client Context Framework.</td>
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
            일상적인 작성을 중단하지 않고 향후 릴리스를 위한 콘텐츠를 개발할 수 있습니다.</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>컨텐츠 조각:</strong>
            프레젠테이션의 결합이 해제된 에디토리얼 컨텐츠를 만들고 선별하여 쉽게 재사용할 수 있습니다.</td>
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
            데스크탑, 모바일 및 소셜 채널에 최적화된 재사용 가능한 경험과 변형을 만들 수 있습니다.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong>컨텐츠 서비스:</strong>
            장치 및 애플리케이션 전반에서 사용할 AEM의 콘텐츠를 JSON으로 내보냅니다.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>SP</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Adobe Analytics 통합 및 컨텐츠 인사이트:</strong>
                Adobe Analytics과 DTM의 간편한 통합. 작성자 환경 내에 성능 정보를 표시합니다.</td>
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
            단계별 마법사: 타겟팅된 경험을 만들고 재사용 가능한 오퍼 라이브러리를 만듭니다.</td>
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
            차세대 이메일 캠페인 솔루션과 쉽게 통합됩니다.</td>
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
            Adobe의 차세대 태그 관리 클라우드 서비스와 통합됩니다.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>화면:</strong>
            디지털 사이니지 및 키오스크에 대한 경험을 관리합니다.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ecommerce.html" target="_blank">전자 상거래</a>:</strong>
            웹, 모바일 및 소셜 터치포인트 전반에 걸쳐 브랜딩되고 개인화된 쇼핑 경험을 제공합니다.
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
            포럼, 스레드된 주석, 이벤트 캘린더 및 기타 다양한 기능을 통해 사이트 방문자와 긴밀하게 소통할 수 있습니다.</td>
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

## Assets 기능

다음은 AEM에서 제공하는 주요 Assets 기능의 매트릭스입니다. 이러한 기능 중 일부는 각 릴리스에 추가된 이전 버전의 증분 개선 사항에 도입되었습니다.

+ [AEM Assets 릴리스 노트](https://helpx.adobe.com/experience-manager/6-5/release-notes/assets.html)

***✔은 이 버전의 기능에 대한 중요한 개선 사항이 있음을 나타냅니다.***

***✔<sup>+</sup> 는 서비스 팩 또는 기능 팩을 통해 사용할 수 있는 기능을 나타냅니다.***

<table>
    <thead>
        <tr>
            <td>에셋 기능</td>
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
            데스크탑 컴퓨터나 터치 지원 장치에서 자산을 관리합니다.</td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/metadata.html" target="_blank">고급 메타데이터 관리</a>:</strong>
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/task-content.html" target="_blank">작업</a> 및 <a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/projects-with-workflows.html" target="_blank">워크플로</a> 관리:</strong>
            AEM Projects를 활용하는 디지털 에셋의 검토 및 승인을 위해 사전 빌드된 워크플로우 및 작업.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>확장성 및 성능:</strong>
            규모에 맞게 수집, 업로드 및 저장에 대한 지원이 향상되었습니다.</td>
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
            HTTP 및 JSON을 통해 자산과 프로그래밍 방식으로 상호 작용합니다.</td>
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
            로그인할 필요 없이 디지털 에셋을 간편하게 임시로 공유</td>
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
            디지털 에셋의 원활한 공유 및 배포를 위한 클라우드 서비스 SAAS 솔루션입니다.</td>
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/touch-ui-asset-insights.html" target="_blank">자산 통찰력</a>:</strong>
            Adobe Analytics을 활용하여 디지털 에셋의 고객 상호 작용을 캡처하고 AEM에서 볼 수 있습니다.</td>
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
            언어 루트를 사용하여 자산 메타데이터의 번역을 자동으로 지원합니다.</td>
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
            제품 카탈로그를 생성합니다. InDesign 템플릿을 기반으로 브로셔, 전단지 및 인쇄 광고를 제작할 수 있습니다.</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/kr/experience-manager/desktop-app/aem-desktop-app.html" target="_blank">AEM Desktop App</a>:</strong>
            Creative Suite 제품을 사용하여 편집할 자산을 로컬 데스크톱에 동기화합니다.
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
                <br> 고품질 파일 조작에 사용되는 Photoshop 및 Acrobat PDF 라이브러리.</td>
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
            클라우드 애플리케이션 만들기 Adobe에서 직접 AEM Assets에 액세스합니다.</td>
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

***✔<sup>SP</sup> 는 서비스 팩 또는 기능 팩을 통해 사용할 수 있는 기능을 나타냅니다.***


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
            스마트 자르기를 포함하여 다양한 크기와 형식으로 이미지를 동적으로 제공합니다.</td>
            <td> </td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/video-profiles.html" target="_blank">비디오</a>:</strong>
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
            주요 오퍼를 선보일 수 있는 클릭 가능한 콘텐츠가 있는 대화형 배너, 비디오를 만듭니다.
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
            <td><strong>설정 (<a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/image-sets.html" target="_blank">이미지</a>, <a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/spin-sets.html" target="_blank">회전</a>, <a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/mixed-media-sets.html" target="_blank">혼합 미디어</a>):</strong>
            사용자가 360도 보기 환경을 확대/축소, 패닝, 회전 및 시뮬레이션할 수 있습니다.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://experienceleague.adobe.com/docs/" target="_blank">뷰어</a>:</strong>
            다양한 화면/장치를 지원하는 사용자 지정 브랜드 리치 미디어 플레이어 및 사전 설정입니다.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/delivering-dynamic-media-assets.html" target="_blank">게재</a>:</strong>
            HTTP/2 프로토콜을 통한 Dynamic Media 콘텐츠 연결 또는 임베드 및 전달을 위한 유연한 옵션.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Scene7에서 Dynamic Media으로 업그레이드:</strong>
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

다음은 AEM에서 제공하는 주요 AEM Forms 추가 기능 매트릭스 입니다. 이러한 기능 중 일부는 각 릴리스에 추가된 이전 버전의 증분 개선 사항에 도입되었습니다.

***✔<sup>+</sup> 이 버전의 기능에 대한 중요한 개선 사항입니다.***

***✔<sup>SP</sup> 는 서비스 팩 또는 기능 팩을 통해 사용할 수 있는 기능을 나타냅니다.***

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
            장치 및 브라우저 설정을 기반으로 매력적인 반응형 적응형 양식을 만듭니다.</td>
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
            문서를 만들어 데이터 캡처 환경을 장기간 저장하거나 인쇄 준비 버전을 저장할 수 있습니다.</td>
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
            재사용 가능한 테마를 만들어 양식의 구성 요소 및 패널의 스타일을 지정합니다.</td>
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
            적응형 양식에 대한 모범 사례를 표준화하고 구현합니다.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#EnhancedintegrationwithAdobeSign" target="_blank">Acrobat Sign 통합</a>:</strong>
            서명 시나리오를 기반으로 Acrobat Sign 통합 양식을 배포할 수 있습니다.</td>
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
            AEM Forms을 사용하면 개인화되고 인터랙티브한 고객 서신을 생성, 관리 및 제공할 수 있습니다.
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
            데이터 통합을 사용하면 양식의 사용자 입력을 기반으로 다양한 데이터 소스에서 데이터를 가져옵니다. 양식 제출 시 캡처된 데이터가 데이터 소스에 다시 기록됩니다.
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
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#FormscentricAEMWorkflowsforAEMFormsonOSGi" target="_blank">Forms 처리를 위한 (OSGi에서) 워크플로우</a>:</strong>
            양식 승인 프로세스의 배포를 단순화합니다.</td>
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
            Adobe Analytics 및 Adobe Target과 통합하여 고객 경험을 개선하고 측정합니다.</td>
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
            분석, 번역, A/B 테스트, 검토 및 게시와 같은 모든 양식/문서/서신을 관리할 수 있는 단일 위치입니다.
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
            iOS, Android 또는 Windows에서 앱 내에서 온라인/오프라인 양식을 처리할 수 있습니다.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/adaptive-document.html" target="_blank">대화형 통신</a>:</strong>
            차트(이전의 적응형 문서)와 같은 대화형 요소를 사용하여 타깃팅된 문과 같은 풍부한 커뮤니케이션을 생성합니다.</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong>Forms 처리를 위한 워크플로우(J2EE):</strong>
            직관적인 IDE를 사용하여 복잡한 양식/문서 중심 워크플로를 빌드합니다.</td>
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
            PDF 및 Office 문서의 보안 액세스 및 권한 부여.
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
            Calvin 프레임워크 및 Chrome 플러그인을 사용하여 적응형 양식을 지원하고 디버깅하십시오.</td>
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

