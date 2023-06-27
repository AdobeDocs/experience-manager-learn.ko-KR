---
title: AEM에서 Launch Cloud Service 구성 만들기
description: AEM에서 Launch Cloud Service 구성을 만드는 방법을 알아봅니다. 그런 다음 Launch Cloud Service 구성을 기존 사이트에 적용할 수 있으며 태그 라이브러리가 작성자 및 게시 환경 모두에서 로드되는 것을 확인할 수 있습니다.
topics: integrations
audience: administrator
solution: Experience Manager, Data Collection, Experience Platform
doc-type: technical video
activity: setup
kt: 5982
thumbnail: 38566.jpg
topic: Integrations
feature: Integrations
role: Developer
level: Intermediate
exl-id: a72ddced-37de-4b62-9e28-fa5b6c8ce5b7
source-git-commit: eecc275e38390b9330464c8ac0750efa2c702c82
workflow-type: tm+mt
source-wordcount: '546'
ht-degree: 0%

---

# AEM에서 Launch Cloud Service 구성 만들기 {#create-launch-cloud-service}

>[!NOTE]
>
>Adobe Experience Platform Launch의 이름을 데이터 수집 기술 집합으로 바꾸는 프로세스가 AEM 제품 UI, 콘텐츠 및 설명서에서 구현되고 있으므로 Launch라는 용어는 여전히 여기에서 사용되고 있습니다.

Adobe Experience Manager에서 Launch Cloud Service 구성을 만드는 방법을 알아봅니다. 그런 다음 AEM Launch Cloud Service 구성을 기존 사이트에 적용할 수 있으며 태그 라이브러리가 작성자 및 게시 환경 모두에서 로드되는 것을 확인할 수 있습니다.

## Launch 클라우드 서비스 만들기

아래 단계를 사용하여 Launch Cloud Service 구성을 만듭니다.

1. 다음에서 **도구** 메뉴, 선택 **Cloud Services** 섹션 및 클릭 **Adobe 실행 구성**

1. 사이트의 구성 폴더를 선택하거나 **WKND 사이트** (WKND 안내서 프로젝트를 사용하는 경우) 를 클릭하고 **만들기**

1. 다음에서 _일반_ 탭, 다음을 사용하여 구성 이름 지정 **제목** 필드 및 선택 **Adobe 실행** 다음에서 _연계된 Adobe IMS 구성_ 드롭다운입니다. 그런 다음,에서 회사 이름을 선택합니다. _회사_ 드롭다운을 클릭하고 다음에서 이전에 만든 속성을 선택합니다. _속성_ 드롭다운입니다.

1. 다음에서 _스테이징_ 및 _프로덕션_ 탭은 기본 구성을 유지합니다. 그러나 실제 프로덕션 설정에 대한 구성, 특히 _라이브러리를 비동기적으로 로드_ 성능 및 최적화 요구 사항에 따라 전환합니다. 또한 _라이브러리 URI_ 스테이징 및 프로덕션에 대한 값이 다릅니다.

1. 마지막으로 **만들기** 을 클릭하여 Launch 클라우드 서비스를 완료합니다.

   ![Launch Cloud Services 구성](assets/launch-cloud-services-config.png)

## 사이트에 Launch Cloud Service 적용

Tag 속성과 해당 라이브러리를 AEM 사이트에 로드하려면 Launch 클라우드 서비스 구성이 사이트에 적용됩니다. 이전 단계에서는 클라우드 서비스 구성이 사이트 이름 폴더(WKND 사이트) 아래에 생성되므로 자동으로 적용되어야 합니다. 이를 확인하겠습니다.

1. 다음에서 **탐색** 메뉴, 선택 **사이트** 아이콘.

1. AEM 사이트의 루트 페이지를 선택하고 **속성**. 그런 다음 로 이동합니다. **고급** 탭 및 아래 **구성** 섹션에서 클라우드 구성 값이 사이트별 을 가리키는지 확인합니다. `conf` 폴더를 삭제합니다.

   ![사이트에 Cloud Services 구성 적용](assets/apply-cloud-services-config-to-site.png)

## 작성자 및 게시 페이지에서 태그 속성 로드 확인

이제 Tag 속성과 해당 라이브러리가 AEM 사이트 페이지에 로드되었는지 확인할 차례입니다.

1. 에서 즐겨찾는 사이트 페이지를 엽니다. **게시됨으로 보기** 모드: 브라우저 콘솔에 로그 메시지가 표시됩니다. 이는 다음과 같은 경우 실행되는 Tag 속성 규칙의 JavaScript 코드 조각의 메시지와 동일합니다. _라이브러리가 로드됨 (페이지 상단)_ 이벤트가 트리거됩니다.

1. 게시에서 확인하려면 먼저 **클라우드 서비스 시작** 을(를) 구성하고 게시 인스턴스에서 사이트 페이지를 엽니다.

   ![작성자 및 게시 페이지의 태그 속성](assets/tag-property-on-author-publish-pages.png)

축하합니다! AEM 프로젝트 코드를 업데이트하지 않고 JavaScript 코드를 AEM 사이트에 삽입하는 AEM 및 데이터 수집 태그 통합을 완료했습니다.

## 과제 - 태그 속성의 규칙 업데이트 및 게시

이전 단원 사용 [태그 속성 만들기](./create-tag-property.md) 간단한 문제를 완료하려면 기존 규칙을 업데이트하여 추가 콘솔 문을 추가하고 _게시 플로우_ AEM 사이트에 배포합니다.

## 다음 단계

[태그 구현 디버깅](debug-tags-implementation.md)
