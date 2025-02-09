---
title: AEM Sites에서 태그 Cloud Service 구성 만들기
description: AEM에서 태그 Cloud Service 구성을 만드는 방법을 알아봅니다.
solution: Experience Manager, Data Collection, Experience Platform
jira: KT-5982
thumbnail: 38566.jpg
topic: Integrations
feature: Integrations
role: Developer
level: Intermediate
badgeIntegration: label="통합" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: a72ddced-37de-4b62-9e28-fa5b6c8ce5b7
duration: 99
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '495'
ht-degree: 0%

---

# AEM에서 태그 Cloud Service 구성 만들기 {#create-launch-cloud-service}

Adobe Experience Manager에서 태그 Cloud Service 구성을 만드는 방법을 알아봅니다. 그런 다음 AEM의 태그 Cloud Service 구성을 기존 사이트에 적용할 수 있으며 태그 라이브러리가 작성자 및 Publish 환경 모두에서 로드되는 것을 확인할 수 있습니다.

## 태그 클라우드 서비스 만들기

아래 단계를 사용하여 태그 클라우드 서비스 구성을 만듭니다.

1. **Cloud Service** 메뉴에서 **도구** 섹션을 선택하고 **시작 구성 Adobe**&#x200B;를 클릭합니다
1. 사이트의 구성 폴더를 선택하거나 **WKND 사이트**(WKND 안내서 프로젝트를 사용하는 경우)를 선택하고 **만들기**&#x200B;를 클릭합니다
1. _일반_ 탭에서 **제목** 필드를 사용하여 구성의 이름을 지정하고 _연결된 Adobe IMS 구성_ 드롭다운에서 **시작 Adobe**&#x200B;을 선택합니다. 그런 다음 _회사_ 드롭다운에서 회사 이름을 선택하고 _속성_ 드롭다운에서 이전에 만든 속성을 선택합니다.
1. _스테이징_ 및 _프로덕션_ 탭에서 기본 구성을 유지합니다. 그러나 실제 프로덕션 설정에 대한 구성, 특히 성능 및 최적화 요구 사항에 따라 _라이브러리를 비동기적으로 로드_ 토글을 검토하고 변경하는 것이 좋습니다. 또한 _라이브러리 URI_ 값은 스테이징과 프로덕션에 대해 다릅니다.
1. 마지막으로 **만들기**&#x200B;를 클릭하여 태그 클라우드 서비스를 완료합니다.

   ![태그 Cloud Service 구성](assets/launch-cloud-services-config.png)

## 사이트에 태그 클라우드 서비스 적용

Tag 속성과 해당 라이브러리를 AEM 사이트에 로드하려면 태그 클라우드 서비스 구성이 사이트에 적용됩니다. 이전 단계에서는 클라우드 서비스 구성이 사이트 이름 폴더(WKND 사이트) 아래에 생성되므로 자동으로 적용되어야 합니다. 이를 확인하겠습니다.

1. **탐색** 메뉴에서 **사이트** 아이콘을 선택합니다.

1. AEM 사이트의 루트 페이지를 선택하고 **속성**&#x200B;을 클릭합니다. 그런 다음 **고급** 탭으로 이동하고 **구성** 섹션에서 클라우드 구성 값이 사이트별 `conf` 폴더를 가리키는지 확인하십시오.

   ![사이트에 Cloud Service 구성 적용](assets/apply-cloud-services-config-to-site.png)

## 작성자 및 Publish 페이지에서 태그 속성 로드 확인

이제 Tag 속성과 해당 라이브러리가 AEM 사이트 페이지에 로드되었는지 확인할 차례입니다.

1. **게시됨으로 보기** 모드에서 즐겨 찾는 사이트 페이지를 열면 로그 메시지가 표시됩니다. _Library Loaded (Page Top)_ 이벤트가 트리거될 때 실행되는 Tag 속성 규칙의 JavaScript 코드 조각에서 보내는 메시지와 동일합니다.

1. Publish에서 확인하려면 먼저 **태그 클라우드 서비스** 구성을 게시하고 Publish 인스턴스에서 사이트 페이지를 엽니다.

   ![작성자 및 Publish 페이지의 태그 속성](assets/tag-property-on-author-publish-pages.png)

축하합니다! AEM 프로젝트 코드를 업데이트하지 않고 AEM 사이트에 JavaScript 코드를 삽입하는 AEM 및 데이터 수집 태그 통합을 완료했습니다.

## 과제 - 태그 속성의 규칙 업데이트 및 게시

이전 [태그 속성 만들기](./create-tag-property.md)에서 얻은 학습 내용을 사용하여 간단한 과제를 완료하고, 기존 규칙을 업데이트하여 콘솔 문을 추가하고, _게시 흐름_&#x200B;을 사용하여 AEM 사이트에 배포합니다.

## 다음 단계

[태그 구현 디버깅](debug-tags-implementation.md)
