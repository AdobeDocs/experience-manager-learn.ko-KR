---
title: Experience Platform 소스 커넥터와 Analytics 및 Real-time Customer Data Platform 통합 자습서
description: Adobe Analytics을 Real-time Customer Data Platform과 통합하는 방법을 알아봅니다.
solution: Real-Time Customer Data Platform, Analytics
feature: Integrations
topic: Integrations
role: Leader, Architect, Admin, Developer
level: Beginner
index: true
hidefromtoc: true
kt: null
thumbnail: null
last-substantial-update: 2023-04-11T00:00:00Z
badgeIntegration: label="통합" type="positive"
source-git-commit: ed53392381fa568de8230288e6b85c87540222cf
workflow-type: tm+mt
source-wordcount: '281'
ht-degree: 3%

---


# Adobe Analytics 및 Real-time Customer Data Platform과 Experience Platform 소스 커넥터 통합

<ol>
    <li><a href="https://experienceleague.adobe.com/?lang=en#dashboard/learning" _target="_blank" rel="noopener noreferrer">스키마 만들기</a> 데이터 수집에 사용됩니다.</li>
    <li><a href="https://experienceleague.adobe.com/docs/platform-learn/tutorials/data-ingestion/create-datasets-and-ingest-data.html" _target="_blank" rel="noopener noreferrer">데이터 세트 만들기</a> 데이터 수집에 사용됩니다.</a></li>
    <li><a href="https://experienceleague.adobe.com/docs/platform-learn/tutorials/identities/label-ingest-and-verify-identity-data.html?lang=en" _target="_blank" rel="noopener noreferrer">스키마에서 올바른 ID 및 ID 네임스페이스 구성</a> 수집된 데이터를 통합 프로필에 연결할 수 있도록 합니다.</li> 
    <li><a href="https://experienceleague.adobe.com/docs/platform-learn/tutorials/profiles/bring-data-into-the-real-time-customer-profile.html" _target="_blank" rel="noopener noreferrer">프로필에 대한 스키마 및 데이터 세트 활성화</a>.</li>
    <li>다음 방법 중 하나를 사용하여 데이터를 Experience Platform에 수집합니다.</li>
        <ul>
            <li><a href="https://experienceleague.adobe.com/docs/platform-learn/tutorials/sources/ingest-data-from-adobe-analytics.html" _target="_blank" rel="noopener noreferrer">Adobe Analytics 소스 커넥터</a></li>
            <li>Experience Platform Web SDK:</li>
                <ul>
                    <li><a href="https://experienceleague.adobe.com/docs/platform-learn/implement-web-sdk/overview.html?lang=ko-KR" _target="_blank" rel="noopener noreferrer">튜토리얼</a></li>
                    <li><a href="https://experienceleague.adobe.com/docs/analytics/implementation/aep-edge/web-sdk/overview.html" _target="_blank" rel="noopener noreferrer">체크리스트</a></li>
                </ul>
            <li>Experience Platform Mobile SDK:</li>
                <ul>
                    <li><a href="https://experienceleague.adobe.com/docs/platform-learn/data-collection/mobile-sdk/create-mobile-properties.html" _target="_blank" rel="noopener noreferrer">튜토리얼</a></li>
                    <li><a href="https://experienceleague.adobe.com/docs/analytics/implementation/aep-edge/mobile-sdk/overview.html" _target="_blank" rel="noopener noreferrer">체크리스트</a></li>
                </ul></li>
            <li>에지 네트워크 서버 API:</li>
                <ul>
                    <li><a href="https://experienceleague.adobe.com/docs/experience-platform/edge-network-server-api/interacting-other-adobe-solutions/interacting-adobe-analytics.html" _target="_blank" rel="noopener noreferrer">튜토리얼</a></li>
                </ul>
       </ul>
    <li><a href="https://experienceleague.adobe.com/docs/platform-learn/tutorials/segments/create-segments.html" _target="_blank" rel="noopener noreferrer">Experience Platform에서 세그먼트를 만듭니다.</a> 시스템은 세그먼트가 배치(데이터 커넥터) 또는 스트리밍(에지 네트워크)으로 평가되는지 여부를 자동으로 결정합니다.</li>
    <li><a href="https://experienceleague.adobe.com/docs/platform-learn/tutorials/destinations/create-destinations-and-activate-data.html" _target="_blank" rel="noopener noreferrer">프로필 속성 및 대상자 멤버십을 원하는 대상에 공유하기 위한 대상을 구성합니다.</a></li>   
</ol>

>[!NOTE]
>
>Adobe Analytics 소스 커넥터에 대한 표준 워크플로우 단계는 Analytics에서 데이터를 &quot;있는 그대로&quot; 수집하는 데 사용되는 스키마 및 데이터 세트를 만듭니다. 따라서 처음 두 단계는 시스템에서 처리됩니다. 매핑 워크플로를 사용하려면 사용자 지정 속성을 만들어야 하므로 단계 순서를 따르십시오.