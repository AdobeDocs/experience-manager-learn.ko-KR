---
title: 저장소 현대화
description: 저장소 현대화, 변경 가능한 콘텐츠 및 변경 불가능한 콘텐츠, 패키지 구조 및 저장소 현대화 CLI 도구에 대해 알아봅니다.
version: Cloud Service
feature: Developer Tools
topic: Migration, Upgrade
role: Developer
level: Experienced
jira: KT-8630
thumbnail: 336958.jpeg
exl-id: e9bd9035-1f2d-4a34-a581-9c1ec2c7bc04
duration: 1305
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '145'
ht-degree: 0%

---

# 저장소 현대화

저장소 현대화, 변경 가능한 콘텐츠 및 변경 불가능한 콘텐츠, 패키지 구조 및 저장소 현대화 CLI 도구에 대해 알아봅니다.

>[!VIDEO](https://video.tv.adobe.com/v/336958?quality=12&learn=on)

## 저장소 현대화 도구

![저장소 현대화 도구](./assets/repository-modernizer.png)

코드 베이스를 리팩터링하는 과정의 일부로 [저장소 현대화 도구](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/refactoring-tools/repo-modernizer.html) 6.x 코드 베이스를 보다 최신 구조로 재구성합니다.

## 주요 활동

* 사용 [Adobe I/O 리포지토리 현대화 도구](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#command-aio-aem-migrationrepository-modernizer) AEM as a Cloud Service 프로젝트의 예상 구조와 일치하도록 프로젝트를 재구성하는 도구입니다.
* 업데이트된 코드 베이스에서 빌드 오류를 수동으로 조정하고 수정합니다.
* 설정 [로컬 개발 환경](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=ko-KR) 업데이트된 코드 베이스를 배포합니다. 프로젝트가 안정적인 상태가 될 때까지 반복합니다.
* 업데이트된 코드 베이스를 AEM as a Cloud Service 개발 환경에 배포하고 계속 유효성을 검사합니다.
