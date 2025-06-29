---
title: AEM as a Cloud Service에서 검색 및 인덱싱
description: AEM as a Cloud Service의 검색 인덱스, AEM 6 인덱스 정의를 변환하는 방법 및 인덱스를 배포하는 방법에 대해 알아봅니다.
version: Experience Manager as a Cloud Service
feature: Search
topic: Migration, Upgrade
role: Developer
level: Experienced
jira: KT-8634
thumbnail: 336963.jpeg
exl-id: f752df86-27d4-4dbf-a3cb-ee97b7d9a17e
duration: 1231
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '297'
ht-degree: 0%

---

# 검색 및 색인 지정

AEM as a Cloud Service의 검색 인덱스, AEM 6 인덱스 정의를 AEM as a Cloud Service과 호환되도록 변환하는 방법 및 인덱스를 AEM as a Cloud Service에 배포하는 방법에 대해 알아봅니다.

>[!VIDEO](https://video.tv.adobe.com/v/3454721?quality=12&learn=on&captions=kor)

## 인덱스 변환기 도구

![인덱스 변환기 도구](./assets/index-converter.png)

코드 베이스를 리팩터링하는 과정의 일부로 [인덱스 변환기 도구](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#command-aio-aem-migrationindex-converter)를 사용하여 사용자 지정 Oak 인덱스 정의를 AEM as a Cloud Service 호환 인덱스 정의로 변환합니다.

[인덱스 변환기 설명서](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/refactoring-tools/index-converter.html?lang=ko)에서 전체 및 현재 인덱스 변환기 기능 집합을 검토하십시오.

## 주요 활동

+ [Adobe I/O Workflow Migrator](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#command-aio-aem-migrationindex-converter) 도구를 사용하여 Asset Compute 마이크로서비스를 사용하도록 에셋 처리 워크플로를 마이그레이션하십시오.
+ [로컬 개발 환경](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=ko)을 설정하고 사용자 지정된 인덱스를 배포합니다. 업데이트된 인덱스가 최신 상태인지 확인하십시오.
+ 업데이트된 코드 베이스를 AEM as a Cloud Service 개발 환경에 배포하고 계속 확인합니다.
+ 기본 제공 인덱스 **ALWAYS**&#x200B;을(를) 수정하는 경우 최신 릴리스에서 실행 중인 AEM as a Cloud Service 환경에서 최신 인덱스 정의를 복사하십시오. 복사한 색인 정의를 필요에 맞게 수정합니다.

## 실습 위주의 운동

이 실습으로 배운 것을 시도함으로써 지식을 적용하세요.

실습형 운동을 시도하기 전에 위의 비디오와 다음 자료를 시청하고 이해했는지 확인하십시오.

+ [AEM as a Cloud Service에 대해 다르게 생각](./introduction.md)
+ [저장소 현대화](./repository-modernization.md)

또한, 이전에 실습한 연습을 완료했는지 확인하십시오.

+ [콘텐츠 전송 도구 실습](./content-migration/content-transfer-tool.md#hands-on-exercise)

<table style="border-width:0">
    <tr>
        <td style="width:150px">
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session7-indexes#cloud-acceleration-bootcamp---session-7-search-and-indexing"><img alt="실습 GitHub 리포지토리" src="./assets/github.png"/>
            </a>        
        </td>
        <td style="width:100%;margin-bottom:1rem;">
            <div style="font-size:1.25rem;font-weight:400;">색인을 사용하여 실습</div>
            <p style="margin:1rem 0">
                Oak 인덱스 정의 및 AEM as a Cloud Service 배포를 살펴봅니다.
            </p>
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session7-indexes#cloud-acceleration-bootcamp---session-7-search-and-indexing" class="spectrum-Button spectrum-Button--primary spectrum-Button--sizeM">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">색인화 시도</span>
            </a>
        </td>
    </tr>
</table>
