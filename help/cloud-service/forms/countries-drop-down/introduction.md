---
title: 국가 만들기 드롭다운 목록 구성 요소
description: Aem Forms 핵심 드롭다운 구성 요소를 기반으로 국가 드롭다운 목록 구성 요소를 만듭니다.
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
topic: Development
feature: Adaptive Forms
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
jira: KT-16517
exl-id: aef151bc-daf1-4abd-914a-6299f3fb58e4
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '245'
ht-degree: 1%

---

# 드롭다운 구성 요소를 기반으로 국가 드롭다운 구성 요소를 만듭니다

Adobe Experience Manager(AEM)에서 새 핵심 구성 요소를 만드는 것은 구성 요소 구조 정의, 대화 상자 맞춤화 및 동적 기능을 위한 슬링 모델 구현 등 여러 단계를 포함하는 흥미로운 프로세스입니다.

이 자습서가 끝날 때까지 다음 방법을 숙지할 수 있습니다.

* Sling 모델을 만들어 사용하여 데이터를 동적으로 가져옵니다.
* 새 필드를 추가하고 다른 필드를 숨겨서 cq 대화 상자를 사용자 지정합니다.
* 재사용을 위해 맞춤화된 강력한 구성 요소 구조를 정의합니다.

&quot;국가&quot;라는 구성 요소를 통해 사용자가 대륙을 선택하고 선택한 대륙에 해당하는 국가로 드롭다운을 동적으로 채울 수 있습니다. 기본 드롭다운 목록 구성 요소를 기반으로 빌드됩니다. 이 특정 사용 사례에 맞게 개선되었습니다.

자세히 살펴보고 이 역동적이고 강력한 구성 요소를 만들어 보겠습니다!

## 사전 요구 사항

Adobe Experience Manager(AEM)에서 새 핵심 구성 요소를 빌드하려면 원활한 개발 프로세스를 보장하기 위한 몇 가지 전제 조건을 충족해야 합니다. 시작하기 전에 필요한 사항은 다음과 같습니다.

* AEM 개발 환경: 로컬에서 실행 중인 클라우드 기반의 기능 설치
* Visual Studio Code 또는 IntelliJ와 같은 AEM 개발 도구에 액세스
* 최신 Archetype이 포함된 MAven 설정 및 AEM 프로젝트
* AEM 개념에 대한 기본 지식
* Git 저장소, JDK의 오른쪽 버전 등 기본 도구 및 설정


## 다음 단계

[구성 요소 구조 만들기](./component.md)
