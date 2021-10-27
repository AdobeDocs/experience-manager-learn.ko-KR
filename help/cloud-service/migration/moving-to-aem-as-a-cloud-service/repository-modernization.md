---
title: 저장소 현대화
description: 저장소 현대화, 변경 가능 및 변경할 수 없는 컨텐츠, 패키지 구조 및 repository modernizer CLI 도구에 대해 알아봅니다.
version: Cloud Service
feature: Developer Tools
topic: Migration, Upgrade
role: Developer
level: Experienced
kt: 8630
thumbnail: 336958.jpeg
exl-id: e9bd9035-1f2d-4a34-a581-9c1ec2c7bc04
source-git-commit: 1ddf6154d50a341d9a0fd4234392c37ced878a73
workflow-type: tm+mt
source-wordcount: '161'
ht-degree: 4%

---

# 저장소 현대화

저장소 현대화, 변경 가능 및 변경할 수 없는 컨텐츠, 패키지 구조 및 repository modernizer CLI 도구에 대해 알아봅니다.

>[!VIDEO](https://video.tv.adobe.com/v/336958/?quality=12&learn=on)

## Repository Modernizer 도구

![Repository Modernizer](./assets/repository-modernizer.png)

코드 베이스를 리팩터링하는 과정의 일부로, [Repository Modernizer 도구](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/refactoring-tools/repo-modernizer.html) 6.x 코드 베이스를 보다 현대적인 구조로 재구성하는 방법을 설명합니다.

### 주요 활동

* 를 사용하십시오 [Adobe I/O Repository Modernizer](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#command-aio-aem-migrationrepository-modernizer) AEM as a Cloud Service 프로젝트의 예상 구조와 일치하도록 프로젝트를 재구성하는 도구입니다.
* 업데이트된 코드 베이스에서 모든 빌드 오류를 수동으로 조정하고 수정합니다.
* 설정 [로컬 개발 환경](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html) 업데이트된 코드 베이스를 배포합니다. 프로젝트가 안정된 상태가 될 때까지 반복합니다.
* 업데이트된 코드 베이스를 AEM as a Cloud Service 개발 환경에 배포하고 계속 유효성을 검사합니다.
