---
title: AEM Headless의 고급 개념 - GraphQL
description: Adobe Experience Manager(AEM) GraphQL API의 고급 개념을 설명하는 전체 튜토리얼입니다.
version: Experience Manager as a Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Intermediate
exl-id: daae6145-5267-4958-9abe-f6b7f469f803
duration: 441
source-git-commit: bd0f42fa37b7bbe19bf0d7fc65801198e64cbcd9
workflow-type: ht
source-wordcount: '1052'
ht-degree: 100%

---

# AEM Headless의 고급 개념

이 전체 튜토리얼은 Adobe Experience Manager(AEM) Headless와 GraphQL의 기본을 다룬 [기본 튜토리얼](../multi-step/overview.md)의 후속 튜토리얼입니다. 고급 튜토리얼에서는 클라이언트 애플리케이션에서 GraphQL 지속 쿼리를 사용하는 방법을 포함하여, 콘텐츠 조각 모델, 콘텐츠 조각 및 AEM GraphQL 지속 쿼리를 사용하는 방법에 대한 심층적인 내용을 설명합니다.

## 사전 요구 사항

[AEM as a Cloud Service용 빠른 설정](../quick-setup/cloud-service.md)을 완료하여 AEM as a Cloud Service 환경을 구성합니다.

이 고급 튜토리얼을 진행하기 전에 이전의 [기본 튜토리얼](../multi-step/overview.md)과 [비디오 시리즈](../video-series/modeling-basics.md) 튜토리얼을 완료하는 것이 좋습니다. 로컬 AEM 환경을 사용하여 튜토리얼을 완료할 수도 있지만 이 튜토리얼에서는 AEM as a Cloud Service용 워크플로만 다룹니다.

>[!CAUTION]
>
>AEM as a Cloud Service 환경에 액세스할 수 없는 경우 [로컬 SDK를 사용하여 AEM Headless 빠른 설정](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/quick-setup/local-sdk.html?lang=ko)을 완료할 수 있습니다. 단, 콘텐츠 조각 탐색과 같은 일부 제품 UI 페이지는 다르다는 점을 염두에 두어야 합니다.



## 목표

이 튜토리얼에서는 다음 주제를 다룹니다.

* 탭 플레이스홀더, 중첩된 조각 참조, JSON 오브젝트, 날짜 및 시간 데이터 유형과 같은 고급 데이터 유형과 검증 규칙을 사용하여 콘텐츠 조각 모델을 만듭니다.
* 중첩된 콘텐츠와 조각 참조를 사용하여 작업하는 동안 콘텐츠 조각을 작성하고, 콘텐츠 조각 작성 거버넌스를 위한 폴더 정책을 구성합니다.
* GraphQL 쿼리에서 변수와 지시문을 사용하여 AEM GraphQL API 기능을 살펴봅니다.
* 매개변수가 있는 GraphQL 쿼리를 AEM에 지속하고, 지속 쿼리에서 캐시 제어 매개변수를 사용하는 방법을 알아봅니다.
* AEM Headless JavaScript SDK를 사용하여 지속 쿼리 요청을 샘플 WKND GraphQL React 앱에 통합합니다.

## AEM Headless의 고급 개념 개요

다음 비디오에서는 이 튜토리얼에서 다루는 개념에 대한 전반적인 개요를 제공합니다. 이 튜토리얼에는 고급 데이터 유형을 사용하여 콘텐츠 조각 모델을 정의하고, 콘텐츠 조각을 중첩하고, AEM에서 GraphQL 쿼리를 지속하는 방법이 포함되어 있습니다.

