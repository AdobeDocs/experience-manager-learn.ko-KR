---
title: DoS/DDoS 방지 이해
description: AEM에 대한 DoS 및 DDoS 공격을 방지하고 완화하는 방법을 알아봅니다.
version: Experience Manager 6.5, Experience Manager as a Cloud Service
feature: Security
topic: Security, Development
role: Admin, Architect, Developer
level: Beginner
doc-type: Article
duration: 75
last-substantial-update: 2024-03-30T00:00:00Z
jira: KT-15219
exl-id: 1d7dd829-e235-4884-a13f-b6ea8f6b4b0b
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '370'
ht-degree: 100%

---

# AEM의 DoS/DDoS 방지 이해

AEM 환경에서 DoS 및 DDoS 공격을 방지하고 완화하는 데 사용할 수 있는 옵션에 대해 알아봅니다. 방지 메커니즘을 살펴보기에 앞서, [DoS](https://developer.mozilla.org/en-US/docs/Glossary/DOS_attack)와 [DDoS](https://developer.mozilla.org/en-US/docs/Glossary/Distributed_Denial_of_Service)에 대한 간략한 개요를 살펴보겠습니다.

- DoS(서비스 거부) 및 DDoS(분산 서비스 거부) 공격은 모두 특정 서버, 서비스 또는 네트워크의 정상적인 작동을 방해하여 의도된 사용자가 액세스하지 못하도록 만드는 악의적인 시도입니다.
- DoS 공격은 일반적으로 단일 소스에서 시작되지만 DDoS 공격은 여러 소스에서 시작됩니다.
- DDoS 공격은 여러 공격 디바이스의 리소스를 결합하기 때문에 DoS 공격에 비해 규모가 더 큰 경우가 많습니다.
- 이러한 공격은 대상에게 과도한 트래픽을 공급하고 네트워크 프로토콜의 취약점을 악용하여 수행됩니다.

다음 표에서는 DoS 및 DDoS 공격을 방지하고 완화하는 방법을 설명합니다.

<table>
    <tbody>
        <tr>
            <td><strong>방지 메커니즘</strong></td>
            <td><strong>설명</strong></td>
            <td><strong>AEM as a Cloud Service</strong></td>
            <td><strong>AEM 6.5(AMS)</strong></td>
            <td><strong>AEM 6.5 (온프레미스)</strong></td>
        </tr>
        <tr>
            <td>웹 애플리케이션 방화벽 (WAF)</td>
            <td>다양한 유형의 공격으로부터 웹 애플리케이션을 보호하도록 설계된 보안 솔루션입니다.</td>
            <td>
            <a href="https://experienceleague.adobe.com/ko/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/examples-and-analysis#waf-rules" target="_blank">WAF-DDoS 보호 라이선스</a></td>
            <td>AMS 계약을 통한 <a href="https://docs.aws.amazon.com/waf/" target="_blank">AWS</a> 또는 <a href="https://azure.microsoft.com/en-us/products/web-application-firewall" target="_blank">Azure</a> WAF.</td>
            <td>선호하는 WAF</td>
        </tr>
        <tr>
            <td>ModSecurity</td>
            <td>ModSecurity(또는 `mod_security` Apache 모듈)는 웹 애플리케이션에 대한 다양한 공격으로부터 보호하는 오픈 소스 크로스 플랫폼 솔루션입니다.<br/> AEM as a Cloud Service에서는 AEM 작성자 서비스 앞에 Apache 웹 서버와 AEM Dispatcher가 없기 때문에 AEM 게시 서비스에만 적용됩니다.</td>
            <td colspan="3"><a href="https://experienceleague.adobe.com/ko/docs/experience-manager-learn/foundation/security/modsecurity-crs-dos-attack-protection" target="_blank">ModSecurity 활성화 </a></td>
        </tr>
        <tr>
            <td>트래픽 필터 규칙</td>
            <td>트래픽 필터 규칙을 사용하여 CDN 계층에서 요청을 차단하거나 허용할 수 있습니다.</td>
            <td><a href="https://experienceleague.adobe.com/ko/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/examples-and-analysis" target="_blank">트래픽 필터 규칙 예시</a></td>
            <td><a href="https://docs.aws.amazon.com/waf/latest/developerguide/waf-rule-statement-type-rate-based.html" target="_blank">AWS</a> 또는 <a href="https://learn.microsoft.com/en-us/azure/web-application-firewall/ag/rate-limiting-overview" target="_blank">Azure</a> 규칙 제한 기능.</td>
            <td>선호하는 솔루션</td>
        </tr>
    </tbody>
</table>

## 사고 후 분석 및 지속적 개선

DoS/DDoS 공격을 식별하고 방지하는 데 있어 표준화된 단일 흐름은 없으며, 조직의 보안 프로세스에 따라 다릅니다. 그러나 **사고 후 분석 및 지속적 개선**&#x200B;은 매우 중요한 단계입니다. 고려해야 할 몇 가지 모범 사례는 다음과 같습니다.

- 로그, 네트워크 트래픽 및 시스템 구성을 검토하는 등 사고 발생 후 분석을 수행하여 DoS/DDoS 공격의 근본 원인을 파악합니다.
- 사고 후 분석 결과를 토대로 방지 메커니즘을 개선합니다.

