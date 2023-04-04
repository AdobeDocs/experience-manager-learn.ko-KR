---
title: 개발 고려 사항
description: 프런트엔드 파이프라인을 활성화하면 프런트엔드 및 백엔드 개발 프로세스에 미치는 영향을 고려하십시오.
version: Cloud Service
type: Tutorial
feature: AEM Project Archetype, Cloud Manager, CI-CD Pipeline
topic: Content Management, Development, Development, Architecture
role: Developer, Architect, Admin
level: Intermediate
kt: 10689
mini-toc-levels: 1
index: y
recommendations: noDisplay, noCatalog
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '201'
ht-degree: 0%

---


# 개발 고려 사항

프런트엔드 파이프라인이 AEM as a Cloud Service 환경에서만 프런트엔드 리소스를 배포하도록 활성화한 후 로컬 AEM 개발에 몇 가지 영향을 주고 git 분기 모델을 조정해야 합니다.

## 목표

* 마찰이 없는 프런트 엔드 및 백 엔드 개발 흐름을 만드는 방법
* 전체 스택과 프런트 엔드 파이프라인 간의 종속성을 검토합니다.


## 로컬 개발 고려 사항

>[!VIDEO](https://video.tv.adobe.com/v/3409421?quality=12&learn=on)


## 개발 접근 방식 조정

* AEM SDK를 사용하는 로컬 개발에 대해 백엔드 개발 팀은 여전히 를 통해 clientlib 생성을 필요로 합니다. `ui.frontend` 모듈이 있지만 AEM as a Cloud Service 환경에 Cloud Manager 배포 중에 이 모듈을 건너뛰어야 합니다. 이에 따라, [프로젝트 업데이트](update-project.md) 제2장.

A __솔루션__ git 분기 모델을 조정하고 AEM 프로젝트 구성 변경 사항이 다시 로 이동하지 않는지 확인할 수 있습니다. __지역 개발__ AEM 백엔드 개발자가 사용하는 것을 분기합니다.


* AEM 프로젝트에 대한 지속적인 개선 사항의 일부로서, 새 구성 요소를 도입하거나 두 구성 요소에 변경 사항이 있는 기존 구성 요소를 업데이트하는 경우 `ui.app` 및 `ui.frontend` 모듈, 전체 스택과 프런트 엔드 파이프라인을 모두 실행해야 합니다.



