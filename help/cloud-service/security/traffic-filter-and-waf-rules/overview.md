---
title: 개요 - AEM 웹 사이트 보호
description: AEM as a Cloud Service의 Web Application Firewall(WAF) 규칙의 하위 범주를 포함하여 트래픽 필터 규칙을 사용하여 DoS, DDoS 및 악성 트래픽으로부터 AEM 웹 사이트를 보호하는 방법에 대해 알아봅니다.
version: Experience Manager as a Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2025-06-04T00:00:00Z
jira: KT-13148
thumbnail: null
source-git-commit: 293157c296676ef1496e6f861ed8c2c24da7e068
workflow-type: tm+mt
source-wordcount: '1185'
ht-degree: 1%

---


# 개요 - AEM 웹 사이트 보호

AEM as a Cloud Service의 **Web Application Firewall(WAF)** 규칙의 하위 범주를 포함하여 **트래픽 필터 규칙**&#x200B;을 사용하여 DoS(서비스 거부), DDoS(분산 서비스 거부), 악의적인 트래픽 및 정교한 공격으로부터 AEM 웹 사이트를 보호하는 방법에 대해 알아봅니다.

또한 표준 트래픽 필터와 WAF 트래픽 필터 규칙 간의 차이점, 사용 시기 및 Adobe 권장 규칙을 시작하는 방법에 대해 알아봅니다.

>[!IMPORTANT]
>
> WAF 트래픽 필터 규칙에는 추가 **WAF-DDoS 보호** 또는 **향상된 보안** 라이선스가 필요합니다. 표준 트래픽 필터 규칙은 기본적으로 Sites 및 Forms 고객이 사용할 수 있습니다.

## AEM as a Cloud Service의 트래픽 보안 소개

AEM as a Cloud Service은 통합 CDN 계층을 활용하여 웹 사이트 전달을 보호하고 최적화합니다. CDN 계층의 가장 중요한 구성 요소 중 하나는 트래픽 규칙을 정의하고 적용하는 기능입니다. 이러한 규칙은 성능 저하 없이 사이트를 남용, 오용 및 공격으로부터 보호하는 보호의 역할을 합니다.

트래픽 보안은 가동 시간을 유지하고 중요한 데이터를 보호하며 합법적인 사용자를 위한 원활한 경험을 보장하기 위해 필수적입니다. AEM은 두 가지 보안 규칙 범주를 제공합니다.

- **표준 트래픽 필터 규칙**
- **웹 응용 프로그램 방화벽(WAF) 트래픽 필터 규칙**

규칙 세트는 고객이 일반적이고 정교한 웹 위협을 방지하고, 악의적이거나 잘못된 클라이언트의 소음을 줄이고, 요청 로깅, 차단 및 패턴 감지를 통해 가시성을 향상시키는 데 도움이 됩니다.

## 표준 및 WAF 트래픽 필터 규칙의 차이점

| 기능 | 표준 트래픽 필터 규칙 | WAF 트래픽 필터 규칙 |
|--------------------------|--------------------------------------------------|---------------------------------------------------------|
| 용도 | DoS, DDoS, 스크래핑 또는 보트 활동과 같은 남용 방지 | 봇으로부터 보호하는 정교한 공격 패턴(예: OWASP Top 10)을 감지하고 대응 |
| 예 | 속도 제한, 지역 차단, 사용자 에이전트 필터링 | SQL 주입, XSS, 알려진 공격 IP |
| 유연성 | YAML을 통해 구성 가능성이 높음 | 사전 정의된 WAF 플래그를 사용하여 YAML을 통해 구성 가능성이 높음 |
| 권장 모드 | `log` 모드로 시작한 다음 `block` 모드로 이동합니다. | `block` WAF 플래그의 경우 `ATTACK-FROM-BAD-IP` 모드로 시작하고 `log` WAF 플래그의 경우 `ATTACK` 모드로 이동한 다음 두 가지 모두에 대해 `block` 모드로 이동합니다. |
| 배포 | YAML에서 정의되고 Cloud Manager 구성 파이프라인을 통해 배포됩니다. | `wafFlags`을(를) 사용하여 YAML에서 정의되며 Cloud Manager 구성 파이프라인을 통해 배포됩니다. |
| 라이선스 | Sites 및 Forms 라이선스에 포함 | **WAF-DDoS 보호 또는 향상된 보안 라이선스가 필요합니다** |

