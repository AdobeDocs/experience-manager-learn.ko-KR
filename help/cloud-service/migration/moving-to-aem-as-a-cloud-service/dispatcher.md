---
title: AEM as a Cloud Service으로 이동할 때 Dispatcher 구성
description: AEM Dispatcher for AEM as a Cloud Service, Dispatcher 변환 도구 및 Dispatcher 도구 SDK를 사용하는 방법에 대한 주요 변경 사항을 알아봅니다.
version: Cloud Service
feature: Dispatcher
topic: Migration, Upgrade
role: Developer
level: Experienced
kt: 8633
thumbnail: 336962.jpeg
exl-id: 81397b21-b4f3-4024-a6da-a9b681453eff
source-git-commit: 53a314c5cd9eaad5a26a0992c750c159f8e3697f
workflow-type: tm+mt
source-wordcount: '174'
ht-degree: 2%

---

# Dispatcher

Dispatcher for AEM 6, Dispatcher 전환 도구 및 Dispatcher 도구 SDK를 사용하는 방법에 중점을 두고 AEM용 AEM Dispatcher as a Cloud Service에 대해 알아봅니다.

>[!VIDEO](https://video.tv.adobe.com/v/336962/?quality=12&learn=on)

## Dispatcher 변환기

![Dispatcher 변환기](./assets/dispatcher-converter-diagram.png)

코드 베이스를 리팩터링하는 과정의 일부로, [AEM Dispatcher 변환기](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/refactoring-tools/dispatcher-transformation-utility-tools.html) 기존 온-프레미스 또는 Adobe Managed Services 디스패처 구성을 AEM as a Cloud Service 호환 디스패처 구성으로 리팩터링합니다.

### 주요 활동

* 를 사용하십시오 [Adobe I/O Dispatcher 변환기 도구](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#aio-aem-migrationdispatcher-converter) 기존 Dispatcher 구성을 마이그레이션하려면 다음을 수행하십시오.
* 에서 Dispatcher 모듈 참조 [AEM 프로젝트 원형](https://github.com/adobe/aem-project-archetype/tree/develop/src/main/archetype/dispatcher.cloud) 좋은 방법입니다.
* [로컬 Dispatcher 도구 설정](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/dispatcher-tools.html) dispatcher의 유효성을 검사하려면 Cloud Service 환경에서 테스트하기 전에


