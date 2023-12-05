---
title: AEM Assets Microservices 및 AEMas a Cloud Service 로 이동
description: AEM Assets as a Cloud Service의 asset compute 마이크로서비스를 통해 기존 AEM Workflow의 역할을 대체하여 에셋에 대한 렌디션을 자동 및 효율적으로 생성하는 방법을 알아봅니다.
version: Cloud Service
feature: Asset Compute Microservices
topic: Migration, Upgrade
role: Developer
level: Experienced
jira: KT-8635
thumbnail: 336990.jpeg
exl-id: 327e8663-086b-4b31-b159-a0cf30480b45
duration: 1006
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '291'
ht-degree: 1%

---

# AEM Assets AEM Microservices - as a Cloud Service으로 이동

AEM Assets as a Cloud Service의 asset compute 마이크로서비스를 통해 기존 AEM Workflow의 역할을 대체하여 에셋에 대한 렌디션을 자동 및 효율적으로 생성하는 방법을 알아봅니다.

>[!VIDEO](https://video.tv.adobe.com/v/336990?quality=12&learn=on)

## 워크플로우 마이그레이션 도구

![자산 워크플로우 마이그레이션 도구](./assets/asset-workflow-migration.png)

코드 베이스를 리팩터링하는 과정의 일부로 [자산 워크플로우 마이그레이션 도구](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/refactoring-tools/asset-workflow-migration-tool.html) AEMas a Cloud Service 의 Asset compute 마이크로서비스 를 사용하도록 기존 워크플로우를 마이그레이션합니다.

## 주요 활동

+ 사용 [Adobe I/O Workflow Migrator](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#command-aio-aem-migrationworkflow-migrator) asset compute 마이크로서비스를 사용하도록 에셋 처리 워크플로를 마이그레이션하는 도구입니다.
+ 설정 [로컬 개발 환경](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=ko-KR) 업데이트된 워크플로를 배포할 수 있습니다. 복잡한 워크플로의 경우 수동으로 조정해야 할 수 있습니다.
+ 업데이트된 워크플로우가 기능 패리티와 일치할 때까지 AEM SDK를 사용하여 로컬 개발 환경에서 계속 반복합니다.
+ 업데이트된 코드 베이스를 AEM as a Cloud Service 개발 환경에 배포하고 계속 유효성을 검사합니다.

## 실습 위주의 운동

이 실습으로 배운 것을 시도함으로써 지식을 적용하세요.

실습형 운동을 시도하기 전에 위의 비디오와 다음 자료를 시청하고 이해했는지 확인하십시오.

+ [AEM as a Cloud Service에 대해 다르게 생각](./introduction.md)
+ [온보딩](./onboarding.md)

또한, 이전에 실습한 연습을 완료했는지 확인하십시오.

+ [실습 찾기 및 색인화](./search-and-indexing.md#hands-on-exercise)

<table style="border-width:0">
    <tr>
        <td style="width:150px">
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session8-assets#cloud-acceleration-bootcamp---session-8-assets-and-microservices"><img alt="실습 GitHub 리포지토리" src="./assets/github.png"/>
            </a>        
        </td>
        <td style="width:100%;margin-bottom:1rem;">
            <div style="font-size:1.25rem;font-weight:400;">자산 업로드 실습</div>
            <p style="margin:1rem 0">
                'aem-upload' npm CLI 모듈을 사용하여 AEM Assets 처리 프로필을 정의하고 폴더에 할당하고 자산을 AEM에 업로드하는 방법을 알아봅니다.
            </p>
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session8-assets#cloud-acceleration-bootcamp---session-8-assets-and-microservices" class="spectrum-Button spectrum-Button--primary spectrum-Button--sizeM">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">자산 관리 시도</span>
            </a>
        </td>
    </tr>
</table>
