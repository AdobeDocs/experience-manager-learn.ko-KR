---
title: AEM Assets 마이크로서비스 및 AEM as a Cloud Service으로 이동
description: AEM Assets as a Cloud Service의 asset compute 마이크로서비스를 통해 기존의 AEM Workflow에서 이 역할을 대체하여 자산에 대한 렌디션을 자동으로 효율적으로 생성할 수 있는 방법을 알아봅니다.
version: Cloud Service
feature: Asset Compute Microservices
topic: Migration, Upgrade
role: Developer
level: Experienced
kt: 8635
thumbnail: 336990.jpeg
exl-id: 327e8663-086b-4b31-b159-a0cf30480b45
source-git-commit: 1ddf6154d50a341d9a0fd4234392c37ced878a73
workflow-type: tm+mt
source-wordcount: '213'
ht-degree: 5%

---

# AEM Assets 마이크로서비스 - AEM as a Cloud Service으로 이동

AEM Assets as a Cloud Service의 asset compute 마이크로서비스를 통해 기존의 AEM Workflow에서 이 역할을 대체하여 자산에 대한 렌디션을 자동으로 효율적으로 생성할 수 있는 방법을 알아봅니다.

>[!VIDEO](https://video.tv.adobe.com/v/336990/?quality=12&learn=on)

## 워크플로우 마이그레이션 도구

![자산 워크플로우 마이그레이션 도구](./assets/asset-workflow-migration.png)

코드 베이스를 리팩터링하는 과정의 일부로, [자산 워크플로우 마이그레이션 도구](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/refactoring-tools/asset-workflow-migration-tool.html) 기존 워크플로우를 마이그레이션하여 AEM as a Cloud Service에서 Asset compute 마이크로 서비스를 사용합니다.

### 주요 활동

* 를 사용하십시오 [Adobe I/O Workflow Migrator](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#command-aio-aem-migrationworkflow-migrator) asset compute 마이크로서비스를 사용하도록 자산 처리 워크플로우를 마이그레이션하는 도구입니다.
* 설정 [로컬 개발 환경](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html) 업데이트된 워크플로우를 배포합니다. 복잡한 워크플로우에 대해 수동 조정이 필요할 수 있습니다.
* 업데이트된 워크플로우가 기능 패리티가 일치할 때까지 AEM SDK를 사용하여 로컬 개발 환경에서 계속 반복합니다.
* 업데이트된 코드 베이스를 AEM as a Cloud Service 개발 환경에 배포하고 계속 유효성을 검사합니다.

