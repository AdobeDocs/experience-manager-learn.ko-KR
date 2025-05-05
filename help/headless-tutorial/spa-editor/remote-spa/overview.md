---
title: SPA 편집기 및 원격 SPA 시작하기 - 개요
description: AEM SPA Editor를 사용하여 편집 가능한 AEM 콘텐츠로 기존 원격 SPA을 보강하려는 개발자를 위한 멀티 파트 자습서를 시작합니다.
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
jira: KT-7630
thumbnail: 333272.jpeg
last-substantial-update: 2022-11-11T00:00:00Z
doc-type: Tutorial
exl-id: c5f933eb-c409-41dc-bb6a-6b2220dfbb47
duration: 294
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '571'
ht-degree: 2%

---

# 개요

{{edge-delivery-services}}

AEM SPA Editor를 사용하여 편집 가능한 AEM 콘텐츠로 기존 React 기반(또는 Next.js) 원격 SPA을 추가하려는 개발자를 위한 멀티 파트 튜토리얼에 오신 것을 환영합니다.

이 자습서는 AEM의 GraphQL API에 대해 AEM 콘텐츠 조각 콘텐츠를 사용하는 React 앱인 [WKND SPA 앱](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/overview.html?lang=ko)을 기반으로 빌드되지만 GraphQL 콘텐츠의 컨텍스트 내 작성은 제공하지 않습니다.

>[!VIDEO](https://video.tv.adobe.com/v/333272?quality=12&learn=on)

## 튜토리얼 기본 정보

이 자습서는 원격 SPA 또는 AEM 컨텍스트 밖에서 실행되는 SPA을 업데이트하여 AEM에서 작성된 콘텐츠를 소비 및 제공하는 방법을 보여 주기 위한 것입니다.

자습서의 대부분의 활동은 JavaScript 개발에 중점을 두지만 AEM을 중심으로 중요한 측면이 다룹니다. 이러한 측면에서는 콘텐츠가 AEM에서 작성 및 저장되는 위치를 정의하고 SPA 경로를 AEM Pages에 매핑하는 작업이 포함됩니다.

자습서는 **AEM as a Cloud Service**&#x200B;에서 작동하도록 디자인되었으며 두 개의 프로젝트로 구성됩니다.

1. __AEM 프로젝트__&#x200B;에 AEM에 배포해야 하는 구성 및 콘텐츠가 포함되어 있습니다.
1. __WKND 앱__ 프로젝트는 AEM의 SPA 편집기와 통합할 SPA입니다.

## 최신 코드

+ 이 자습서 코드의 시작점은 `remote-spa-tutorial` 폴더의 [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/remote-spa-tutorial)에서 찾을 수 있습니다.

## 사전 요구 사항

이 자습서에서는 다음 작업을 수행해야 합니다.

+ [AEM as a Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=ko)
+ [Node.js v18](https://nodejs.org/en/)
+ [Java™ 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
+ [Maven 3.6+](https://maven.apache.org/)
+ [Git](https://git-scm.com/downloads)
+ [aem-guides-wknd.all-2.1.0.zip 이상](https://github.com/adobe/aem-guides-wknd/releases)
+ [aem-guides-wknd-graphql 소스 코드](https://github.com/adobe/aem-guides-wknd-graphql/tree/main)

이 자습서에서는 다음과 같이 가정합니다.

+ IDE로 [Microsoft® Visual Studio 코드](https://visualstudio.microsoft.com/)
+ `~/Code/aem-guides-wknd-graphql/remote-spa-tutorial`의 작업 디렉터리
+ `http://localhost:4502`에서 작성자 서비스로 AEM SDK 실행
+ 암호가 `admin`인 로컬 `admin` 계정으로 AEM SDK를 실행하는 중
+ `http://localhost:3000`에서 SPA 실행

>[!NOTE]
>
> **로컬 개발 환경을 설정하는 데 도움이 필요하십니까?** AEM as a Cloud Service SDK를 사용하여 로컬 개발 환경을 설정하는 방법에 대한 [다음 안내서를 확인하십시오](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=ko-KR).

## 1. AEM for SPA 편집기 구성

AEM 편집기와 SPA을 통합하려면 AEM SPA 구성이 필요합니다. 이러한 구성은 AEM 프로젝트를 통해 관리 및 배포됩니다. 이 장에서는 필요한 구성과 이를 정의하는 방법에 대해 알아봅니다.

+ [AEM for SPA Editor를 구성하는 방법 알아보기](./aem-configure.md)

## 2. SPA Bootstrap

AEM SPA Editor가 SPA을 작성 컨텍스트에 통합하려면 SPA에 몇 가지 사항을 추가해야 합니다.

+ [SPA for AEM SPA Editor를 부트스트랩하는 방법에 대해 알아봅니다](./spa-bootstrap.md)

## 3. 편집 가능한 고정 구성 요소

먼저 편집 가능한 &quot;고정 구성 요소&quot;를 SPA에 추가하는 방법을 살펴봅니다. 개발자가 SPA에서 특정 편집 가능한 구성 요소를 배치하는 방법을 보여 줍니다. 작성자는 구성 요소의 콘텐츠를 변경할 수 있지만 구성 요소를 제거하거나 배치, 위치 또는 크기를 변경할 수 없습니다.

+ [편집 가능한 고정 구성 요소에 대해 알아보기](./spa-fixed-component.md)

## 4. 편집 가능한 컨테이너 구성 요소

그런 다음 편집 가능한 &quot;컨테이너 구성 요소&quot;를 SPA에 추가해 보십시오. 개발자가 컨테이너 구성 요소를 SPA에 배치하는 방법을 보여 줍니다. 컨테이너 구성 요소를 사용하여 작성자는 허용된 구성 요소를 배치하고 구성 요소의 레이아웃을 조정할 수 있습니다.

+ [편집 가능한 컨테이너 구성 요소에 대해 알아보기](./spa-container-component.md)

## 5. 동적 경로 및 편집 가능한 구성 요소

마지막으로, 이전 장에서 설명한 개념을 사용하여 동적 경로(경로의 매개 변수를 기반으로 다른 콘텐츠를 표시하는 경로)를 살펴보십시오. 이렇게 하면 AEM SPA Editor를 사용하여 프로그래밍 방식으로 구동되고 파생되는 경로의 콘텐츠를 작성하는 방법을 보여 줍니다.

+ [동적 경로 및 편집 가능한 구성 요소에 대해 알아보기](./spa-dynamic-routes.md)

## 추가 리소스

+ [AEM SPA React Editable Components](https://www.npmjs.com/package/@adobe/aem-react-editable-components)
