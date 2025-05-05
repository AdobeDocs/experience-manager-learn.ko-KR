---
title: AEM Headless의 고급 개념 - GraphQL
description: Adobe Experience Manager(AEM) GraphQL API의 고급 개념을 설명하는 종단간 자습서입니다.
version: Experience Manager as a Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Intermediate
exl-id: daae6145-5267-4958-9abe-f6b7f469f803
duration: 441
source-git-commit: bd0f42fa37b7bbe19bf0d7fc65801198e64cbcd9
workflow-type: tm+mt
source-wordcount: '1052'
ht-degree: 0%

---

# AEM Headless의 고급 개념

이 종단간 튜토리얼은 Adobe Experience Manager(AEM) Headless 및 GraphQL의 기본 사항을 다루는 [기본 튜토리얼](../multi-step/overview.md)을 계속합니다. 고급 자습서에서는 클라이언트 애플리케이션에서 GraphQL 지속 쿼리를 사용하는 등 콘텐츠 조각 모델, 콘텐츠 조각 및 AEM GraphQL 지속 쿼리를 사용하여 작업하는 측면을 자세히 설명합니다.

## 사전 요구 사항

AEM as a Cloud Service에 대한 [빠른 설정](../quick-setup/cloud-service.md)을 완료하여 AEM as a Cloud Service 환경을 구성하십시오.

이 고급 자습서를 진행하기 전에 이전 [기본 자습서](../multi-step/overview.md) 및 [비디오 시리즈](../video-series/modeling-basics.md) 자습서를 완료하는 것이 좋습니다. 로컬 AEM 환경을 사용하여 자습서를 완료할 수 있지만, 이 자습서에서는 AEM as a Cloud Service의 워크플로우만 다룹니다.

>[!CAUTION]
>
>AEM as a Cloud Service 환경에 액세스할 수 없는 경우 로컬 SDK을 사용하여 [AEM Headless 빠른 설정](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/quick-setup/local-sdk.html?lang=ko)을 완료할 수 있습니다. 하지만 콘텐츠 조각 탐색과 같은 일부 제품 UI 페이지는 다르다는 점에 유의해야 합니다.



## 목표

이 튜토리얼에서는 다음 주제를 다룹니다.

* 유효성 검사 규칙 과 탭 자리 표시자, 중첩된 조각 참조, JSON 개체 및 날짜 및 시간 데이터 유형과 같은 고급 데이터 유형을 사용하여 콘텐츠 조각 모델을 만듭니다.
* 중첩된 콘텐츠 및 조각 참조로 작업하는 동안 콘텐츠 조각을 작성하고 콘텐츠 조각 작성 거버넌스에 대한 폴더 정책을 구성합니다.
* 변수 및 지시문과 함께 GraphQL 쿼리를 사용하여 AEM GraphQL API 기능을 살펴보십시오.
* AEM의 매개 변수를 사용하여 GraphQL 쿼리를 지속하고 지속된 쿼리와 함께 캐시 제어 매개 변수를 사용하는 방법을 알아봅니다.
* AEM Headless GraphQL SDK을 사용하여 지속 쿼리에 대한 요청을 샘플 WKND JavaScript React 앱에 통합합니다.

## AEM Headless의 고급 개념 개요

다음 비디오에서는 이 자습서에서 다루는 개념에 대한 높은 수준의 개요를 제공합니다. 이 자습서에는 고급 데이터 유형이 포함된 콘텐츠 조각 모델을 정의하고, 콘텐츠 조각을 중첩하며, AEM에서 GraphQL 쿼리를 지속하는 작업이 포함되어 있습니다.

