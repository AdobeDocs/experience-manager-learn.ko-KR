---
title: AEM as a Cloud Service으로 이동할 때 Dispatcher 구성
description: AEM as a Cloud Service용 AEM Dispatcher, Dispatcher 변환 도구 및 Dispatcher 도구 SDK 사용 방법에 대한 주목할 만한 변경 사항에 대해 알아봅니다.
version: Experience Manager as a Cloud Service
feature: Dispatcher
topic: Migration, Upgrade
role: Developer
level: Experienced
jira: KT-8633
thumbnail: 336962.jpeg
exl-id: 81397b21-b4f3-4024-a6da-a9b681453eff
duration: 1618
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '237'
ht-degree: 4%

---


# Dispatcher

AEM 6용 AEM의 주목할 만한 변경 사항, Dispatcher 변환 도구 및 Dispatcher 도구 SDK 사용 방법에 초점을 맞춰 AEM as a Cloud Service용 Dispatcher Dispatcher에 대해 알아봅니다.

>[!VIDEO](https://video.tv.adobe.com/v/3455403?quality=12&learn=on&captions=kor)

## Dispatcher 변환기

![Dispatcher 변환기](./assets/dispatcher-converter-diagram.png)

코드 베이스를 리팩터링하는 과정의 일부로, [AEM Dispatcher 변환기](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/refactoring-tools/dispatcher-transformation-utility-tools.html?lang=ko)를 사용하여 기존 온-프레미스 또는 Adobe Managed Services Dispatcher 구성을 AEM as a Cloud Service 호환 Dispatcher 구성으로 리팩터링하십시오.

## 주요 활동

+ 기존 Dispatcher 구성을 마이그레이션하려면 [Adobe I/O Dispatcher 변환기 도구](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#aio-aem-migrationdispatcher-converter)를 사용하십시오.
+ 모범 사례로 [AEM Project Archetype](https://github.com/adobe/aem-project-archetype/tree/develop/src/main/archetype/dispatcher.cloud)의 Dispatcher 모듈을 참조하십시오.
+ Cloud Service 환경에서 테스트하기 전에 Dispatcher의 유효성을 검사하도록 [로컬 Dispatcher 도구를 설정](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/dispatcher-tools.html?lang=ko)합니다.

## 실습 위주의 운동

이 실습으로 배운 것을 시도함으로써 지식을 적용하세요.

실습형 운동을 시도하기 전에 위의 비디오와 다음 자료를 시청하고 이해했는지 확인하십시오.

+ [AEM 현대화 도구](./aem-modernization-tools.md)
+ [온보딩](./onboarding.md)
+ [Cloud Manager](./cloud-manager.md)

또한, 이전에 실습한 연습을 완료했는지 확인하십시오.

+ [Cloud Manager 실습](./cloud-manager.md#hands-on-exercise)

<table style="border-width:0">
    <tr>
        <td style="width:150px">
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session5-dispatcher#cloud-acceleration-bootcamp---session-5-dispatcher"><img alt="실습 GitHub 리포지토리" src="./assets/github.png"/>
            </a>        
        </td>
        <td style="width:100%;margin-bottom:1rem;">
            <div style="font-size:1.25rem;font-weight:400;">Dispatcher 도구를 사용한 실습</div>
            <p style="margin:1rem 0">
                AEM SDK의 Dispatcher 도구를 사용하여 탐색하여 Dispatcher 구성을 확인하고 Docker를 사용하여 로컬로 AEM Dispatcher을 실행합니다.
            </p>
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session5-dispatcher#cloud-acceleration-bootcamp---session-5-dispatcher" class="spectrum-Button spectrum-Button--primary spectrum-Button--sizeM">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Dispatcher 도구 사용</span>
            </a>
        </td>
    </tr>
</table>
