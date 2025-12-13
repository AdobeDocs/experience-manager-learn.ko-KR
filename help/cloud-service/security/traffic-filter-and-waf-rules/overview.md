---
title: 개요 - AEM 웹 사이트 보호
description: AEM as a Cloud Service에서 웹 애플리케이션 방화벽(WAF) 규칙의 하위 카테고리를 포함하여 트래픽 필터 규칙을 사용하여 DoS, DDoS 및 악성 트래픽으로부터 AEM 웹 사이트를 보호하는 방법에 대해 알아봅니다.
version: Experience Manager as a Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Developer
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2025-06-04T00:00:00Z
jira: KT-13148
thumbnail: null
exl-id: e6d67204-2f76-441c-a178-a34798fe266d
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '1185'
ht-degree: 100%

---

# 개요 - AEM 웹 사이트 보호

AEM as a Cloud Service에서 **트래픽 필터 규칙**&#x200B;과 그 하위 카테고리인 **웹 애플리케이션 방화벽(WAF)** 규칙을 사용하여 서비스 거부(DoS), 분산 서비스 거부(DDoS), 악성 트래픽 및 정교한 공격으로부터 AEM 웹 사이트를 보호하는 방법에 대해 알아봅니다.

또한 표준 트래픽 필터 규칙과 WAF 트래픽 필터 규칙의 차이점, 각각의 사용 시점, Adobe에서 권장하는 규칙을 사용하여 시작하는 방법에 대해서도 확인할 수 있습니다.

>[!IMPORTANT]
>
> WAF 트래픽 필터 규칙을 사용하려면 추가 **WAF-DDoS Protection** 또는 **향상된 보안** 라이선스가 필요합니다. 표준 트래픽 필터 규칙은 기본적으로 Sites 및 Forms 고객에게 제공됩니다.


