---
title: AEM as a Cloud Service으로 이동하기 위해 AEM 현대화 도구 사용
description: AEM 현대화 도구 를 사용하여 기존 AEM 프로젝트 및 콘텐츠를 AEM as a Cloud Service과 호환되도록 업그레이드하는 방법에 대해 알아봅니다.
version: Cloud Service
topic: Migration, Upgrade
feature: Migration
role: Developer
level: Experienced
jira: KT-8629
thumbnail: 336965.jpeg
exl-id: 310f492c-0095-4015-81a4-27d76f288138
duration: 2502
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '385'
ht-degree: 1%

---


# AEM 현대화 도구

AEM 현대화 도구 를 사용하여 기존 AEM Sites 콘텐츠를 AEM as a Cloud Service과 호환되고 모범 사례에 맞게 업그레이드하는 방법에 대해 알아봅니다.

## 올인원 컨버터

>[!VIDEO](https://video.tv.adobe.com/v/338802?quality=12&learn=on)

## 페이지 전환

>[!VIDEO](https://video.tv.adobe.com/v/338799?quality=12&learn=on)

## 구성 요소 전환

>[!VIDEO](https://video.tv.adobe.com/v/338788?quality=12&learn=on)

## 정책 가져오기

>[!VIDEO](https://video.tv.adobe.com/v/338797?quality=12&learn=on)

## AEM 현대화 도구 사용

![AEM 현대화 도구 수명 주기](./assets/aem-modernization-tools.png)

AEM 현대화 도구는 레거시 정적 템플릿, 기초 구성 요소 및 parsys로 구성된 기존 AEM 페이지를 편집 가능한 템플릿, AEM 코어 WCM 구성 요소 및 레이아웃 컨테이너와 같은 최신 접근 방식을 사용하도록 자동으로 변환합니다.

## 주요 활동

+ AEM 6.x 프로덕션을 복제하여 AEM 현대화 도구 실행
+ 패키지 관리자를 통해 AEM 6.x 프로덕션 클론에 [최신 AEM 현대화 도구](https://github.com/adobe/aem-modernize-tools/releases/latest)를 다운로드하여 설치하십시오.

+ [페이지 구조 변환기](https://opensource.adobe.com/aem-modernize-tools/pages/structure/about.html)는 레이아웃 컨테이너를 사용하여 정적 템플릿의 기존 페이지 콘텐츠를 매핑된 편집 가능한 템플릿으로 업데이트합니다.
   + OSGi 구성을 사용하여 전환 규칙 정의
   + 기존 페이지에 대해 페이지 구조 변환기 실행

+ [구성 요소 변환기](https://opensource.adobe.com/aem-modernize-tools/pages/component/about.html)은(는) 레이아웃 컨테이너를 사용하여 정적 템플릿의 기존 페이지 콘텐츠를 매핑된 편집 가능한 템플릿으로 업데이트합니다.
   + JCR 노드 정의/XML을 통해 전환 규칙 정의
   + 기존 페이지에 대해 구성 요소 변환기 도구 실행

+ [정책 가져오기](https://opensource.adobe.com/aem-modernize-tools/pages/policy/about.html)가 디자인 구성에서 정책을 만듭니다.
   + JCR 노드 정의/XML을 사용하여 변환 규칙 정의
   + 기존 디자인 정의에 대해 정책 가져오기 실행
   + 가져온 정책을 AEM 구성 요소 및 컨테이너에 적용

## 실습 위주의 운동

이 실습으로 배운 것을 시도함으로써 지식을 적용하세요.

실습형 운동을 시도하기 전에 위의 비디오와 다음 자료를 시청하고 이해했는지 확인하십시오.

+ [AEM as a Cloud Service에 대해 다르게 생각](./introduction.md)
+ [저장소 현대화](./repository-modernization.md)
+ [변경 가능한 콘텐츠 및 변경 불가능한 콘텐츠](../../developing/basics/mutable-immutable.md)
+ [AEM 프로젝트 구조](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/aem-project-content-package-structure.html)

또한, 이전에 실습한 연습을 완료했는지 확인하십시오.

+ [BPA 및 CAM 실습 운동](./bpa-and-cam.md#hands-on-exercise)

<table style="border-width:0">
    <tr>
        <td style="width:150px">
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session2-migration#bootcamp---session-2-migration-methodology"><img alt="실습 GitHub 리포지토리" src="./assets/github.png"/>
            </a>        
        </td>
        <td style="width:100%;margin-bottom:1rem;">
            <div style="font-size:1.25rem;font-weight:400;">AEM 현대화 실습</div>
            <p style="margin:1rem 0">
                AEM 현대화 도구 를 사용하여 탐색하고 AEM as a Cloud Service 모범 사례를 준수하도록 레거시 WKND 사이트를 업데이트합니다.
            </p>
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session2-migration#bootcamp---session-2-migration-methodology" class="spectrum-Button spectrum-Button--primary spectrum-Button--sizeM">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">AEM 현대화 도구 사용</span>
            </a>
        </td>
    </tr>
</table>

## 기타 리소스

+ [AEM 현대화 도구 다운로드](https://github.com/adobe/aem-modernize-tools/releases/latest)
+ [AEM 현대화 도구 설명서](https://opensource.adobe.com/aem-modernize-tools/)
+ [AEM Gems - AEM 현대화 세트 소개](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/Introducing-the-AEM-Modernization-Suite.html)

1. 로컬 AEM SDK에 새로 현대화한 wknd 레거시 사이트를 배포합니다. AEM ASK는 여기에서 다운로드할 수 있습니다.
   + [소프트웨어 배포 포털](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html).