>[!VIDEO](https://video.tv.adobe.com/v/3446134?quality=12&learn=on&captions=kor)

>[!CAUTION]
>
>이 비디오(2:25)에서는 패키지 관리자를 통해 GraphiQL 쿼리 편집기를 설치하여 GraphQL 쿼리를 살펴보는 방법에 대해 설명합니다. 그러나 최신 버전의 AEM as a Cloud Service에서는 **GraphiQL 탐색기**&#x200B;가 기본 제공되므로 별도의 패키지 설치가 필요하지 않습니다. 자세한 내용은 [GraphiQL IDE 사용](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/graphiql-ide.html?lang=ko)을 참조하십시오.


## 프로젝트 설정

WKND 사이트 프로젝트에는 필요한 모든 구성이 포함되어 있으므로 [빠른 설정](../quick-setup/cloud-service.md)을 완료한 후 튜토리얼을 바로 시작할 수 있습니다. 이 섹션에서는 AEM Headless 프로젝트를 만들 때 사용할 수 있는 몇 가지 중요한 단계만 강조해서 설명합니다.


### 기존 구성 검토

AEM에서 새 프로젝트를 시작하기 위한 첫 번째 단계는 작업 영역으로 구성을 만들고 GraphQL API 엔드포인트를 만드는 것입니다. 구성을 검토하거나 만들려면 **도구** > **일반** > **구성 브라우저**&#x200B;로 이동합니다.

![구성 브라우저로 이동](assets/overview/create-configuration.png)

`WKND Shared` 사이트 구성에 튜토리얼을 위한 사이트 구성이 이미 생성되어 있음을 확인합니다. 고유한 프로젝트에 대한 구성을 만들려면 오른쪽 상단에서 **만들기**&#x200B;를 선택한 다음 나타나는 구성 만들기 모달에서 양식을 작성합니다.

![WKND 공유 구성 검토](assets/overview/review-wknd-shared-configuration.png)

### GraphQL API 엔드포인트 검토

다음으로 GraphQL 쿼리를 보낼 API 엔드포인트를 구성해야 합니다. 기존 엔드포인트를 검토하거나 새 엔드포인트를 만들려면 **도구** > **일반** > **GraphQL**&#x200B;로 이동합니다.

![엔드포인트 구성](assets/overview/endpoints.png)

`WKND Shared Endpoint`가 이미 생성되어 있음을 확인합니다. 프로젝트에 대한 엔드포인트를 만들려면 오른쪽 상단에서 **만들기**&#x200B;를 선택한 다음 워크플로를 따릅니다.

![WKND 공유 엔드포인트 검토](assets/overview/review-wknd-shared-endpoint.png)

>[!NOTE]
>
> 엔드포인트를 저장하면 보안 콘솔을 방문하라는 모달이 표시됩니다. 여기서 엔드포인트에 대한 액세스를 구성하려는 경우 보안 설정을 조정할 수 있습니다. 그러나 보안 권한 자체는 이 튜토리얼의 범위를 벗어납니다. 자세한 내용은 [AEM 설명서](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security.html?lang=ko)를 참조하십시오.

### WKND 콘텐츠 구조 및 언어 루트 폴더 검토

명확하게 정의된 콘텐츠 구조는 AEM Headless 구현의 성공에 중요합니다. 이는 콘텐츠의 확장성, 유용성 및 권한 관리에 도움이 됩니다.

언어 루트 폴더는 EN이나 FR과 같이 이름에 ISO 언어 코드가 포함된 폴더입니다. AEM 번역 관리 시스템은 이러한 폴더를 사용하여 콘텐츠의 기본 언어와 콘텐츠 번역 언어를 정의합니다.

**탐색** > **자산** > **파일**&#x200B;로 이동합니다.

![파일로 이동](assets/overview/files.png)

**WKND 공유** 폴더로 이동합니다. 제목이 “영어”이고 이름이 “EN”인 폴더를 살펴봅니다. 이 폴더는 WKND 사이트 프로젝트의 언어 루트 폴더입니다.

![영어 폴더](assets/overview/english.png)

고유한 프로젝트를 위해 구성 내부에 언어 루트 폴더를 만듭니다. 자세한 내용은 [폴더 만들기](/help/headless-tutorial/graphql/advanced-graphql/author-content-fragments.md#create-folders)의 섹션을 참조하십시오.

### 중첩된 폴더에 구성 할당

마지막으로 프로젝트 구성을 언어 루트 폴더에 할당해야 합니다. 이렇게 하면 프로젝트 구성에 정의된 콘텐츠 조각 모델을 기반으로 콘텐츠 조각을 만들 수 있습니다.

구성에 언어 루트 폴더를 할당하려면 폴더를 선택한 다음 상단 탐색 막대에서 **속성**&#x200B;을 선택합니다.

![속성 선택](assets/overview/properties.png)

다음으로, **클라우드 서비스** 탭으로 이동하여 **클라우드 구성** 필드에서 폴더 아이콘을 선택합니다.

![클라우드 구성](assets/overview/cloud-conf.png)

표시되는 모달에서 이전에 만든 구성을 선택하여 언어 루트 폴더를 할당합니다.

### 모범 사례

AEM에서 프로젝트를 만들 때 참고할 수 있는 모범 사례는 다음과 같습니다.

* 폴더 계층 구조는 현지화와 번역을 염두에 두고 모델링해야 합니다. 즉, 언어 폴더가 구성 폴더 내에 중첩되어야 하며, 이렇게 하면 해당 구성 폴더 내의 콘텐츠를 쉽게 번역할 수 있습니다.
* 폴더 계층 구조는 단순하고 간단해야 합니다. 나중에 폴더와 조각을 이동하거나 이름을 바꾸지 마십시오. 특히 라이브 사용을 위해 게시한 후에는 경로가 변경되어 조각 참조와 GraphQL 쿼리에 영향을 줄 수 있습니다.

## 스타터 및 솔루션 패키지

두 개의 AEM **패키지**&#x200B;를 사용할 수 있으며, [패키지 관리자](/help/headless-tutorial/graphql/advanced-graphql/author-content-fragments.md#sample-content)를 통해 설치할 수 있습니다.

* [Advanced-GraphQL-Tutorial-Starter-Package-1.1.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/Advanced-GraphQL-Tutorial-Starter-Package-1.1.zip): 튜토리얼의 뒷부분에서 사용되며, 샘플 이미지와 폴더가 포함되어 있습니다.
* [Advanced-GraphQL-Tutorial-Solution-Package-1.2.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/Advanced-GraphQL-Tutorial-Solution-Package-1.2.zip): 새로운 콘텐츠 조각 모델, 콘텐츠 조각 및 지속 GraphQL 쿼리를 포함하여 1~4장에 대한 완성된 솔루션이 포함되어 있습니다. [클라이언트 애플리케이션 통합](/help/headless-tutorial/graphql/advanced-graphql/client-application-integration.md) 장으로 바로 건너뛰고자 하는 사용자에게 유용합니다.


[React 앱 - 고급 튜토리얼 - WKND 모험](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/advanced-tutorial/README.md) 프로젝트를 통해 샘플 애플리케이션을 검토하고 탐색할 수 있습니다. 이 샘플 애플리케이션은 지속 GraphQL 쿼리를 호출하여 AEM에서 콘텐츠를 검색하고 몰입형 환경으로 렌더링합니다.

## 시작하기

이 고급 튜토리얼을 시작하려면 다음 단계를 따르십시오.

1. [AEM as a Cloud Service](../quick-setup/cloud-service.md)를 사용하여 개발 환경을 설정합니다.
1. [콘텐츠 조각 모델 만들기](/help/headless-tutorial/graphql/advanced-graphql/create-content-fragment-models.md)에 대한 튜토리얼 장을 시작합니다.
