---
title: 컨텐츠 전송 도구를 사용한 컨텐츠 마이그레이션
description: 컨텐츠 전송 도구 가 AEM 6에서 AEM as a Cloud Service으로 컨텐츠를 마이그레이션하는 방법에 대해 알아봅니다.
version: Experience Manager as a Cloud Service
topic: Migration, Upgrade
feature: Migration
role: Developer
level: Experienced
jira: KT-8919
thumbnail: 336970.jpeg
exl-id: c51ce8e3-e83c-4f8b-a835-70335ed3a5b9
duration: 1362
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '295'
ht-degree: 3%

---


# 콘텐츠 전송 도구

컨텐츠 전송 도구 가 AEM 6.3+에서 AEM as a Cloud Service으로 컨텐츠를 마이그레이션하는 방법에 대해 알아봅니다.

>[!VIDEO](https://video.tv.adobe.com/v/336970?quality=12&learn=on)

## 컨텐츠 전송 도구 사용

![컨텐츠 전송 도구 수명 주기](../assets/content-transfer-tool.png)

컨텐츠 전송 도구는 AEM 6.3+에 설치되며 컨텐츠를 AEM as a Cloud Service으로 전송합니다.

## 주요 활동

+ [최신 콘텐츠 전송 도구](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=Content*+Transfer*+Tool*&amp;1_group.propertyvalues.property=입니다.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atoling&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=2)를 다운로드합니다.
+ AEM Author 6.3 이상의 최종 콘텐츠를 AEM as a Cloud Service Author 서비스로 전송합니다.
   + 전송할 최종 콘텐츠가 포함된 AEM 6.3 이상 작성자에 콘텐츠 전송 도구를 설치합니다.
   + 컨텐츠 전송 도구를 일괄로 실행하여 컨텐츠 세트를 전송합니다.
+ AEM Publish 6.3 이상의 최종 콘텐츠를 AEM as a Cloud Service Publish 서비스로 전송합니다.
   + 전송할 최종 콘텐츠가 포함된 AEM 6.3+ 게시에서 콘텐츠 전송 도구를 설치합니다.
   + 컨텐츠 전송 도구를 일괄로 실행하여 컨텐츠 세트를 전송합니다.
+ 원할 경우, 마지막 컨텐츠 전송 이후 새 컨텐츠를 전송하여 AEM as a Cloud Service에서 &quot;추가&quot; 컨텐츠

## 실습 위주의 운동

이 실습으로 배운 것을 시도함으로써 지식을 적용하세요.

실습형 운동을 시도하기 전에 위의 비디오와 다음 자료를 시청하고 이해했는지 확인하십시오.

+ [AEM 현대화 도구](../aem-modernization-tools.md)
+ [온보딩](../onboarding.md)
+ [Cloud Manager](../cloud-manager.md)

또한, 이전에 실습한 연습을 완료했는지 확인하십시오.

+ [Dispatcher 실습](../dispatcher.md#hands-on-exercise)

<table style="border-width:0">
    <tr>
        <td style="width:150px">
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session6-transfercontent#cloud-acceleration-bootcamp---session-6-content"><img alt="실습 GitHub 리포지토리" src="../assets/github.png"/>
            </a>        
        </td>
        <td style="width:100%;margin-bottom:1rem;">
            <div style="font-size:1.25rem;font-weight:400;">컨텐츠 전송 도구를 사용하여 실습</div>
            <p style="margin:1rem 0">
                컨텐츠 전송 도구 가 컨텐츠를 AEM 6에서 AEM as a Cloud Service으로 자동으로 이동하는 방법을 탐색합니다.
            </p>
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session6-transfercontent#cloud-acceleration-bootcamp---session-6-content" class="spectrum-Button spectrum-Button--primary spectrum-Button--sizeM">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">콘텐츠 전송 도구 사용</span>
            </a>
        </td>
    </tr>
</table>

## 기타 리소스

+ [콘텐츠 전송 도구 다운로드](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=Content*+Transfer*+Tool*&amp;1_group.propertyvalues.property=입니다.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atoling&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=2)
+ [일괄 가져오기 서비스 방법 비디오](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/migration/bulk-import.html)

