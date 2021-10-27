---
title: AEM as a Cloud Service에서 검색 및 색인 지정
description: AEM as a Cloud Service의 검색 인덱스, AEM 6 인덱스 정의를 변환하는 방법 및 인덱스를 배포하는 방법에 대해 알아봅니다.
version: Cloud Service
feature: Search
topic: Migration, Upgrade
role: Developer
level: Experienced
kt: 8634
thumbnail: 336963.jpeg
exl-id: f752df86-27d4-4dbf-a3cb-ee97b7d9a17e
source-git-commit: 1ddf6154d50a341d9a0fd4234392c37ced878a73
workflow-type: tm+mt
source-wordcount: '211'
ht-degree: 3%

---

# 검색 및 색인 지정

AEM as a Cloud Service의 검색 인덱스, AEM 6 인덱스 정의를 AEM과 호환되도록 변환하는 방법, 그리고 인덱스를 AEM에 배포하는 방법에 대해.

>[!VIDEO](https://video.tv.adobe.com/v/336963/?quality=12&learn=on)

## Index Converter 도구

![Index Converter 도구](./assets/index-converter.png)

코드 베이스를 리팩터링하는 과정의 일부로, [인덱스 변환기 도구](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#command-aio-aem-migrationindex-converter) 사용자 지정 Oak 색인 정의를 AEM as a Cloud Service 호환 인덱스 정의로 변환하려면 다음을 수행하십시오.

### 주요 활동

* 를 사용하십시오 [Adobe I/O Workflow Migrator](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#command-aio-aem-migrationindex-converter) asset compute 마이크로서비스를 사용하도록 자산 처리 워크플로우를 마이그레이션하는 도구입니다.
* 설정 [로컬 개발 환경](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html) 사용자 지정된 인덱스를 배포합니다. 업데이트된 인덱스가 최신 상태인지 확인합니다.
* 업데이트된 코드 베이스를 AEM as a Cloud Service 개발 환경에 배포하고 계속 유효성을 검사합니다.
* 기본 색인을 수정하는 경우 **항상** 최신 릴리스에서 실행되는 AEM as a Cloud Service 환경에서 최신 인덱스 정의를 복사합니다. 필요에 맞게 복사된 인덱스 정의를 수정합니다.