>[!VIDEO](https://video.tv.adobe.com/v/3469394/?quality=12&learn=on)

## AEM as a Cloud Service의 트래픽 보안 소개

AEM as a Cloud Service는 통합 CDN 계층을 활용하여 웹 사이트 게재를 보호하고 최적화합니다. CDN 계층의 가장 핵심적인 구성 요소 중 하나는 트래픽 규칙을 정의하고 적용하는 기능입니다. 이러한 규칙은 성능 저하 없이도 사이트를 남용, 오용 및 공격으로부터 보호하는 보호막 역할을 합니다.

트래픽 보안은 서비스 가동 시간을 유지하고, 민감한 데이터를 보호하며, 적합한 사용자의 원활한 이용 경험을 보장하는 데 필수적입니다. AEM은 두 가지 범주의 보안 규칙을 제공합니다.

- **표준 트래픽 필터 규칙**
- **웹 애플리케이션 방화벽(WAF) 트래픽 필터 규칙**

이 규칙 세트는 일반적이고 정교한 웹 위협을 방지하고, 악성 또는 오작동 클라이언트로 인한 노이즈를 줄이며, 요청 로깅, 차단 및 패턴 탐지를 통해 가시성을 향상시킵니다.

## 표준 트래픽 필터 규칙과 WAF 트래픽 필터 규칙의 차이점

| 기능 | 표준 트래픽 필터 규칙 | WAF 트래픽 필터 규칙 |
|--------------------------|--------------------------------------------------|---------------------------------------------------------|
| 용도 | DoS, DDoS, 스크래핑 또는 봇 활동과 같은 남용 방지 | 봇으로부터의 보호를 포함하여 OWASP Top 10 같은 정교한 공격 패턴 탐지 및 대응 |
| 예 | 속도 제한, 지역 차단, 사용자 에이전트 필터링 | SQL 주입, XSS, 알려진 공격 IP |
| 유연성 | YAML을 통해 높은 구성 가능 | WAF 플래그가 미리 정의된 YAML을 통해 높은 구성 가능 |
| 권장 모드 | 먼저 `log` 모드로 시작 후 `block` 모드로 전환 | `ATTACK-FROM-BAD-IP` WAF 플래그는 `block` 모드, `ATTACK` WAF 플래그는 `log` 모드로 시작 후 두 플래그 모두에 대해 `block` 모드로 전환 |
| 배포 | YAML로 정의하고 Cloud Manager 구성 파이프라인을 통해 배포 | `wafFlags`를 포함한 YAML로 정의하고 Cloud Manager 구성 파이프라인을 통해 배포 |
| 라이선스 | Sites 및 Forms 라이선스에 포함 | **WAF-DDoS 보호 또는 향상된 보안 라이선스 필요** |

표준 트래픽 필터 규칙은 속도 제한이나 특정 지역 차단과 같은 비즈니스별 정책을 시행하는 데 유용하며, IP 주소, 경로 또는 사용자 에이전트와 같은 요청 속성 및 헤더를 기반으로 트래픽을 차단하는 데도 유용합니다.
반면에 WAF 트래픽 필터 규칙은 알려진 웹 악용 및 공격 벡터에 대한 포괄적이고 사전 예방적인 보호를 제공하며, 긍정 오류(즉, 합법적인 트래픽 차단)를 제한하는 고급 인텔리전스를 갖추고 있습니다.
두 가지 유형의 규칙을 정의하려면 YAML 구문을 사용합니다. 자세한 내용은 [트래픽 필터 규칙 구문](https://experienceleague.adobe.com/ko/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#rules-syntax)을 참조하시기 바랍니다.

## 사용 시점 및 이유

**표준 트래픽 필터 규칙**&#x200B;은 다음과 같은 경우에 사용합니다.

- IP 속도 제한과 같이 조직별 제한을 적용하려는 경우
- 필터링이 필요한 특정 패턴(예: 악성 IP 주소, 지역, 헤더)을 알고 있는 경우

**WAF 트래픽 필터 규칙**&#x200B;은 다음과 같은 경우에 사용합니다.

- 전문 데이터 소스에서 수집된 알려진 악성 IP뿐만 아니라 광범위하게 알려진 공격 패턴(예: 인젝션, 프로토콜 남용)으로부터 포괄적인 **사전 예방적 보호**&#x200B;가 필요한 경우
- 합법적인 트래픽을 차단할 가능성을 제한하면서 악성 요청을 거부하려는 경우
- 간단한 구성 규칙을 적용하여 일반적이고 정교한 위협을 방어하는 데 필요한 노력을 제한하려는 경우

이러한 규칙은 함께 심층 방어 전략을 제공하여 AEM as a Cloud Service 고객이 디지털 자산을 보호하기 위한 사전 예방적 조치와 사후 대응적 조치를 모두 취할 수 있도록 합니다.

## Adobe 권장 규칙

Adobe는 AEM Sites를 신속하게 보호할 수 있도록 표준 트래픽 필터 및 WAF 트래픽 필터 규칙에 대한 권장 규칙을 제공합니다.

- **표준 트래픽 필터 규칙**(기본적으로 사용 가능): DoS, DDoS 및 봇 공격과 같은 일반적인 남용 시나리오를 **CDN Edge**, **원본** 또는 제재 대상 국가의 트래픽으로부터 방어합니다.\
  해당 예는 다음과 같습니다.
   - _CDN Edge에서_ 초당 500개 이상의 요청을 생성하는 IP에 대한 속도 제한
   - _원본에서_ 초당 100개 이상의 요청을 생성하는 IP에 대한 속도 제한
   - 해외 자산 통제국(OFAC)에 의해 지정된 국가의 트래픽 차단

- **WAF 트래픽 필터 규칙**(추가 라이선스 필요): SQL 주입, XSS(크로스 사이트 스크립팅) 및 기타 웹 애플리케이션 공격과 같은 [상위 10개 OWASP](https://owasp.org/www-project-top-ten/) 위협을 포함한 정교한 위협에 대한 추가 보호를 제공합니다.
해당 예는 다음과 같습니다.
   - 알려진 잘못된 IP 주소의 요청 차단
   - 공격으로 플래그 지정된 의심스러운 요청 로깅 또는 차단

>[!TIP]
>
> Adobe의 보안 전문 지식과 지속적인 업데이트를 활용하려면 **Adobe 권장 규칙**&#x200B;을 적용하여 시작하시기 바랍니다. 비즈니스에 특정 위험이나 극단적 사례가 있거나 긍정 오류(합법적인 트래픽 차단)가 발생하는 경우, 필요에 따라 **사용자 정의 규칙**&#x200B;을 정의하거나 기본 설정을 확장할 수 있습니다.

## 시작하기

아래의 설정 안내서 및 사용 사례를 따라 AEM as a Cloud Service에서 WAF 규칙을 포함한 트래픽 필터 규칙을 정의하고, 배포하고, 테스트하고, 분석하는 방법에 대해 알아봅니다. 이를 통해 나중에 Adobe 권장 규칙을 자신 있게 적용할 수 있는 배경 지식을 얻을 수 있습니다.

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
                    <p class="is-size-6">WAF 규칙을 포함한 트래픽 필터 규칙의 결과를 생성, 배포, 테스트 및 분석하는 방법을 알아봅니다.</p>
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

이 안내서는 AEM as a Cloud Service 환경에서 Adobe 권장 표준 트래픽 필터 및 WAF 트래픽 필터 규칙을 설정하고 배포하기 위한 단계별 지침을 제공합니다.

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
                    <a href="./use-cases/using-traffic-filter-rules.md" title="표준 트래픽 필터 규칙을 사용한 AEM 웹 사이트 보호" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/use-cases/using-traffic-filter-rules.png" alt="표준 트래픽 필터 규칙을 사용한 AEM 웹 사이트 보호"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./use-cases/using-traffic-filter-rules.md" target="_self" rel="referrer" title="표준 트래픽 필터 규칙을 사용한 AEM 웹 사이트 보호">표준 트래픽 필터 규칙을 사용한 AEM 웹 사이트 보호</a>
                    </p>
                    <p class="is-size-6">AEM as a Cloud Service에서 Adobe 권장 표준 트래픽 필터 규칙을 사용하여 DoS, DDoS 및 봇 남용으로부터 AEM 웹 사이트를 보호하는 방법에 대해 알아봅니다.</p>
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
                    <p class="is-size-6">AEM as a Cloud Service에서 Adobe 권장 웹 애플리케이션 방화벽(WAF) 트래픽 필터 규칙을 사용하여 DoS, DDoS 및 봇 남용을 포함한 정교한 위협으로부터 AEM 웹 사이트를 보호하는 방법에 대해 알아봅니다.</p>
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

더 고급 시나리오의 경우, 특정 비즈니스 요구 사항에 따라 사용자 정의 트래픽 필터 규칙을 구현하는 방법을 보여 주는 다음 사용 사례를 살펴볼 수 있습니다.

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
                    <a href="./how-to/request-logging.md" title="민감한 요청 모니터링" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="assets/how-to/wknd-login.png" alt="민감한 요청 모니터링"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./how-to/request-logging.md" target="_self" rel="referrer" title="민감한 요청 모니터링">민감한 요청 모니터링</a>
                    </p>
                    <p class="is-size-6">AEM as a Cloud Service에서 트래픽 필터 규칙을 사용하여 민감한 요청을 로깅하여 모니터링하는 방법에 대해 알아봅니다.</p>
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
                        <a href="./how-to/request-transformation.md" target="_self" rel="referrer" title="요청 표준화">요청 표준화</a>
                    </p>
                    <p class="is-size-6">AEM as a Cloud Service에서 트래픽 필터 규칙을 사용하여 요청을 변환하여 정규화하는 방법에 대해 알아봅니다.</p>
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

- [WAF 규칙이 포함된 트래픽 필터 규칙](https://experienceleague.adobe.com/ko/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf)
