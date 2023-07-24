---
source-git-commit: ed53392381fa568de8230288e6b85c87540222cf
workflow-type: tm+mt
source-wordcount: '280'
ht-degree: 1%

---


# Adobe Analytics과 Real-time Customer Data Platform 통합

{{analytics-description}}

{{real-time-cdp-description}}

Adobe Analytics과 Adobe Real-time Customer Data Platform(RTCDP)를 통합하면 고객 경험과 마케팅 노력을 향상시키고자 하는 비즈니스에 몇 가지 이점을 제공할 수 있습니다. 주요 이점은 다음과 같습니다.

+ **향상된 대상 타깃팅 및 개인화**: 장치 및 채널에 대한 정확한 마케팅, 최적화된 참여를 위한 맞춤 메시지.
+ **향상된 랜딩 페이지 최적화**: 장치 및 행동에 따라 맞춤 설정된 경험으로 사용자 만족도 및 전환을 향상시킵니다.
+ **원활한 대상 활성화**: 선호하는 채널을 통한 효과적인 타겟팅에 고객 프로필을 활용하여 관련 메시지를 전달할 수 있습니다.

Adobe Analytics과 Real-Time CDP을 결합함으로써 기업은 마케팅 노력을 한 차원 더 끌어올려 개인화된 경험을 제공하고, 고객 참여를 높이고, 다양한 디지털 접점에서 전환을 최적화할 수 있습니다.

<table>
    <thead>
        <tr>
            <th>Experience Cloud 애플리케이션</th>
            <th>를 사용하여 통합</th>
            <th>사용 시기</th>
            <th>일반적인 사용 사례</th>
        </tr>
    </thead>
    <tr>
        <td><a href="../../integrations/tutorials/analytics-real-time-cdp/experience-platform-source-connector.md" target="_blank" rel="noreferrer">Analytics와 Real-Time CDP</a></td>
        <td>Experience Platform 소스 커넥터</td>
        <td>
            <ul>
                <li>Analytics 데이터를 보고서 세트에서 Experience Platform으로 수집하려는 경우.</li>
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
        <td><a href="https://adobe.com" target="_blank" rel="noreferrer">TODO: Real-Time CDP이 포함된 Analytics</a></td>
        <td>Experience Platform 가장자리</td>
        <td>
            <ul>
                <li>장기 분석 전략을 구현하는 경우.</li>
                <li>동일한 페이지 및 다음 페이지 개인화 사용 사례를 지원하기 위해 고객 프로필에 Analytics 데이터 가용성이 필요한 신규 고객 또는 기존 고객인 경우.</li>
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
</table>
