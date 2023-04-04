---
title: BPA 및 CAM 프로젝트 설정
description: Best Practices Analyzer 및 Cloud Acceleration Manager에서 AEM as a Cloud Service 으로 마이그레이션하는 사용자 정의 가이드를 제공하는 방법을 알아봅니다.
version: Cloud Service
feature: Developer Tools
topic: Migration, Upgrade
role: Developer
level: Experienced
kt: 8627
thumbnail: 336957.jpeg
exl-id: f8289dd4-b293-4b8f-b14d-daec091728c0
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '427'
ht-degree: 4%

---

# 모범 사례 분석기 및 Cloud Acceleration Manager

BPA(Best Practices Analyzer) 및 CAM(Cloud Acceleration Manager)이 AEM as a Cloud Service으로 마이그레이션하는 사용자 정의 가이드를 제공하는 방법을 알아봅니다. 

>[!VIDEO](https://video.tv.adobe.com/v/336957?quality=12&learn=on)

## BPA 및 CAM 사용

![BPA 및 CAM 상위 수준 다이어그램](assets/bpa-cam-diagram.png)

BPA 패키지는 프로덕션 AEM 6.x 환경의 클론에 설치해야 합니다. BPA는 CAM에 업로드할 수 있는 보고서를 생성합니다. 이 보고서는 AEM as a Cloud Service으로 이동하기 위해 수행해야 하는 주요 활동에 대한 지침을 제공합니다.

## 주요 활동

+ 프로덕션 6.x 환경을 복제합니다. 컨텐츠 및 리팩터링 코드를 마이그레이션함에 따라 프로덕션 환경의 복제본이 포함되는 것은 다양한 도구와 변경 사항을 테스트하는 데 중요합니다.
+ 에서 최신 BPA 도구를 다운로드합니다. [소프트웨어 배포 포털](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html) AEM 6.x 복제 환경에 설치합니다.
+ BPA 도구를 사용하여 CAM(Cloud Acceleration Manager)에 업로드할 수 있는 보고서를 생성합니다. CAM은 [https://experience.adobe.com/](https://experience.adobe.com/) > **Experience Manager** > **Cloud Acceleration Manager**.
+ AEM as a Cloud Service으로 이동하기 위해 현재 코드 베이스와 환경에 업데이트가 필요한 사항에 대한 지침을 제공하려면 CAM을 사용합니다.

## 실습 운동

이 실습 운동으로 배운 것을 시도하여 여러분의 지식을 적용하세요.

실습 테스트를 하기 전에 위의 비디오와 다음 자료를 보고 이해했는지 확인하십시오.

+ [AEM as a Cloud Service에 대해 다르게 생각하다](./introduction.md)
+ [AEM as a Cloud Service 소개](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/introduction/what-is-aem-as-a-cloud-service.html?lang=en)
+ [AEM as a Cloud Service 아키텍처](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/introduction/architecture.html?lang=en)
+ [가변 및 변경할 수 없는 컨텐츠](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/basics/mutable-immutable.html?lang=en)
+ [AEM as a Cloud Service 및 AEM 6.x를 위한 개발 차이점](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/development-guidelines.html#developing)

<table style="border-width:0">
    <tr>
        <td style="width:150px">
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session1-differently#bootcamp---session-1-introduction-and-thinking-differently"><img alt="실습 GitHub 리포지토리" src="./assets/github.png"/>
            </a>        
        </td>
        <td style="width:100%;margin-bottom:1rem;">
            <div style="font-size:1.25rem;font-weight:400;">모범 사례 분석기 사용</div>
            <p style="margin:1rem 0">
                BPA(Best Practices Analyzer)를 살펴보고 예제 위반이 포함된 레거시 WKND 코드 베이스에 대해 실행하여 결과를 검토합니다.
            </p>
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session1-differently#bootcamp---session-1-introduction-and-thinking-differently" class="spectrum-Button spectrum-Button--primary spectrum-Button--sizeM">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">모범 사례 분석기 를 사용해 보십시오</span>
            </a>
        </td>
    </tr>
</table>


## 기타 리소스

+ [모범 사례 분석기 다운로드](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=Best*+Practices*+Analyzer*&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=1)