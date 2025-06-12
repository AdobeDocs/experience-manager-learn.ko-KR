---
title: 범용 편집기를 사용한 React 애플리케이션 편집
description: 범용 편집기를 사용하여 샘플 React 애플리케이션의 콘텐츠를 편집하는 방법을 알아봅니다.
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
workflow-type: ht
source-wordcount: '415'
ht-degree: 100%

---

# 범용 편집기를 사용한 React 애플리케이션 편집

범용 편집기를 사용하여 샘플 React 애플리케이션의 콘텐츠를 편집하는 방법을 알아봅니다. 콘텐츠는 AEM의 콘텐츠 조각 내에 저장되며 GraphQL API를 사용하여 가져옵니다.

이 튜토리얼에서는 로컬 개발 환경을 설정하고, React 앱의 콘텐츠를 편집하도록 계측하고, 범용 편집기를 사용하여 콘텐츠를 편집하는 과정을 안내합니다.

## 학습 내용

이 튜토리얼에서는 다음 주제를 다룹니다.

- 범용 편집기에 대한 간략한 개요
- 로컬 개발 환경 설정
   - **AEM SDK**: GraphQL API를 사용하여 React 앱의 콘텐츠 조각 내에 저장된 콘텐츠를 제공합니다.
   - **React 앱**: AEM의 콘텐츠를 표시하는 간단한 사용자 인터페이스입니다.
   - **범용 편집기 서비스**: 범용 편집기와 AEM SDK를 연결하는 _범용 편집기 서비스의 로컬 사본_&#x200B;입니다.
- 범용 편집기를 사용하여 콘텐츠를 편집하도록 React 앱을 계측하는 방법
- 범용 편집기를 사용하여 React 앱 콘텐츠를 편집하는 방법


## 범용 편집기 개요

범용 편집기는 콘텐츠 작성자와 개발자(프론트엔드 및 백엔드)를 지원합니다. 범용 편집기의 주요 이점 중 일부를 살펴보겠습니다.

- WYSIWYG(What You See Is What You Get) 모드로 Headless 및 Headful 콘텐츠를 편집할 수 있도록 설계되었습니다.
- React, Angular, Vue 등 다양한 프론트엔드 기술 전반에 걸쳐 일관된 콘텐츠 편집 경험을 제공합니다. 따라서 콘텐츠 작성자는 기반이 되는 프론트엔드 기술에 대해 걱정하지 않고 콘텐츠를 편집할 수 있습니다.
- 프론트엔드 애플리케이션에서 범용 편집기를 활성화하기 위해서는 최소한의 계측만 필요합니다. 이를 통해 개발자의 생산성을 극대화하고 경험 빌드에 집중할 수 있습니다.
- 콘텐츠 작성자, 프론트엔드 개발자 및 백엔드 개발자 등 세 가지 역할 간 관심 분야를 분리함으로써 각자가 핵심 업무에 집중할 수 있도록 지원합니다.


## 샘플 React 앱

이 튜토리얼에서는 [**WKND Teams**](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/basic-tutorial#react-app---basic-tutorial---teampersons)를 샘플 React 앱으로 사용합니다. **WKND Teams** React 앱에는 팀원 목록과 각 팀원의 세부 정보가 표시됩니다.

제목, 설명 및 팀원과 같은 팀 세부 정보는 AEM의 _팀_ 콘텐츠 조각으로 저장됩니다. 마찬가지로 이름, 약력 및 프로필 사진과 같은 개인 정보는 AEM의 _개인_ 콘텐츠 조각으로 저장됩니다.

React 앱의 콘텐츠는 AEM의 GraphQL API를 통해 제공되며, 사용자 인터페이스는 두 개의 React 구성 요소인 `Teams`와 `Person`을 사용하여 빌드되었습니다.

해당 튜토리얼을 통해 **WKND Teams** React 앱을 빌드하는 방법을 알아볼 수 있습니다. 튜토리얼은 [여기](https://experienceleague.adobe.com/ko/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview)에서 찾을 수 있습니다.

## 다음 단계

[로컬 개발 환경 설정](./local-development-setup.md) 방법에 대해 알아봅니다.
