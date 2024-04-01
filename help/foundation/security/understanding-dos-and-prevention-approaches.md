---
title: DoS/DDoS 방지 이해
description: AEM에 대한 DoS 및 DDoS 공격을 예방하고 완화하는 방법에 대해 알아봅니다.
version: 6.5, Cloud Service
feature: Security
topic: Security, Development
role: Admin, Architect, Developer
level: Beginner
doc-type: Article
duration: 0
last-substantial-update: 2024-03-30T00:00:00Z
jira: KT-15219
source-git-commit: a504ace72b1b90c6e7c711a939595b95f24733e6
workflow-type: tm+mt
source-wordcount: '370'
ht-degree: 1%

---


# AEM의 DoS/DDoS 방지 이해

AEM 환경에서 DoS 및 DDoS 공격을 예방하고 완화하는 데 사용할 수 있는 옵션에 대해 알아봅니다. 예방 메커니즘으로 들어가기 전에 [실행](https://developer.mozilla.org/en-US/docs/Glossary/DOS_attack) 및 [DDoS](https://developer.mozilla.org/en-US/docs/Glossary/Distributed_Denial_of_Service).

- DoS(Denial of Service) 및 DDoS(Distributed Denial of Service) 공격은 대상 서버, 서비스 또는 네트워크의 정상적인 기능을 방해하는 악의적인 시도로서, 의도한 사용자가 액세스할 수 없게 합니다.
- DoS 공격은 일반적으로 단일 소스에서 발생하는 반면 DDoS 공격은 여러 소스에서 발생합니다.
- DDoS 공격은 여러 공격 장치의 결합된 자원으로 인해 DoS 공격에 비해 규모가 큰 경우가 많다.
- 이러한 공격은 과도한 트래픽으로 대상을 범람시킴으로써 수행되고, 네트워크 프로토콜의 취약점을 악용합니다.

다음 표에서는 DoS 및 DDoS 공격을 예방 및 완화하는 방법을 설명합니다.

<table>
    <tbody>
        <tr>
            <td><strong>예방 기구</strong></td>
            <td><strong>설명</strong></td>
            <td><strong>AEM as a Cloud Service</strong></td>
            <td><strong>AEM 6.5 (AMS)</strong></td>
            <td><strong>AEM 6.5(온프레미스)</strong></td>
        </tr>
        <tr>
            <td>WAF(Web Application Firewall)</td>
            <td>다양한 유형의 공격으로부터 웹 애플리케이션을 보호하기 위해 설계된 보안 솔루션입니다.</td>
            <td>
            <a href="https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/examples-and-analysis#waf-rules" target="_blank">WAF-DDoS Protection 라이센스</a></td>
            <td><a href="https://docs.aws.amazon.com/waf/" target="_blank">AWS</a> 또는 <a href="https://azure.microsoft.com/en-us/products/web-application-firewall" target="_blank">Azure</a> AMS 계약을 통한 WAF.</td>
            <td>선호하는 WAF</td>
        </tr>
        <tr>
            <td>ModSecurity</td>
            <td>ModSecurity(일명 'mod_security' Apache 모듈)는 웹 애플리케이션에 대한 다양한 공격으로부터 보호하는 오픈 소스 크로스 플랫폼 솔루션입니다.<br/> AEM as a Cloud Service에서는 AEM Author 서비스 앞에 Apache 웹 서버와 AEM Dispatcher가 없으므로 AEM Publish 서비스에만 적용할 수 있습니다.</td>
            <td colspan="3"><a href="https://experienceleague.adobe.com/en/docs/experience-manager-learn/foundation/security/modsecurity-crs-dos-attack-protection" target="_blank">ModSecurity 활성화 </a></td>
        </tr>
        <tr>
            <td>트래픽 필터 규칙</td>
            <td>트래픽 필터 규칙을 사용하여 CDN 계층에서 요청을 차단하거나 허용할 수 있습니다.</td>
            <td><a href="https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/examples-and-analysis" target="_blank">트래픽 필터 규칙 예</a></td>
            <td><a href="https://docs.aws.amazon.com/waf/latest/developerguide/waf-rule-statement-type-rate-based.html" target="_blank">AWS</a> 또는 <a href="https://learn.microsoft.com/en-us/azure/web-application-firewall/ag/rate-limiting-overview" target="_blank">Azure</a> 규칙 제한 기능입니다.</td>
            <td>선호하는 솔루션</td>
        </tr>
    </tbody>
</table>

## 사고 후 분석 및 지속적인 개선

DoS/DDoS 공격을 식별하고 방지하는 단일 크기 표준 흐름은 없지만 조직의 보안 프로세스에 따라 다릅니다. 다음 **사고 후 분석 및 지속적인 개선** 이것은 이 과정에서 중요한 단계이다. 다음은 고려해야 할 몇 가지 모범 사례입니다.

- 사고 후 분석을 수행하여 로그, 네트워크 트래픽 및 시스템 구성을 검토하여 DoS/DDoS 공격의 근본 원인을 파악합니다.
- 사고 후 분석 결과를 토대로 예방 메커니즘을 개선한다.

