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
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '309'
ht-degree: 2%

---

# 검색 및 색인 지정

AEM as a Cloud Service의 검색 인덱스, AEM 6 인덱스 정의를 AEM과 호환되도록 변환하는 방법, 그리고 인덱스를 AEM에 배포하는 방법에 대해.

>[!VIDEO](https://video.tv.adobe.com/v/336963?quality=12&learn=on)

## Index Converter 도구

![Index Converter 도구](./assets/index-converter.png)

코드 베이스를 리팩터링하는 과정의 일부로, [인덱스 변환기 도구](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#command-aio-aem-migrationindex-converter) 사용자 지정 Oak 색인 정의를 AEM as a Cloud Service 호환 인덱스 정의로 변환하려면 다음을 수행하십시오.

## 주요 활동

+ 를 사용하십시오 [Adobe I/O Workflow Migrator](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#command-aio-aem-migrationindex-converter) asset compute 마이크로서비스를 사용하도록 자산 처리 워크플로우를 마이그레이션하는 도구입니다.
+ 설정 [로컬 개발 환경](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=ko-KR) 사용자 지정된 인덱스를 배포합니다. 업데이트된 인덱스가 최신 상태인지 확인합니다.
+ 업데이트된 코드 베이스를 AEM as a Cloud Service 개발 환경에 배포하고 계속 유효성을 검사합니다.
+ 기본 색인을 수정하는 경우 **항상** 최신 릴리스에서 실행되는 AEM as a Cloud Service 환경에서 최신 인덱스 정의를 복사합니다. 필요에 맞게 복사된 인덱스 정의를 수정합니다.

## 실습 운동

이 실습 운동으로 배운 것을 시도하여 여러분의 지식을 적용하세요.

실습 테스트를 하기 전에 위의 비디오와 다음 자료를 보고 이해했는지 확인하십시오.

+ [AEM as a Cloud Service에 대해 다르게 생각하다](./introduction.md)
+ [저장소 현대화](./repository-modernization.md)

또한 이전의 실습 운동을 완료했는지 확인하십시오.

+ [컨텐츠 전송 도구 실습 연습](./content-migration/content-transfer-tool.md#hands-on-exercise)

<table style="border-width:0">
    <tr>
        <td style="width:150px">
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session7-indexes#cloud-acceleration-bootcamp---session-7-search-and-indexing"><img alt="실습 GitHub 리포지토리" src="./assets/github.png"/>
            </a>        
        </td>
        <td style="width:100%;margin-bottom:1rem;">
            <div style="font-size:1.25rem;font-weight:400;">인덱스 사용</div>
            <p style="margin:1rem 0">
                AEM as a Cloud Service에 Oak 인덱스 정의 및 배포를 탐색합니다.
            </p>
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session7-indexes#cloud-acceleration-bootcamp---session-7-search-and-indexing" class="spectrum-Button spectrum-Button--primary spectrum-Button--sizeM">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">색인 지정 시도</span>
            </a>
        </td>
    </tr>
</table>
