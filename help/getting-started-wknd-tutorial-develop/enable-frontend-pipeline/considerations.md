---
title: 개발 고려 사항
description: 프론트엔드 파이프라인을 활성화하면 프론트엔드 및 백엔드 개발 프로세스에 미치는 영향을 고려합니다.
version: Cloud Service
feature: AEM Project Archetype, Cloud Manager, CI-CD Pipeline
topic: Content Management, Development, Development, Architecture
role: Developer, Architect, Admin
level: Intermediate
jira: KT-10689
mini-toc-levels: 1
index: y
recommendations: noDisplay, noCatalog
doc-type: Tutorial
exl-id: a3b27d5b-b167-4c60-af49-8f2e8d814c86
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '201'
ht-degree: 0%

---

# 개발 고려 사항

프론트엔드 파이프라인을 활성화하여 AEM as a Cloud Service 환경의 프론트엔드 리소스만 배포하면, 로컬 AEM 개발에 어느 정도 영향을 미치며, git 분기 모델을 수정해야 합니다.

## 목표

* 마찰 없는 프론트엔드 및 백엔드 개발 플로우를 사용하는 방법
* 전체 스택 파이프라인과 프론트엔드 파이프라인 간의 종속성 검토


## 로컬 개발 고려 사항

>[!VIDEO](https://video.tv.adobe.com/v/3409421?quality=12&learn=on)


## 조정된 개발 접근 방식

* AEM SDK를 사용하는 로컬 개발의 경우 백엔드 개발 팀은 다음을 통해 clientlib을 생성해야 합니다. `ui.frontend` 모듈이지만 Cloud Manager를 AEM as a Cloud Service 환경에 배포하는 동안에는 건너뜁니다. 이 경우 다음에 설명된 프로젝트 구성 변경 사항을 분리하는 방법에 대한 문제가 나타납니다. [프로젝트 업데이트](update-project.md) 챕터.

A __솔루션__ git 분기 모델을 조정하고 AEM 프로젝트 구성 변경 사항이 __로컬 개발__ AEM 백엔드 개발자가 사용하는 를 분기합니다.


* AEM 프로젝트에 대한 지속적인 개선 사항의 일부로, 새 구성 요소를 도입하거나 두 구성 요소에 변경 사항이 있는 기존 구성 요소를 업데이트하는 경우 `ui.app` 및 `ui.frontend` 모듈에서는 전체 스택 파이프라인과 프론트엔드 파이프라인을 모두 실행해야 합니다.
