---
title: AEM as a Cloud Service으로 검색 및 색인 지정
description: AEM as a Cloud Service의 검색 인덱스, AEM 6 인덱스 정의를 변환하는 방법 및 인덱스를 배포하는 방법에 대해 알아봅니다.
version: Cloud Service
feature: Search
topic: Migration, Upgrade
role: Developer
level: Experienced
jira: KT-8634
thumbnail: 336963.jpeg
exl-id: f752df86-27d4-4dbf-a3cb-ee97b7d9a17e
duration: 1231
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '297'
ht-degree: 0%

---

# 검색 및 색인 지정

AEM as a Cloud Service의 검색 인덱스, AEM 6 인덱스 정의를 AEMas a Cloud Service 과 호환되도록 변환하는 방법, 인덱스를 AEMas a Cloud Service 에 배포하는 방법에 대해 알아봅니다.

>[!VIDEO](https://video.tv.adobe.com/v/336963?quality=12&learn=on)

## 인덱스 변환기 도구

![인덱스 변환기 도구](./assets/index-converter.png)

코드 베이스를 리팩터링하는 과정의 일부로 [인덱스 변환기 도구](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#command-aio-aem-migrationindex-converter) AEM 를 클릭하여 사용자 지정 Oak 색인 정의를 as a Cloud Service으로 호환되는 색인 정의로 변환합니다.

리뷰 [인덱스 변환기 설명서](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/refactoring-tools/index-converter.html) 전체 및 현재 Index Converter 기능 세트용

## 주요 활동

+ 사용 [Adobe I/O Workflow Migrator](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#command-aio-aem-migrationindex-converter) asset compute 마이크로서비스를 사용하도록 에셋 처리 워크플로를 마이그레이션하는 도구입니다.
+ 설정 [로컬 개발 환경](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=ko-KR) 맞춤화된 인덱스를 배포할 수 있습니다. 업데이트된 인덱스가 최신 상태인지 확인하십시오.
+ 업데이트된 코드 베이스를 AEM as a Cloud Service 개발 환경에 배포하고 계속 유효성을 검사합니다.
+ 기본 제공 색인을 수정하는 경우 **항상** 최신 릴리스에서 실행 중인 AEM as a Cloud Service 환경에서 최신 인덱스 정의를 복사합니다. 복사한 색인 정의를 필요에 맞게 수정합니다.

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
                Oak 색인 정의 및 AEM에 as a Cloud Service 배포를 살펴봅니다.
            </p>
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session7-indexes#cloud-acceleration-bootcamp---session-7-search-and-indexing" class="spectrum-Button spectrum-Button--primary spectrum-Button--sizeM">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">색인화 시도</span>
            </a>
        </td>
    </tr>
</table>
