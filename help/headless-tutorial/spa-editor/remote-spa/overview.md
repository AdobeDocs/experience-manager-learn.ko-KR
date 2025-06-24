---
title: SPA 편집기 및 원격 SPA 시작하기 - 개요
description: AEM SPA 편집기를 사용하여 편집 가능한 AEM 콘텐츠로 기존 원격 SPA를 확장하려는 개발자를 위한 멀티 파트 튜토리얼을 시작합니다.
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
hide: true
source-git-commit: 5b008419d0463e4eaa1d19c9fe86de94cba5cb9a
workflow-type: ht
source-wordcount: '571'
ht-degree: 100%

---

# 개요

{{edge-delivery-services}}

AEM SPA 편집기를 사용하여 편집 가능한 AEM 콘텐츠로 기존 React 기반(또는 Next.js) 원격 SPA를 확장하려는 개발자를 위한 멀티 파트 튜토리얼을 시작합니다.

이 튜토리얼은 AEM의 GraphQL API에 대한 AEM 콘텐츠 조각 콘텐츠를 사용하는 React 앱인 [WKND GraphQL App](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/overview.html?lang=ko)에서 빌드되지만 상황에 맞는 SPA 콘텐츠 작성 기능은 제공하지 않습니다.

>[!VIDEO](https://video.tv.adobe.com/v/3444854?quality=12&learn=on&captions=kor)

## 튜토리얼 정보

이 튜토리얼은 원격 SPA 또는 AEM 맥락의 밖에서 실행되는 SPA를 업데이트하여 AEM에서 작성된 콘텐츠를 소비 및 게재할 수 있는 방법을 설명하기 위한 목적으로 제작되었습니다.

튜토리얼의 대부분 활동은 JavaScript 개발에 중점을 두지만 AEM과 관련된 핵심적인 측면도 다루고 있습니다. 이러한 측면에는 AEM 내에서 콘텐츠가 작성되고 저장되는 위치를 정의하는 것과 SPA 경로를 AEM 페이지에 매핑하는 것이 포함됩니다.

이 튜토리얼은 **AEM as a Cloud Service** 작업을 위해 설계되었으며, 두 개의 프로젝트로 구성되어 있습니다.

1. __AEM 프로젝트__&#x200B;에는 AEM에 배포해야 하는 구성과 콘텐츠가 포함되어 있습니다.
1. __WKND 앱__ 프로젝트는 AEM의 SPA 편집기와 통합되는 SPA입니다.

## 최신 코드

+ 이 튜토리얼 코드의 출발점은 `remote-spa-tutorial` 폴더의 [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/remote-spa-tutorial)에서 찾을 수 있습니다.

## 사전 요구 사항

이 튜토리얼을 실행하려면 다음 소프트웨어 및 리소스가 필요합니다.

+ [AEM as a Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=ko)
+ [Node.js v18](https://nodejs.org/en/)
+ [Java™ 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
+ [Maven 3.6+](https://maven.apache.org/)
+ [Git](https://git-scm.com/downloads)
+ [aem-guides-wknd.all-2.1.0.zip 이상](https://github.com/adobe/aem-guides-wknd/releases)
+ [aem-guides-wknd-graphql 소스 코드](https://github.com/adobe/aem-guides-wknd-graphql/tree/main)

이 튜토리얼에서는 다음을 가정합니다.

+ IDE로 [Microsoft® Visual Studio Code](https://visualstudio.microsoft.com/)를 사용함
+ 작업 디렉터리는 `~/Code/aem-guides-wknd-graphql/remote-spa-tutorial`임
+ AEM SDK를 작성자 서비스로 실행 중이며 주소는 `http://localhost:4502`임
+ AEM SDK를 로컬 `admin` 계정과 암호 `admin`로 실행 중임
+ SPA는 `http://localhost:3000`에서 실행 중임

>[!NOTE]
>
> **로컬 개발 환경을 설정하는 데 도움이 필요하십니까?** [AEM as a Cloud Service SDK를 사용하여 로컬 개발 환경을 설정하는 방법에 대한 다음 안내서](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=ko)를 확인하십시오.

## &#x200B;1. SPA 편집기용 AEM 구성

SPA를 AEM SPA 편집기와 통합하려면 AEM 구성이 필요합니다. 이러한 구성은 AEM 프로젝트를 통해 관리되고 배포됩니다. 이 장에서는 어떤 구성이 필요한지와 필요한 구성을 정의하는 방법에 대해 알아봅니다.

+ [SPA 편집기용 AEM 구성 방법 알아보기](./aem-configure.md)

## &#x200B;2. SPA 부트스트랩

AEM SPA 편집기가 SPA를 작성 컨텍스트에 통합하려면 SPA에 몇 가지 추가 작업을 수행해야 합니다.

+ [AEM SPA 편집기용 SPA 부트스트랩 방법 알아보기](./spa-bootstrap.md)

## &#x200B;3. 편집 가능한 고정 구성 요소

먼저, SPA에 편집 가능한 “고정 구성 요소”를 추가하는 방법을 살펴보겠습니다. 여기에서는 개발자가 SPA에 특정한 편집 가능 구성 요소를 배치하는 방법을 보여 줍니다. 작성자는 구성 요소의 내용을 변경할 수 있지만 구성 요소를 제거하거나 배치, 위치 또는 크기를 변경할 수는 없습니다.

+ [편집 가능한 고정 구성 요소에 대해 알아보기](./spa-fixed-component.md)

## &#x200B;4. 편집 가능한 컨테이너 구성 요소

다음으로, SPA에 편집 가능한 “컨테이너 구성 요소”를 추가하는 방법을 살펴보겠습니다. 여기에서는 개발자가 SPA에 컨테이너 구성 요소를 배치하는 방법을 보여 줍니다. 컨테이너 구성요소를 사용하면 작성자가 허용된 구성 요소를 컨테이너에 배치하고 구성요소의 레이아웃을 조정할 수 있습니다.

+ [편집 가능한 컨테이너 구성 요소에 대해 알아보기](./spa-container-component.md)

## &#x200B;5. 동적 경로 및 편집 가능한 구성 요소

마지막으로, 이전 장에서 설명한 개념을 동적 경로에 적용해 보겠습니다. 동적 경로는 경로의 매개변수에 따라 다른 콘텐츠를 표시하는 경로입니다. 여기에서는 AEM SPA 편집기를 사용하여 프로그래밍 방식으로 생성되거나 유도된 경로에서 콘텐츠를 작성하는 방법을 보여 줍니다.

+ [동적 경로와 편집 가능한 구성 요소에 대해 알아보기](./spa-dynamic-routes.md)

## 추가 리소스

+ [AEM SPA React 편집 가능한 구성 요소](https://www.npmjs.com/package/@adobe/aem-react-editable-components)