표준 트래픽 필터 규칙은 속도 제한 또는 특정 영역 차단과 같은 비즈니스별 정책을 적용하고 IP 주소, 경로 또는 사용자 에이전트와 같은 요청 속성 및 헤더를 기반으로 트래픽을 차단하는 데 유용합니다.
반면 WAF 트래픽 필터 규칙은 알려진 웹 악용 및 공격 벡터에 대한 포괄적인 사전 보호를 제공하며, 가양성(즉, 합법적인 트래픽 차단)을 제한하는 고급 인텔리전스를 보유하고 있습니다.
두 유형의 규칙을 모두 정의하려면 YAML 구문을 사용합니다. 자세한 내용은 [트래픽 필터 규칙 구문](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#rules-syntax)을 참조하십시오.

## 사용 시기 및 이유

다음과 같은 경우 **표준 트래픽 필터 규칙 사용**:

- IP 속도 조절과 같은 조직별 제한을 적용할 수 있습니다.
- 필터링이 필요한 특정 패턴(예: 악의적인 IP 주소, 영역, 헤더)은 알고 있습니다.

다음과 같은 경우 **WAF 트래픽 필터 규칙 사용**:

- 널리 알려진 공격 패턴(예: 주입, 프로토콜 남용)과 전문가 데이터 소스에서 수집된 알려진 악의적인 IP로부터 포괄적인 **사전 보호**&#x200B;를 원합니다.
- 합법적인 트래픽을 차단할 가능성을 제한하면서 악의적인 요청을 거부하려고 합니다.
- 간단한 구성 규칙을 적용하여 일반적이고 정교한 위협으로부터 방어하기 위한 노력의 양을 제한하려는 경우

이러한 규칙을 함께 사용하면 AEM as a Cloud Service 고객이 디지털 속성을 보호할 때 사전 예방적 조치와 사후 조치를 모두 취할 수 있는 심층 방어 전략을 제공합니다.

## Adobe 권장 규칙

Adobe은 AEM 사이트를 빠르게 보호하는 데 도움이 되는 표준 트래픽 필터 및 WAF 트래픽 필터 규칙에 대한 권장 규칙을 제공합니다.

- **표준 트래픽 필터 규칙**(기본적으로 사용 가능): **CDN 에지**, **원본** 또는 승인된 국가의 트래픽에 대한 DoS, DDoS 및 봇 공격과 같은 일반적인 남용 시나리오를 해결합니다.\
  예를 들면 다음과 같습니다.
   - CDN 에지&#x200B;_에서 초당 500개 이상의 요청을 만드는 속도 제한 IP_
   - 원본에서 _초당 100개가 넘는 요청을 만드는 속도 제한 IP_
   - OFAC(Office of Foreign Assets Control)에 의해 나열된 국가로부터의 트래픽 차단

- **WAF 트래픽 필터 규칙**(추가 라이선스 필요): SQL 삽입, XSS(교차 사이트 스크립팅) 및 기타 웹 애플리케이션 공격과 같은 [OWASP Top Ten](https://owasp.org/www-project-top-ten/) 위협을 포함하여 정교한 위협에 대한 추가 보호를 제공합니다.
예를 들면 다음과 같습니다.
   - 알려진 잘못된 IP 주소에서 요청 차단
   - 공격으로 플래그가 지정된 의심스러운 요청 로깅 또는 차단

>[!TIP]
>
> 먼저 **Adobe 권장 규칙**&#x200B;을 적용하여 Adobe의 보안 전문 지식과 지속적인 업데이트를 활용할 수 있습니다. 비즈니스에 특정 위험 또는 경계 사례가 있거나 허위 사실(합법적인 트래픽 차단)이 있는 경우 **사용자 지정 규칙**&#x200B;을 정의하거나 사용자의 요구 사항에 맞게 기본 설정을 확장할 수 있습니다.

## 시작하기

아래 설정 안내서 및 사용 사례에 따라 AEM as a Cloud Service에서 WAF 규칙을 포함한 트래픽 필터 규칙을 정의, 배포, 테스트 및 분석하는 방법을 알아봅니다. 이렇게 하면 배경 지식을 얻을 수 있으므로 나중에 Adobe 권장 규칙을 자신 있게 적용할 수 있습니다.

<!-- CARDS
{target = _self}

* ./setup.md
  {title = How to set up traffic filter rules including WAF rules}
  {description = Learn how to set up to create, deploy, test, and analyze the results of traffic filter rules including WAF rules.}
  {image = ./assets/setup/rules-setup.png}
  {cta = Start Now}
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="How to set up traffic filter rules including WAF rules">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./setup.md" title="WAF 규칙을 포함한 트래픽 필터 규칙을 설정하는 방법" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/setup/rules-setup.png" alt="WAF 규칙을 포함한 트래픽 필터 규칙을 설정하는 방법"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./setup.md" target="_self" rel="referrer" title="WAF 규칙을 포함한 트래픽 필터 규칙을 설정하는 방법">WAF 규칙을 포함한 트래픽 필터 규칙을 설정하는 방법</a>
                    </p>
                    <p class="is-size-6">WAF 규칙을 포함한 트래픽 필터 규칙의 결과를 생성, 배포, 테스트 및 분석하도록 설정하는 방법에 대해 알아봅니다.</p>
                </div>
                <a href="./setup.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">지금 시작</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->

## Adobe 권장 규칙 설정 안내서

이 안내서는 AEM as a Cloud Service 환경에서 Adobe 권장 표준 트래픽 필터 및 WAF 트래픽 필터 규칙을 설정하고 배포하는 단계별 지침을 제공합니다.

<!-- CARDS
{target = _self}

* ./use-cases/using-traffic-filter-rules.md
  {title = Protecting AEM websites using standard traffic filter rules}
  {description = Learn how to protect AEM websites from DoS, DDoS and bot abuse using Adobe-recommended standard traffic filter rules in AEM as a Cloud Service.}
  {image = ./assets/use-cases/using-traffic-filter-rules.png}
  {cta = Apply Rules}

* ./use-cases/using-waf-rules.md
  {title = Protecting AEM websites using WAF rules}
  {description = Learn how to protect AEM websites from sophisticated threats including DoS, DDoS, and bot abuse using Adobe-recommended Web Application Firewall (WAF) traffic filter rules in AEM as a Cloud Service.}
  {image = ./assets/use-cases/using-waf-rules.png}
  {cta = Activate WAF}
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Protecting AEM websites using standard traffic filter rules">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./use-cases/using-traffic-filter-rules.md" title="표준 트래픽 필터 규칙을 사용하여 AEM 웹 사이트 보호" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/use-cases/using-traffic-filter-rules.png" alt="표준 트래픽 필터 규칙을 사용하여 AEM 웹 사이트 보호"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./use-cases/using-traffic-filter-rules.md" target="_self" rel="referrer" title="표준 트래픽 필터 규칙을 사용하여 AEM 웹 사이트 보호">표준 트래픽 필터 규칙을 사용하여 AEM 웹 사이트 보호</a>
                    </p>
                    <p class="is-size-6">AEM as a Cloud Service에서 Adobe 권장 표준 트래픽 필터 규칙을 사용하여 DoS, DDoS 및 봇 남용으로부터 AEM 웹 사이트를 보호하는 방법을 알아봅니다.</p>
                </div>
                <a href="./use-cases/using-traffic-filter-rules.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">규칙 적용</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Protecting AEM websites using WAF rules">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./use-cases/using-waf-rules.md" title="WAF 규칙을 사용하여 AEM 웹 사이트 보호" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/use-cases/using-waf-rules.png" alt="WAF 규칙을 사용하여 AEM 웹 사이트 보호"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./use-cases/using-waf-rules.md" target="_self" rel="referrer" title="WAF 규칙을 사용하여 AEM 웹 사이트 보호">WAF 규칙을 사용하여 AEM 웹 사이트 보호</a>
                    </p>
                    <p class="is-size-6">AEM as a Cloud Service에서 Adobe 권장 웹 애플리케이션 방화벽(WAF) 트래픽 필터 규칙을 사용하여 DoS, DDoS 및 봇 남용을 비롯한 정교한 위협으로부터 AEM 웹 사이트를 보호하는 방법에 대해 알아봅니다.</p>
                </div>
                <a href="./use-cases/using-waf-rules.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">WAF 활성화</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->

## 고급 사용 사례

고급 시나리오의 경우 특정 비즈니스 요구 사항을 기반으로 사용자 지정 트래픽 필터 규칙을 구현하는 방법을 보여 주는 다음 사용 사례를 살펴볼 수 있습니다.

<!-- CARDS
{target = _self}

* ./how-to/request-logging.md

* ./how-to/request-blocking.md

* ./how-to/request-transformation.md
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Monitoring sensitive requests">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./how-to/request-logging.md" title="중요한 요청 모니터링" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="assets/how-to/wknd-login.png" alt="중요한 요청 모니터링"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./how-to/request-logging.md" target="_self" rel="referrer" title="중요한 요청 모니터링">중요한 요청 모니터링</a>
                    </p>
                    <p class="is-size-6">AEM as a Cloud Service에서 트래픽 필터 규칙을 사용하여 중요한 요청을 기록하여 모니터링하는 방법을 알아봅니다.</p>
                </div>
                <a href="./how-to/request-logging.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">자세히 알아보기</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Restricting access">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./how-to/request-blocking.md" title="액세스 제한" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="assets/how-to/elk-tool-dashboard-blocked.png" alt="액세스 제한"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./how-to/request-blocking.md" target="_self" rel="referrer" title="액세스 제한">액세스 제한</a>
                    </p>
                    <p class="is-size-6">AEM as a Cloud Service에서 트래픽 필터 규칙을 사용하여 특정 요청을 차단하여 액세스를 제한하는 방법에 대해 알아봅니다.</p>
                </div>
                <a href="./how-to/request-blocking.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">자세히 알아보기</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Normalizing requests">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./how-to/request-transformation.md" title="요청 표준화" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="assets/how-to/aemrequest-log-transformation.png" alt="요청 표준화"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./how-to/request-transformation.md" target="_self" rel="referrer" title="요청 표준화">요청 정규화</a>
                    </p>
                    <p class="is-size-6">AEM as a Cloud Service에서 트래픽 필터 규칙을 사용하여 요청을 변환하여 요청을 정규화하는 방법을 알아봅니다.</p>
                </div>
                <a href="./how-to/request-transformation.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">자세히 알아보기</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->

## 추가 리소스

- [WAF 규칙을 포함하는 트래픽 필터 규칙](https://experienceleague.adobe.com/ko/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf)
