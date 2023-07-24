---
source-git-commit: ed53392381fa568de8230288e6b85c87540222cf
workflow-type: tm+mt
source-wordcount: '266'
ht-degree: 4%

---


# Analytics와 Experience Manager 통합

{{analytics-description}}

{{experience-manager-description}}

Adobe Analytics과 Adobe Experience Manager을 통합하면 다음과 같은 몇 가지 이점이 있습니다.

+ **정확한 세분화**: 캠페인의 개인화된 대상 세그먼트에 대한 Adobe Analytics 및 Audience Manager 병합.
+ **포괄적인 고객** 프로필: 데이터 소스를 통합하여 상호 작용 및 동작을 통합적으로 이해할 수 있습니다.
+ **최적화된 광고 타깃팅**: Adobe Analytics 및 Audience Manager의 데이터 기반 타깃팅을 통해 광고 효과를 향상시킵니다.
+ **정보에 입각한 결정**: 더 나은 선택을 위해 병합된 Adobe Analytics 및 Audience Manager 데이터의 자세한 통찰력을 제공합니다.
+ **개인화된 경험**: 두 플랫폼의 기능을 모두 활용하여 터치포인트 간에 개인화된 콘텐츠 및 오퍼를 제공합니다.

## 일반적인 통합

<table>
    <thead>
        <tr>
            <th>Experience Cloud 애플리케이션</th>
            <th>를 사용하여 통합</th>
            <th>사용 시기</th>
            <th>일반적인 사용 사례</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><a href="https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform/analytics-using-web-sdk.html" target="_blank" rel="noreferrer">Analytics와 AEM Sites</a></td>
            <td>Experience Platform Web SDK 태그 확장 또는 alloy.js</td>
            <td>
                <ul>
                    <li>Adobe Analytics에서 AEM 웹 분석 데이터에 대해 보고하고 향후 다른 Experience Cloud 애플리케이션과 통합할 수 있습니다.</li>
                </ul>
            </td>
            <td>
                <ul>
                  <li>웹 사이트 트래픽 추적.</li>
                  <li>마케팅 캠페인 모니터링</li>
                  <li>웹 사이트 성능 최적화.</li>
                </ul>
            </td>
        </tr>
        <tr>
            <td><a href="https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/analytics/collect-data-analytics.html" target="_blank" rel="noreferrer">Analytics와 AEM Sites</a></td>
            <td>Adobe Analytics 태그 확장 또는 AppMeasurement.js</td>
            <td>
                <ul>
                    <li>Adobe Analytics에서 웹 분석 데이터만 필요한 경우.</li>
                    <li>추적 가능한 웹 사이트 요소에 AEM 핵심 구성 요소를 사용하는 경우.</li>
                    <li>최소 구성 및 구현이 필요한 경우.</li>
                </ul>
            </td>
            <td>
                <ul>
                  <li>웹 사이트 트래픽 추적.</li>
                  <li>마케팅 캠페인 모니터링</li>
                  <li>웹 사이트 성능 최적화.</li>
                </ul>
            </td>
        </tr>
        <tr>
            <td><a href="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/forms/forms-and-analytics/introduction.html" target="_blank" rel="noreferrer">Analytics 및 AEM Forms as Cloud Service</a></td>
            <td>Experience Platform Web SDK 태그 확장 또는 alloy.js</td>
            <td>
              <ul>
                <li>Adobe Analytics에서 디지털 양식 분석 데이터를 보고자 할 때 및 향후 다른 Experience Cloud 애플리케이션과 통합할 수 있습니다.</li>
              </ul>
            </td>
            <td>
                <ul>
                  <li>양식 제출을 추적합니다.</li>
                  <li>양식 필드 오류 모니터링</li>
                  <li>제출된 양식 필드 값에 대해 보고합니다.</li>
                </ul>
            </td>
        </tr>
    </tbody>          
</table>
