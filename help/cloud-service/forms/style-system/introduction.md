---
title: AEM Forms에서 스타일 시스템 사용
description: 버튼 구성 요소의 스타일 변형 만들기
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
topic: Development
feature: Adaptive Forms
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
jira: KT-16276
exl-id: f97a9aed-99d3-4cce-bc3b-c80638e4f1cb
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '231'
ht-degree: 1%

---

# 소개

Adobe Experience Manager(AEM)의 스타일 시스템 을 사용하면 구성 요소의 여러 가지 시각적 변형을 만든 다음 양식을 작성할 때 사용할 스타일을 선택할 수 있습니다. 이렇게 하면 각 스타일에 대한 사용자 지정 구성 요소를 만들 필요 없이 구성 요소를 보다 유연하고 재사용할 수 있습니다.

이 문서는 cloud manager를 사용하여 클라우드 인스턴스에 변경 사항을 푸시하기 전에 버튼 구성 요소의 변형을 만들고 로컬 클라우드 준비 환경에서 변형을 테스트하는 데 도움이 됩니다.

이 스크린샷은 양식 작성자가 사용할 수 있는 버튼 구성 요소에 대한 2가지 스타일 변형을 보여 줍니다.


![단추 변형](assets/button-variations.png)

## 사전 요구 사항

* 핵심 구성 요소가 포함된 AEM Forms cloud ready 인스턴스
* 테마 복제: 테마를 복제하는 데 익숙해야 합니다. 이 자습서에서는 [이젤 테마](https://github.com/adobe/aem-forms-theme-easel)를 복제했습니다. 필요에 따라 사용 가능한 테마를 복제할 수 있습니다.

* Apache Maven의 최신 릴리스를 설치합니다. Apache Maven은 Java™ 프로젝트에 일반적으로 사용되는 빌드 자동화 도구입니다. 최신 릴리스를 설치하면 테마 맞춤화에 필요한 종속성이 확보됩니다.
* 일반 텍스트 편집기를 설치합니다. 예를 들어 Microsoft® Visual Studio Code입니다. Microsoft® 같은 일반 텍스트 편집기를 사용하면 Visual Studio Code에서 테마 파일을 편집하고 수정할 수 있는 사용자 친화적인 환경을 제공합니다.



## 다음 단계

[스타일 정책 만들기](./style-policy.md)