>[!VIDEO](https://video.tv.adobe.com/v/3446134?quality=12&learn=on&captions=kor)

>[!CAUTION]
>
>이 비디오(2시 25분)에는 GraphQL 쿼리를 살펴보기 위해 패키지 관리자를 통해 GraphiQL 쿼리 편집기를 설치하는 방법에 대한 설명이 나와 있습니다. 그러나 최신 버전의 AEM as Cloud Service에서는 기본 제공 **GraphiQL 탐색기**&#x200B;가 제공되므로 패키지를 설치할 필요가 없습니다. 자세한 내용은 [GraphiQL IDE 사용](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/graphiql-ide.html?lang=ko)을 참조하십시오.


## 프로젝트 설정

WKND 사이트 프로젝트에는 필요한 모든 구성이 있으므로 [빠른 설정](../quick-setup/cloud-service.md)을 완료한 후 바로 자습서를 시작할 수 있습니다. 이 섹션에서는 고유한 AEM Headless 프로젝트를 만들 때 사용할 수 있는 몇 가지 중요한 단계만 강조 표시합니다.


### 기존 구성 검토

AEM에서 새 프로젝트를 시작하는 첫 번째 단계는 구성 및 작업 공간을 만들고 GraphQL API 끝점을 만드는 것입니다. 구성을 검토하거나 만들려면 **도구** > **일반** > **구성 브라우저**&#x200B;로 이동하십시오.

![구성 브라우저로 이동](assets/overview/create-configuration.png)

자습서에 대해 `WKND Shared` 사이트 구성이 이미 생성되었는지 확인하십시오. 프로젝트에 대한 구성을 만들려면 오른쪽 상단에서 **만들기**&#x200B;를 선택하고 표시되는 구성 만들기 모달에서 양식을 작성합니다.

![WKND 공유 구성 검토](assets/overview/review-wknd-shared-configuration.png)

### GraphQL API 엔드포인트 검토

다음으로, GraphQL 쿼리를 전송할 API 끝점을 구성해야 합니다. 기존 끝점을 검토하거나 새로 만들려면 **도구** > **일반** > **GraphQL**(으)로 이동합니다.

![끝점 구성](assets/overview/endpoints.png)

`WKND Shared Endpoint`이(가) 이미 만들어졌는지 확인하십시오. 프로젝트의 끝점을 만들려면 오른쪽 상단에서 **만들기**&#x200B;를 선택하고 워크플로를 따릅니다.

![WKND 공유 끝점 검토](assets/overview/review-wknd-shared-endpoint.png)

>[!NOTE]
>
> 끝점을 저장한 후에는 보안 콘솔 방문에 대한 모달이 표시되며, 이를 통해 끝점에 대한 액세스를 구성하려는 경우 보안 설정을 조정할 수 있습니다. 그러나 보안 권한 자체는 이 자습서에서 다루지 않습니다. 자세한 내용은 [AEM 설명서](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security.html?lang=ko)를 참조하세요.

### WKND 콘텐츠 구조 및 언어 루트 폴더 검토

잘 정의된 콘텐츠 구조는 AEM Headless 구현의 성공을 위한 핵심 요소입니다. 콘텐츠의 확장성, 유용성 및 권한 관리에 유용합니다.

언어 루트 폴더는 EN 또는 FR과 같은 ISO 언어 코드가 있는 폴더입니다. AEM 번역 관리 시스템은 이러한 폴더를 사용하여 콘텐츠의 기본 언어와 콘텐츠 번역을 위한 언어를 정의합니다.

**탐색** > **Assets** > **파일**(으)로 이동합니다.

![파일로 이동](assets/overview/files.png)

**WKND 공유** 폴더로 이동합니다. 제목이 &quot;English&quot;이고 이름이 &quot;EN&quot;인 폴더를 관찰합니다. 이 폴더는 WKND 사이트 프로젝트의 언어 루트 폴더입니다.

![영어 폴더](assets/overview/english.png)

프로젝트에 대해 구성 내에 언어 루트 폴더를 만듭니다. 자세한 내용은 [폴더 만들기](/help/headless-tutorial/graphql/advanced-graphql/author-content-fragments.md#create-folders) 섹션을 참조하십시오.

### 중첩된 폴더에 구성 할당

마지막으로, 프로젝트의 구성을 언어 루트 폴더에 할당해야 합니다. 이 할당을 통해 프로젝트 구성에 정의된 콘텐츠 조각 모델을 기반으로 콘텐츠 조각을 만들 수 있습니다.

언어 루트 폴더를 구성에 할당하려면 폴더를 선택한 다음 상단 탐색 막대에서 **속성**&#x200B;을 선택합니다.

![속성 선택](assets/overview/properties.png)

그런 다음 **클라우드 서비스** 탭으로 이동하여 **클라우드 구성** 필드에서 폴더 아이콘을 선택합니다.

![클라우드 구성](assets/overview/cloud-conf.png)

표시되는 모달에서 이전에 만든 구성을 선택하여 언어 루트 폴더를 할당합니다.

### 모범 사례

다음은 AEM에서 나만의 프로젝트를 만들 때의 모범 사례입니다.

* 폴더 계층 구조는 현지화 및 번역을 염두에 두고 모델링해야 합니다. 즉, 언어 폴더를 구성 폴더 내에 중첩하여 해당 구성 폴더 내의 콘텐츠를 쉽게 번역할 수 있습니다.
* 폴더 계층 구조는 단순하고 간단하게 유지되어야 합니다. 특히 라이브 사용을 위해 게시한 후에는 조각 참조 및 GraphQL 쿼리에 영향을 줄 수 있는 경로를 변경하므로 나중에 폴더 및 조각을 이동하거나 이름을 변경하지 마십시오.

## 스타터 및 솔루션 패키지

두 개의 AEM **패키지**&#x200B;를 사용할 수 있으며 [패키지 관리자](/help/headless-tutorial/graphql/advanced-graphql/author-content-fragments.md#sample-content)를 통해 설치할 수 있습니다.

* [Advanced-GraphQL-Tutorial-Starter-Package-1.1.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/Advanced-GraphQL-Tutorial-Starter-Package-1.1.zip)은(는) 자습서에서 나중에 사용되며 샘플 이미지와 폴더를 포함합니다.
* [Advanced-GraphQL-Tutorial-Solution-Package-1.2.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/Advanced-GraphQL-Tutorial-Solution-Package-1.2.zip)에는 새 콘텐츠 조각 모델, 콘텐츠 조각 및 지속 GraphQL 쿼리를 포함하여 챕터 1-4에 대한 완성된 솔루션이 포함되어 있습니다. [클라이언트 응용 프로그램 통합](/help/headless-tutorial/graphql/advanced-graphql/client-application-integration.md) 장으로 바로 건너뛰려는 사용자에게 유용합니다.


[React 앱 - 고급 자습서 - WKND 모험](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/advanced-tutorial/README.md) 프로젝트를 사용하여 샘플 응용 프로그램을 검토하고 탐색할 수 있습니다. 이 샘플 애플리케이션은 지속 GraphQL 쿼리를 호출하여 AEM에서 콘텐츠를 검색하고 몰입형 경험에서 렌더링합니다.

## 시작하기

이 고급 자습서를 시작하려면 다음 단계를 따르십시오.

1. [AEM as a Cloud Service](../quick-setup/cloud-service.md)을(를) 사용하여 개발 환경을 설정합니다.
1. [콘텐츠 조각 모델 만들기](/help/headless-tutorial/graphql/advanced-graphql/create-content-fragment-models.md)에서 자습서 챕터를 시작합니다.
