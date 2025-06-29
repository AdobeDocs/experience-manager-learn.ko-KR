---
title: BPA 및 CAM 프로젝트 설정
description: Best Practices Analyzer 및 Cloud Acceleration Manager이 AEM as a Cloud Service으로 마이그레이션하기 위한 맞춤형 안내서를 제공하는 방법에 대해 알아봅니다.
version: Experience Manager as a Cloud Service
feature: Developer Tools
topic: Migration, Upgrade
role: Developer
level: Experienced
jira: KT-8627
thumbnail: 336957.jpeg
exl-id: f8289dd4-b293-4b8f-b14d-daec091728c0
duration: 680
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '350'
ht-degree: 2%

---

# Best Practices Analyzer 및 Cloud Acceleration Manager

Best Practices Analyzer(BPA) 및 Cloud Acceleration Manager(CAM)가 어떻게 AEM as a Cloud Service으로 마이그레이션하기 위한 맞춤형 안내서를 제공하는지에 대해 알아봅니다. 

>[!VIDEO](https://video.tv.adobe.com/v/3453355?quality=12&learn=on&captions=kor)

## BPA 및 CAM 사용

![BPA 및 CAM 높은 수준 다이어그램](assets/bpa-cam-diagram.png)

BPA 패키지는 프로덕션 AEM 6.x 환경의 클론에 설치해야 합니다. BPA는 CAM에 업로드할 수 있는 보고서를 생성하므로 AEM as a Cloud Service으로 이동하기 위해 수행해야 하는 주요 활동에 대한 지침을 제공합니다.

## 주요 활동

+ 프로덕션 6.x 환경의 클론을 만듭니다. 콘텐츠를 마이그레이션하고 코드를 리팩터링할 때 프로덕션 환경의 클론을 만들면 다양한 도구와 변경 사항을 테스트할 수 있습니다.
+ [소프트웨어 배포 포털](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html)에서 최신 BPA 도구를 다운로드하고 AEM 6.x 복제 환경에 설치합니다.
+ BPA 도구를 사용하여 CAM(Cloud Acceleration Manager)에 업로드할 수 있는 보고서를 생성합니다. CAM은 [https://experience.adobe.com/](https://experience.adobe.com/) > **Experience Manager** > **Cloud Acceleration Manager**&#x200B;을 통해 액세스합니다.
+ CAM을 사용하여 AEM as a Cloud Service으로 이동하기 위해 현재 코드 베이스 및 환경에 대해 업데이트해야 하는 사항에 대한 지침을 제공합니다.

## 실습 위주의 운동

이 실습으로 배운 것을 시도함으로써 지식을 적용하세요.

실습형 운동을 시도하기 전에 위의 비디오와 다음 자료를 시청하고 이해했는지 확인하십시오.

+ [AEM as a Cloud Service에 대해 다르게 생각](./introduction.md)
+ [AEM as a Cloud Service이란?](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/introduction/what-is-aem-as-a-cloud-service.html?lang=ko)
+ [AEM as a Cloud Service 아키텍처](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/introduction/architecture.html?lang=ko)
+ [변경 가능한 콘텐츠 및 변경 불가능한 콘텐츠](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/basics/mutable-immutable.html?lang=ko)
+ [AEM as a Cloud Service 및 AEM 6.x용 개발 차이점](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/development-guidelines.html?lang=ko#developing)

<table style="border-width:0">
    <tr>
        <td style="width:150px">
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session1-differently#bootcamp---session-1-introduction-and-thinking-differently"><img alt="실습 GitHub 리포지토리" src="./assets/github.png"/>
            </a>        
        </td>
        <td style="width:100%;margin-bottom:1rem;">
            <div style="font-size:1.25rem;font-weight:400;">Best Practices Analyzer 활용</div>
            <p style="margin:1rem 0">
                모범 사례 분석기(BPA)를 탐색하고 위반 사례가 포함된 이전 WKND 코드 베이스에 대해 실행하여 결과를 검토하십시오.
            </p>
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session1-differently#bootcamp---session-1-introduction-and-thinking-differently" class="spectrum-Button spectrum-Button--primary spectrum-Button--sizeM">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">모범 사례 분석기 사용</span>
            </a>
        </td>
    </tr>
</table>


## 기타 리소스

+ [모범 사례 분석기 다운로드](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=Best*+Practices*+Analyzer*&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=1)