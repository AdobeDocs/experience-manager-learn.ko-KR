---
source-git-commit: ed53392381fa568de8230288e6b85c87540222cf
workflow-type: tm+mt
source-wordcount: '248'
ht-degree: 1%

---


# Adobe Analytics과 Customer Journey Analytics 통합

{{analytics-description}}

{{customer-journey-analytics-description}}

Adobe Analytics과 Customer Journey Analytics을 통합하면 다음과 같은 주요 이점이 있습니다.

+ **포괄적인 통찰력** 를 고객 행동 및 환경 설정에 포함시킵니다.
+ **원활한 크로스 채널 추적** 전체적인 관점을 위해.
+ **통합 데이터 및 보고** 를 참조하십시오.
+ **향상된 개인화** 고객 참여도 향상.
+ **실시간 데이터 인사이트** 민첩한 의사 결정을 위해.

## 일반적인 통합

<table>
    <thead>
        <tr>
            <td>Experience Cloud 애플리케이션</td>
            <td>를 사용하여 통합</td>
            <td>사용 시기</td>
            <td>일반적인 사용 사례</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><a href="../../integrations/tutorials/analytics-customer-journey-analytics/experience-platform-source-connector.md" target="_blank" rel="noreferrer">Analytics와 Customer Journey Analytics</a></td>
            <td>Experience Platform 소스 커넥터</td>
            <td>
                <ul>
                    <li>TODO: 이 통합을 사용하여 Analytics 데이터를 보고서 세트에서 Experience Platform으로 수집할 수 있습니다.</li>
                    <li>고객 프로필에 대한 데이터 가용성이 데이터 수집 시점부터 2~30분일 수 있고 데이터 레이크에 대한 가용성은 최대 90분입니다.</li>
                </ul>
            </td>
            <td>
                <ul>
                    <li>사용자 인터페이스가 시작한 간편한 워크플로우.</li>
                    <li>사용자 인터페이스를 매핑하여 Analytics prop 및 eVar를 새 XDM 필드에 복사합니다.</li>
                    <li>실시간 고객 프로필 및 Customer Journey Analytics에서 가치를 얻는 가장 빠른 방법입니다.</li>
                </ul>
            </td>
        </tr>
        <tr>
            <td><a href="https://www.adobe.com/" target="_blank" rel="noreferrer">링크 방법: Customer Journey Analytics이 있는 Analytics</a></td>
            <td>Experience Platform 가장자리</td>
            <td>
                <ul>
                    <li>장기적인 전략을 구현하려는 경우. 이렇게 하면 AEP Web SDK, AEP Mobile SDK 또는 Edge Network Server API를 사용하여 장치에서 Experience Platform으로 직접 데이터를 전송합니다.</li>
                    <li>신규 고객 또는 기존 고객은 동일한 페이지 및 다음 페이지 개인화 사용 사례를 지원하기 위해 고객 프로필에 Analytics 데이터 가용성이 필요합니다.</li>
                </ul>
            </td>
            <td>
                <ul>
                    <li>사용 사례를 지원하기 위해 사용하기 위해 수집된 데이터에 대해 가장 높은 수준의 제어 기능을 제공합니다.</li>
                    <li>클라이언트측 데이터는 XDM 필드에 쉽게 매핑됩니다.</li>
                    <li>실시간 고객 프로필에서 데이터를 가장 빠르게 사용할 수 있습니다.</li>
                </ul>
            </td>
        </tr>  
    </tbody>          
</table>
