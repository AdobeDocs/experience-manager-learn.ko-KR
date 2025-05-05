---
title: 범용 편집기를 사용하여 React 앱 편집
description: 범용 편집기를 사용하여 샘플 React 앱의 콘텐츠를 편집하는 방법에 대해 알아봅니다.
version: Experience Manager as a Cloud Service
feature: Developer Tools, Headless
topic: Development, Content Management
role: Architect, Developer
level: Intermediate
doc-type: Tutorial
duration: 87
last-substantial-update: 2024-04-19T00:00:00Z
jira: KT-15359
thumbnail: KT-15359.png
exl-id: 69ed610e-2eff-43b3-98f9-3dc40594e879
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '415'
ht-degree: 0%

---

# 범용 편집기를 사용하여 React 앱 편집

범용 편집기를 사용하여 샘플 React 앱의 콘텐츠를 편집하는 방법에 대해 알아봅니다. 콘텐츠는 AEM의 콘텐츠 조각 내에 저장되며 GraphQL API를 사용하여 가져옵니다.

이 튜토리얼에서는 로컬 개발 환경을 설정하고, React 앱을 측정하여 콘텐츠를 편집하고, 범용 편집기를 사용하여 콘텐츠를 편집하는 프로세스를 안내합니다.

## 학습 내용

이 튜토리얼에서는 다음 주제를 다룹니다.

- 유니버설 편집기에 대한 간략한 개요
- 로컬 개발 환경 설정
   - **AEM SDK**: GraphQL API를 사용하여 React 앱용 콘텐츠 조각 내에 저장된 콘텐츠를 제공합니다.
   - **React 앱**: AEM의 콘텐츠를 표시하는 간단한 사용자 인터페이스입니다.
   - **유니버설 편집기 서비스**: 유니버설 편집기와 AEM SDK을 바인딩하는 _유니버설 편집기 서비스의 로컬 복사본_.
- 범용 편집기를 사용하여 콘텐츠를 편집하기 위해 React 앱을 측정하는 방법
- 범용 편집기를 사용하여 React 앱 콘텐츠를 편집하는 방법


## 유니버설 편집기 개요

범용 편집기를 통해 콘텐츠 작성자 및 개발자(프론트엔드 및 백엔드)가 작업할 수 있으므로 범용 편집기의 몇 가지 주요 이점을 살펴보겠습니다.

- 볼 수 있는 것(WYSIWYG) 모드에서 Headless 및 Headful 콘텐츠를 편집하도록 빌드되었습니다.
- React, Angular, Vue 등과 같은 다양한 프론트엔드 기술에 걸쳐 일관된 콘텐츠 편집 환경을 제공합니다. 따라서 콘텐츠 작성자는 기본 프론트엔드 기술에 대한 걱정 없이 콘텐츠를 편집할 수 있습니다.
- 프론트엔드 애플리케이션에서 유니버설 편집기를 활성화하려면 매우 최소한의 계기가 필요합니다. 따라서 개발자의 생산성을 극대화하고 경험을 구축하는 데 집중할 수 있습니다.
- 콘텐츠 작성자, 프론트엔드 개발자 및 백엔드 개발자의 세 가지 역할에 걸쳐 문제를 분리하여 각 역할이 핵심 책임에 집중할 수 있도록 했습니다.


## 샘플 React 앱

이 자습서에서는 [**WKND Teams**](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/basic-tutorial#react-app---basic-tutorial---teampersons)를 샘플 React 앱으로 사용합니다. **WKND Teams** React 앱에 팀원 목록과 세부 정보가 표시됩니다.

제목, 설명, 팀원 등 팀 세부 정보는 AEM에서 _팀_ 콘텐츠 조각으로 저장됩니다. 마찬가지로 이름, 약력 및 프로필 사진 등 개인 세부 정보는 AEM에서 _개인_ 콘텐츠 조각으로 저장됩니다.

React 앱의 콘텐츠는 GraphQL API를 사용하여 AEM에서 제공하며 사용자 인터페이스는 두 개의 React 구성 요소인 `Teams` 및 `Person`을(를) 사용하여 빌드되었습니다.

해당 튜토리얼을 통해 **WKND Teams** React 앱을 빌드하는 방법을 배울 수 있습니다. 자습서는 [여기](https://experienceleague.adobe.com/ko/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview)에서 찾을 수 있습니다.

## 다음 단계

[로컬 개발 환경을 설정](./local-development-setup.md)하는 방법에 대해 알아봅니다.
