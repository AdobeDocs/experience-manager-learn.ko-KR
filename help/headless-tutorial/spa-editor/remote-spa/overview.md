---
title: SPA 편집기 및 원격 SPA 시작하기 - 개요
description: AEM SPA Editor를 사용하여 편집 가능한 AEM 컨텐츠으로 기존 Remote SPA 컨텐츠를 보강하려는 개발자를 위한 여러 부분으로 구성된 자습서를 시작합니다.
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
kt: 7630
thumbnail: 333272.jpeg
last-substantial-update: 2022-11-11T00:00:00Z
exl-id: c5f933eb-c409-41dc-bb6a-6b2220dfbb47
source-git-commit: ece15ba61124972bed0667738ccb37575d43de13
workflow-type: tm+mt
source-wordcount: '618'
ht-degree: 5%

---

# 개요

SPA Editor를 사용하여 편집 가능한 AEM 컨텐츠으로 기존 React 기반(또는 Next.js) Remote SPA을 보강하려는 개발자를 위한 다중 부분 자습서를 시작합니다.

이 자습서는 [WKND GraphQL 앱](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/overview.html): AEM GraphQL API를 통해 AEM 컨텐츠 조각 컨텐츠를 소비하는 React 앱이지만 SPA 컨텐츠를 컨텍스트 내에서 작성할 수 없습니다.

>[!VIDEO](https://video.tv.adobe.com/v/333272/?quality=12&learn=on)

## 자습서 정보

AEM 컨텍스트 외부에서 실행 중인 Remote SPA 또는 SPA을 업데이트하여 AEM에서 작성된 컨텐츠를 사용하고 전달하는 방법을 보여 주기 위한 자습서입니다.

자습서의 대부분의 활동은 JavaScript 개발에 중점을 두지만, 중요한 측면은 AEM을 중심으로 진행됩니다. 이러한 측면에는 AEM에서 컨텐츠가 작성 및 저장되는 위치를 정의하고 SPA 경로를 AEM 페이지에 매핑하는 작업이 포함됩니다.

이 튜토리얼은 **AEM as a Cloud Service** 및 는 두 개의 프로젝트로 구성됩니다.

1. 다음 __AEM 프로젝트__ AEM에 배포해야 하는 구성 및 콘텐츠를 포함합니다.
1. __WKND 앱__ 프로젝트는 AEM SPA 편집기와 통합할 SPA입니다.

## 최신 코드

+ 이 자습서 코드의 시작점은 [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/remote-spa) 에서 `remote-spa-tutorial` 폴더를 입력합니다.

## 사전 요구 사항

이 자습서에는 다음 내용이 필요합니다.

+ [AEM as a Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=en)
+ [Node.js v16+](https://nodejs.org/en/)
+ [npm v8+](https://www.npmjs.com/)
+ [Java™ 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
+ [Maven 3.6+](https://maven.apache.org/)
+ [Git](https://git-scm.com/downloads)
+ [aem-guides-wknd.all-2.1.0.zip 이상](https://github.com/adobe/aem-guides-wknd/releases)
+ [aem-guides-wknd-graphql 소스 코드](https://github.com/adobe/aem-guides-wknd-graphql/tree/main)

이 자습서에서는 다음을 가정합니다.

+ [Microsoft® Visual Studio 코드](https://visualstudio.microsoft.com/) IDE로
+ 의 작업 디렉토리 `~/Code/aem-guides-wknd-graphql/remote-spa-tutorial`
+ 에서 작성자 서비스로 AEM SDK 실행 `http://localhost:4502`
+ 로컬에서 AEM SDK 실행 `admin` 암호가 있는 계정 `admin`
+ SPA 실행 `http://localhost:3000`

>[!NOTE]
>
> **로컬 개발 환경 설정에 대한 도움이 필요하십니까?** 다음을 확인하십시오 [AEM as a Cloud Service SDK를 사용하여 로컬 개발 환경을 설정하는 데 대한 다음 안내서](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=ko-KR).

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

마지막으로, 이전 장에서 설명한 개념을 동적 경로에 사용하십시오. 경로의 매개 변수에 따라 서로 다른 컨텐츠를 표시하는 경로. 이는 프로그래밍 방식으로 제어되고 파생된 경로에 AEM SPA 편집기를 사용하여 컨텐츠를 작성할 수 있는 방법을 보여줍니다.

+ [동적 경로 및 편집 가능한 구성 요소에 대해 알아보기](./spa-dynamic-routes.md)

## 추가 리소스

+ [AEM SPA React 편집 가능한 구성 요소](https://www.npmjs.com/package/@adobe/aem-react-editable-components)