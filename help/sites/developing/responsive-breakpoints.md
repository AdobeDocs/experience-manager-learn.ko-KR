---
title: 응답형 중단점
description: AEM 반응형 페이지 편집기에 대한 새 반응형 중단점을 구성하는 방법에 대해 알아봅니다.
version: Experience Manager as a Cloud Service
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
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '256'
ht-degree: 0%

---

# 응답형 중단점

AEM 반응형 페이지 편집기에 대한 새 반응형 중단점을 구성하는 방법에 대해 알아봅니다.

## CSS 중단점 만들기

먼저 반응형 AEM 사이트가 준수하는 AEM 반응형 그리드 CSS에서 미디어 중단점을 만듭니다.

`/ui.apps/src/main/content/jcr_root/apps/[app name]/clientlibs/clientlib-grid/less/grid.less` 파일에서 모바일 에뮬레이터와 함께 사용할 중단점을 만듭니다. CSS 중단점을 AEM 반응형 페이지 편집기 중단점에 매핑하므로 각 중단점에 대한 `max-width`을(를) 메모하십시오.

![새 반응형 중단점 만들기](./assets/responsive-breakpoints/create-new-breakpoints.jpg)

## 템플릿의 중단점 사용자 지정

`ui.content/src/main/content/jcr_root/conf/<app name>/settings/wcm/templates/page-content/structure/.content.xml` 파일을 열고 `cq:responsive/breakpoints`을(를) 새 중단점 노드 정의로 업데이트합니다. 각 [CSS 중단점](#create-new-css-breakpoints)에는 `breakpoints` 아래에 해당 노드가 있어야 하며, `width` 속성은 CSS 중단점의 `max-width`(으)로 설정되어 있습니다.

![템플릿의 응답형 중단점 사용자 지정](./assets/responsive-breakpoints/customize-template-breakpoints.jpg)

## 에뮬레이터 만들기

작성자가 페이지 편집기에서 편집할 응답형 보기를 선택할 수 있도록 AEM 에뮬레이터를 정의해야 합니다.

`/ui.apps/src/main/content/jcr_root/apps/<app name>/emulators` 아래에 에뮬레이터 노드 만들기

예, `/ui.apps/src/main/content/jcr_root/apps/wknd-examples/emulators/phone-landscape`. CRXDE Lite의 `/libs/wcm/mobile/components/emulators`에서 참조 에뮬레이터 노드를 로 복사하고 복사본을 업데이트하여 노드 정의를 신속하게 진행합니다.

![새 에뮬레이터 만들기](./assets/responsive-breakpoints/create-new-emulators.jpg)

## 장치 그룹 만들기

에뮬레이터를 [AEM 페이지 편집기에서 사용할 수 있도록](#update-the-templates-device-group) 그룹화합니다.

`/ui.apps/src/main/content/jcr_root` 아래에 `/apps/settings/mobile/groups/<name of device group>` 노드 구조를 만듭니다.

![새 장치 그룹 만들기](./assets/responsive-breakpoints/create-new-device-group.jpg)

`/apps/settings/mobile/groups/<device group name>`에서 `.content.xml` 파일을 만들고 다음을 정의합니다.
아래 코드와 유사한 코드를 사용하는 새 에뮬레이터:

![새 장치 만들기](./assets/responsive-breakpoints/create-new-device.jpg)

## 템플릿의 장치 그룹 업데이트

마지막으로, 장치 그룹을 페이지 템플릿에 다시 매핑하여 이 템플릿으로 만든 페이지에 대한 에뮬레이터를 페이지 편집기에서 사용할 수 있도록 합니다.

`ui.content/src/main/content/jcr_root/conf/[app name]/settings/wcm/templates/page-content/structure/.content.xml` 파일을 열고 `cq:deviceGroups` 속성을 업데이트하여 새 모바일 그룹(예: `cq:deviceGroups="[mobile/groups/customdevices]"`)을 참조합니다.
