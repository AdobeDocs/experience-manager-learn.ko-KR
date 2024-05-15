---
title: 응답형 중단점
description: AEM 반응형 페이지 편집기에 대한 새 반응형 중단점을 구성하는 방법에 대해 알아봅니다.
version: Cloud Service
feature: Page Editor
topic: Mobile, Development
role: Developer
level: Intermediate
doc-type: Article
last-substantial-update: 2023-01-05T00:00:00Z
jira: KT-11664
thumbnail: kt-11664.jpeg
exl-id: 8b48c28f-ba7f-4255-be96-a7ce18ca208b
duration: 52
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '256'
ht-degree: 0%

---

# 응답형 중단점

AEM 반응형 페이지 편집기에 대한 새 반응형 중단점을 구성하는 방법에 대해 알아봅니다.

## CSS 중단점 만들기

먼저, 응답형 AEM 사이트가 준수하는 AEM 반응형 그리드 CSS에서 미디어 중단점을 만듭니다.

위치 `/ui.apps/src/main/content/jcr_root/apps/[app name]/clientlibs/clientlib-grid/less/grid.less` 파일을 만들 때 모바일 에뮬레이터와 함께 사용할 중단점을 만듭니다. 다음을 기록해 둡니다. `max-width` 각 중단점에 대해 CSS 중단점을 AEM 반응형 페이지 편집기 중단점에 매핑하므로

![새 반응형 중단점 만들기](./assets/responsive-breakpoints/create-new-breakpoints.jpg)

## 템플릿의 중단점 사용자 지정

를 엽니다. `ui.content/src/main/content/jcr_root/conf/<app name>/settings/wcm/templates/page-content/structure/.content.xml` 파일 및 업데이트 `cq:responsive/breakpoints` (새 중단점 노드 정의 포함) 각 [CSS 중단점](#create-new-css-breakpoints) 에는 해당 노드가 있어야 합니다. `breakpoints` (포함) `width` css 중단점으로 설정된 속성 `max-width`.

![템플릿의 반응형 중단점 사용자 지정](./assets/responsive-breakpoints/customize-template-breakpoints.jpg)

## 에뮬레이터 만들기

작성자가 페이지 편집기에서 편집할 응답형 보기를 선택할 수 있도록 AEM 에뮬레이터를 정의해야 합니다.

아래에 에뮬레이터 노드 만들기 `/ui.apps/src/main/content/jcr_root/apps/<app name>/emulators`

예, `/ui.apps/src/main/content/jcr_root/apps/wknd-examples/emulators/phone-landscape`. 에서 참조 에뮬레이터 노드 복사 `/libs/wcm/mobile/components/emulators` 에 CRXDE Lite을 지정하고 사본을 업데이트하여 노드 정의를 신속하게 만듭니다.

![새 에뮬레이터 만들기](./assets/responsive-breakpoints/create-new-emulators.jpg)

## 장치 그룹 만들기

에뮬레이터를 그룹화하여 [AEM 페이지 편집기에서 사용할 수 있도록 설정](#update-the-templates-device-group).

만들기 `/apps/settings/mobile/groups/<name of device group>` 아래의 노드 구조 `/ui.apps/src/main/content/jcr_root`.

![새 장치 그룹 만들기](./assets/responsive-breakpoints/create-new-device-group.jpg)

만들기 `.content.xml` 파일 위치 `/apps/settings/mobile/groups/<device group name>` 아래 코드와 유사한 코드를 사용하여 새 에뮬레이터를 정의합니다.

![새 장치 만들기](./assets/responsive-breakpoints/create-new-device.jpg)

## 템플릿의 장치 그룹 업데이트

마지막으로, 장치 그룹을 페이지 템플릿에 다시 매핑하여 이 템플릿으로 만든 페이지에 대한 에뮬레이터를 페이지 편집기에서 사용할 수 있도록 합니다.

를 엽니다. `ui.content/src/main/content/jcr_root/conf/[app name]/settings/wcm/templates/page-content/structure/.content.xml` 파일 및 업데이트 `cq:deviceGroups` 새 모바일 그룹을 참조할 속성(예: `cq:deviceGroups="[mobile/groups/customdevices]"`)
