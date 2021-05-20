---
title: SPA 편집기 및 원격 SPA 시작하기 - 개요
description: AEM SPA Editor를 사용하여 편집 가능한 AEM 컨텐츠으로 기존 Remote SPA 컨텐츠를 보강하려는 개발자를 위한 여러 부분으로 구성된 자습서를 시작합니다.
topic: 헤드리스, SPA, 개발
feature: SPA 편집기, 핵심 구성 요소, API, 개발
role: Developer, Architect
level: Beginner
kt: 7630
thumbnail: 333272.jpeg
source-git-commit: cede0c97e0f322fe5d20d5c4f685ed10b90af1d4
workflow-type: tm+mt
source-wordcount: '694'
ht-degree: 1%

---


# 개요

SPA Editor를 사용하여 편집 가능한 AEM 컨텐츠으로 기존 React 기반(또는 Next.js) Remote SPA을 보강하려는 개발자를 위한 다중 부분 자습서를 시작합니다.

이 자습서는 AEM GraphQL API를 통해 AEM 컨텐츠 조각 컨텐츠를 사용하는 React 앱인 [WKND GraphQL 앱](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/overview.html)을 기반으로 빌드하지만 SPA 컨텐츠의 컨텍스트 내 작성은 제공하지 않습니다.

>[!VIDEO](https://video.tv.adobe.com/v/333272/?quality=12&learn=on)

## 자습서 정보

AEM 컨텍스트 외부에서 실행 중인 Remote SPA 또는 SPA을 업데이트하여 AEM에서 작성된 컨텐츠를 사용하고 전달하는 방법을 보여 주기 위한 자습서입니다.

자습서의 대부분의 활동은 JavaScript 개발에 중점을 두지만, 중요한 측면은 AEM을 중심으로 진행됩니다. 이러한 측면에는 AEM에서 컨텐츠가 작성 및 저장되는 위치를 정의하고 SPA 경로를 AEM 페이지에 매핑하는 작업이 포함됩니다.

이 자습서는 **AEM as a Cloud Service**&#x200B;에서 사용하도록 설계되었으며, 다음 두 개의 프로젝트로 구성됩니다.

1. __AEM Project__&#x200B;에는 AEM에 배포해야 하는 구성 및 컨텐츠가 포함되어 있습니다.
1. __WKND__ Appproject는 AEM SPA 편집기와 통합할 SPA입니다

## 최신 코드

+ 이 자습서의 코드는 `feature/spa-editor` 분기의 [GitHub](https://github.com/adobe/aem-guides-wknd-graphql)에 있습니다.

## 전제 조건

이 자습서에는 다음 내용이 필요합니다.

+ [AEM as a Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=en)
+ [Node.js v14+](https://nodejs.org/en/)
+ [npm v7+](https://www.npmjs.com/)
+ [Java™ 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
+ [Maven 3.6+](https://maven.apache.org/)
+ [Git](https://git-scm.com/downloads)
+ [aem-guides-wknd.all.0.3.0.zip 이상](https://github.com/adobe/aem-guides-wknd/releases)
+ [aem-guides-wknd-graphql 소스 코드](https://github.com/adobe/aem-guides-wknd-graphql)

이 자습서에서는 다음을 가정합니다.

+ [Microsoft® Visual Studio ](https://visualstudio.microsoft.com/) Codeas the IDE
+ `~/Code/wknd-app`의 작업 디렉터리
+ `http://localhost:4502`에서 작성자 서비스로 AEM SDK 실행
+ 암호 `admin`를 사용하여 로컬 `admin` 계정으로 AEM SDK 실행
+ `http://localhost:3000`에서 SPA 실행

>[!NOTE]
>
> **로컬 개발 환경 설정에 대한 도움이 필요하십니까?** AEM as a  [Cloud Service SDK로 사용하여 로컬 개발 환경을 설정하려면 다음 안내서를 확인하십시오](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html).


## 빠른 설정

빠른 설정을 사용하면 WKND App SPA 및 AEM SPA Editor를 15분 내에 사용하여 보고서를 작성하고 실행할 수 있습니다. 이 가속 설정을 사용하면 자습서의 종료 상태로 바로 이동하여 AEM SPA 편집기에서 SPA 작성을 탐색할 수 있습니다.

+ [빠른 설정에 대해 알아보기](./quick-setup.md)

## 1. AEM for SPA 편집기 구성

SPA을 AEM SPA 편집기와 통합하려면 AEM 구성이 필요합니다. 이러한 구성은 AEM Project를 통해 관리 및 배포됩니다. 이 장에서는 필요한 구성과 구성 정의 방법에 대해 알아봅니다.

+ [AEM for SPA Editor 구성 방법 알아보기](./aem-configure.md)

## 2. SPA Bootstrap

AEM SPA Editor를 SPA의 작성 컨텍스트에 통합하려면 SPA에 몇 가지 추가 사항이 있어야 합니다.

+ [SPA for AEM Editor를 부트스트랩하는 방법을 알아봅니다](./spa-bootstrap.md)

## 3. 편집 가능한 고정 구성 요소

먼저 SPA에 편집 가능한 &quot;고정 구성 요소&quot;를 추가하는 방법을 살펴보십시오. 개발자가 특정 편집 가능한 구성 요소를 SPA에 배치하는 방법을 보여줍니다. 작성자는 구성 요소의 컨텐츠를 변경할 수 있지만 구성 요소를 제거하거나 배치, 위치 또는 크기를 변경할 수는 없습니다.

+ [편집 가능한 고정 구성 요소에 대해 알아보기](./spa-fixed-component.md)

## 4. 편집 가능한 컨테이너 구성 요소

다음으로, SPA에 편집 가능한 &quot;컨테이너 구성 요소&quot;를 추가해 보십시오. 개발자가 SPA에서 컨테이너 구성 요소를 배치할 수 있는 방법을 보여줍니다. 컨테이너 구성 요소를 사용하면 작성자가 허용된 구성 요소를 여기에 배치하고 구성 요소의 레이아웃을 조정할 수 있습니다.

+ [편집 가능한 컨테이너 구성 요소에 대해 알아보기](./spa-container-component.md)

## 5. 동적 경로 및 편집 가능한 구성 요소

마지막으로, 이전 장에서 설명한 개념을 동적 경로에 사용하십시오.경로의 매개 변수에 따라 서로 다른 컨텐츠를 표시하는 경로. 이는 프로그래밍 방식으로 제어되고 파생된 경로에 AEM SPA 편집기를 사용하여 컨텐츠를 작성할 수 있는 방법을 보여줍니다.

+ [동적 경로 및 편집 가능한 구성 요소에 대해 알아보기](./spa-dynamic-routes.md)

## 추가 리소스

+ [AEM 문서에서 외부 SPA 편집](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/hybrid/editing-external-spa.html)
+ [AEM WCM 구성 요소 - React Core 구현](https://www.npmjs.com/package/@adobe/aem-core-components-react-base)
+ [AEM WCM 구성 요소 - Spa 편집기 - React Core 구현](https://www.npmjs.com/package/@adobe/aem-core-components-react-spa)
